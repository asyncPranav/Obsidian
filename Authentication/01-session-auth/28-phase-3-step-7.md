
---

Exactly. 👍 **This is the next logical authorization feature.**

Right now you have:

- Authentication ✅
    
- Session authentication ✅
    
- Logout/session invalidation ✅
    
- Task ownership authorization ✅
    
- Role-based authorization ✅
    
- Admin can access admin-only routes ✅
    
- Normal users cannot access admin-only routes ✅
    

Now we need to solve:

> **How can an admin promote a normal user from `user` → `admin`?**

And importantly, **normal users must never be able to change their own role.**

# Step 13 — Admin Role Management

Our goal:

```text
Admin
  │
  ├── GET /api/users
  │
  └── PATCH /api/users/:id/role
                         │
                         ▼
                    Change role
```

A normal user:

```text
User
  │
  └── PATCH /api/users/:id/role
              ↓
         ❌ 403 Forbidden
```

---

## 1. Why should we make a separate route?

Your existing user update route probably looks roughly like:

```http
PATCH /api/users/:id
```

We **should not simply add `role` to `updateUserValidator`**.

Why?

Because then we'd potentially mix:

```text
Normal profile information
        +
Sensitive authorization information
```

For example:

```json
{
  "name": "Pranav",
  "email": "new@email.com",
  "role": "admin"
}
```

We don't want a normal user to ever have the ability to do this.

Instead, make role management an explicitly privileged operation:

```http
PATCH /api/users/:id/role
```

That makes the intention very clear.

---

# 2. Desired request flow

The route should be:

```text
PATCH /api/users/:id/role
              │
              ▼
      validateObjectId
              │
              ▼
        authenticate
              │
              ▼
      authorize("admin")
              │
              ▼
      validate role input
              │
              ▼
      updateUserRole()
              │
              ▼
          MongoDB
```

Notice the important ordering:

```text
authenticate
      ↓
authorize("admin")
```

Why?

Because `authorize()` uses:

```js
req.user.role
```

And `req.user` is created by:

```js
authenticate
```

So this would be WRONG:

```js
authorize("admin"),
authenticate,
```

because `req.user` doesn't exist yet.

---

# 3. Create role validator

Create:

```text
src/validators/updateRole.validator.js
```

Code:

```js
import { body } from "express-validator";

const updateRoleValidator = [
  body("role")
    .notEmpty()
    .withMessage("Role is required")
    .isIn(["user", "admin"])
    .withMessage("Role must be either user or admin"),
];

export default updateRoleValidator;
```

This means the client can only send:

```json
{
  "role": "admin"
}
```

or:

```json
{
  "role": "user"
}
```

Anything else:

```json
{
  "role": "superadmin"
}
```

gets rejected.

---

# 4. Add controller

Open:

```text
src/controllers/user.controller.js
```

Add:

```js
const updateUserRole = async (req, res, next) => {
  try {
    const { role } = req.body;

    const updatedUser = await User.findByIdAndUpdate(
      req.params.id,
      { role },
      {
        new: true,
        runValidators: true,
      },
    ).select("-password");

    if (!updatedUser) {
      return next(new ApiError(404, "User not found"));
    }

    res.status(200).json({
      status: "success",
      message: "User role updated successfully",
      data: {
        user: updatedUser,
      },
    });
  } catch (error) {
    next(error);
  }
};
```

Then add it to your exports:

```js
export {
  getAllUsers,
  getUser,
  createUser,
  updateUser,
  deleteUser,
  updateUserRole,
};
```

---

# 5. Add route

Now open:

```text
src/routes/user.route.js
```

Import:

```js
import authorize from "../middlewares/authorize.middleware.js";
```

Import the new validator:

```js
import updateRoleValidator from "../validators/updateRole.validator.js";
```

And import the controller:

```js
import {
  getAllUsers,
  getUser,
  createUser,
  updateUser,
  deleteUser,
  updateUserRole,
} from "../controllers/user.controller.js";
```

Then add:

```js
router.patch(
  "/:id/role",
  authenticate,
  authorize("admin"),
  validateObjectId,
  updateRoleValidator,
  validate,
  updateUserRole,
);
```

So your user routes will conceptually become:

```js
router
  .route("/")
  .get(authenticate, authorize("admin"), getAllUsers)
  .post(createUserValidator, validate, createUser);

router
  .route("/:id")
  .get(authenticate, authorize("admin"), validateObjectId, getUser)
  .patch(
    authenticate,
    authorize("admin"),
    validateObjectId,
    updateUserValidator,
    validate,
    updateUser,
  )
  .delete(
    authenticate,
    authorize("admin"),
    validateObjectId,
    deleteUser,
  );

router.patch(
  "/:id/role",
  authenticate,
  authorize("admin"),
  validateObjectId,
  updateRoleValidator,
  validate,
  updateUserRole,
);
```

### One important point

Because this route is:

```text
/:id/role
```

it should be defined **before**:

```text
/:id
```

or at least be arranged carefully so the generic `/:id` route doesn't interfere with routing behavior.

---

# 6. Test with Postman

Suppose we have:

```text
User A
role = admin

User B
role = user
```

Login as User A.

Then:

```http
PATCH /api/users/<User-B-ID>/role
```

Body:

```json
{
  "role": "admin"
}
```

Expected:

```json
{
  "status": "success",
  "message": "User role updated successfully"
}
```

MongoDB:

```text
User B
   ↓
role: "admin"
```

Now User B is an admin.

---

# 7. Test the reverse

Admin can also demote:

```http
PATCH /api/users/<User-B-ID>/role
```

```json
{
  "role": "user"
}
```

Expected:

```text
role → user
```

---

# 8. Very important security test

Now login as a normal user.

Try:

```http
PATCH /api/users/<some-user-id>/role
```

with:

```json
{
  "role": "admin"
}
```

The request reaches:

```text
authenticate
     ↓
req.user
     ↓
authorize("admin")
     ↓
req.user.role === "user"
     ↓
❌ 403 Forbidden
```

So the user **cannot promote themselves**.

---

# 9. Notice what we've achieved

We now have two completely different concepts:

### Authentication

```text
"Who are you?"
```

Handled by:

```js
authenticate
```

which gives us:

```js
req.user
```

### Authorization

```text
"Are you allowed to do this?"
```

Handled by:

```js
authorize("admin")
```

which checks:

```js
req.user.role
```

Therefore:

```text
Request
   ↓
Session
   ↓
authenticate
   ↓
req.user
   ↓
authorize("admin")
   ↓
Controller
```

This is the architecture I want you to really understand.

---

## Step 13 checklist

```text
□ Create updateRole.validator.js
□ Validate role = "user" or "admin"
□ Create updateUserRole controller
□ Don't add role to normal updateUserValidator
□ Add PATCH /:id/role
□ Protect route with authenticate
□ Protect route with authorize("admin")
□ Test admin → user
□ Test user → admin
□ Test normal user attempting role change → 403
□ Verify role changed in MongoDB
```

**One security improvement we'll address after this:** whether an admin should be allowed to accidentally demote/delete **themselves**, especially if they're the only admin. That's a real authorization edge case worth learning once this basic role-management flow works.