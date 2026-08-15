
---

Yes buddy. **Step 12 is done** ✅

We are continuing the **same project and same architecture**:

- Phase 1 → Task/User CRUD ✅
    
- Phase 2 → Authentication + Authorization
    
- `express-session` + MongoDB session store
    
- **No custom Session model**
    
- `req.session.userId` → authentication
    
- `req.user` → authenticated user
    
- Ownership authorization ✅
    
- Role-based authorization ✅
    
- Logout/session destruction ✅
    

So now we move forward.

# Step 13 — Protect the User Routes Properly

We have authentication and authorization working. Now we need to apply them correctly to our **User API**.

This is where we start thinking about:

> **Who is allowed to perform which operation?**

---

## 1. Look at our current User routes

From Phase 1 we have:

```text
GET     /api/users
POST    /api/users
GET     /api/users/:id
PATCH   /api/users/:id
DELETE  /api/users/:id
```

But now we have authentication and roles.

So we should **not leave these routes public**.

Currently, someone could potentially do:

```text
GET /api/users
```

without even logging in.

That's not what we want.

---

# 2. Decide who can access User routes

Our project has:

```text
user
admin
```

We want:

|Endpoint|Who can access?|
|---|---|
|`POST /api/users`|Public registration is handled by `/api/auth/register`|
|`GET /api/users`|Admin|
|`GET /api/users/:id`|Admin|
|`PATCH /api/users/:id`|Admin / later possibly self|
|`DELETE /api/users/:id`|Admin|

Notice something important:

We already have:

```text
POST /api/auth/register
```

So we **don't need**:

```text
POST /api/users
```

for normal registration anymore.

---

# 3. First remove public user creation

Your `user.route.js` currently has:

```js
router
  .route("/")
  .get(getAllUsers)
  .post(createUserValidator, validate, createUser);
```

We no longer need:

```js
.post(createUserValidator, validate, createUser)
```

because registration is now:

```text
POST /api/auth/register
```

So change:

```js
router
  .route("/")
  .get(getAllUsers);
```

---

# 4. Protect `GET /api/users`

We want:

```text
GET /api/users
```

to be **admin only**.

Your route should eventually look like:

```js
router
  .route("/")
  .get(authenticateUser, authorize("admin"), getAllUsers);
```

The flow becomes:

```text
GET /api/users
       ↓
authenticateUser
       ↓
Is session valid?
       ↓
req.user
       ↓
authorize("admin")
       ↓
Is req.user.role === "admin"?
       ↓
      YES
       ↓
getAllUsers
```

---

# 5. What happens if a normal user tries?

Suppose:

```text
req.user.role = "user"
```

Request:

```text
GET /api/users
```

Authentication succeeds:

```text
User is logged in ✅
```

But authorization fails:

```text
role !== admin
```

Therefore:

```text
403 Forbidden
```

This demonstrates the difference between:

### Authentication

> "Who are you?"

and:

### Authorization

> "Are you allowed to do this?"

---

# 6. Protect `GET /api/users/:id`

Same principle.

Change:

```js
router
  .route("/:id")
  .get(validateObjectId, getUser)
```

to:

```js
router
  .route("/:id")
  .get(
    authenticateUser,
    authorize("admin"),
    validateObjectId,
    getUser
  );
```

The order is important.

Conceptually:

```text
Request
  ↓
Authentication
  ↓
Authorization
  ↓
Validate ID
  ↓
Controller
```

---

# 7. Protect PATCH

Your current route:

```js
.patch(
  validateObjectId,
  updateUserValidator,
  validate,
  updateUser
)
```

Now we need authentication + authorization:

```js
.patch(
  authenticateUser,
  authorize("admin"),
  validateObjectId,
  updateUserValidator,
  validate,
  updateUser
)
```

So only an admin can update users.

---

# 8. Protect DELETE

Current:

```js
.delete(validateObjectId, deleteUser);
```

Change to:

```js
.delete(
  authenticateUser,
  authorize("admin"),
  validateObjectId,
  deleteUser
);
```

Now:

```text
DELETE /api/users/:id
```

requires:

```text
valid session
      +
admin role
```

---

# 9. Complete `user.route.js`

After Step 13, your route should look approximately like this:

```js
import express from "express";

import validateObjectId from "../middlewares/validateObjectId.middleware.js";
import validate from "../middlewares/validate.middleware.js";
import updateUserValidator from "../validators/updateUser.validator.js";

import authenticateUser from "../middlewares/authenticate.middleware.js";
import authorize from "../middlewares/authorize.middleware.js";

import {
  getAllUsers,
  getUser,
  updateUser,
  deleteUser,
} from "../controllers/user.controller.js";

const router = express.Router();

router
  .route("/")
  .get(
    authenticateUser,
    authorize("admin"),
    getAllUsers
  );

router
  .route("/:id")
  .get(
    authenticateUser,
    authorize("admin"),
    validateObjectId,
    getUser
  )
  .patch(
    authenticateUser,
    authorize("admin"),
    validateObjectId,
    updateUserValidator,
    validate,
    updateUser
  )
  .delete(
    authenticateUser,
    authorize("admin"),
    validateObjectId,
    deleteUser
  );

export default router;
```

**Use your actual middleware filenames/names** if they differ. The important thing is the flow, not the exact filename.

---

# 10. Why registration is now under `/auth`

We now have a cleaner separation:

```text
AUTH
────────────────────

POST /api/auth/register
POST /api/auth/login
POST /api/auth/logout
GET  /api/auth/me
```

versus:

```text
USERS
────────────────────

GET    /api/users
GET    /api/users/:id
PATCH  /api/users/:id
DELETE /api/users/:id
```

Think of it like:

```text
/auth
   ↓
Authentication operations

/users
   ↓
User resource management
```

That's a much more realistic API design.

---

# 11. Test Step 13 carefully

We need **two users**:

```text
User A → role: user
User B → role: admin
```

You can change one user's role directly in MongoDB for learning/testing.

### Test 1 — Not logged in

```http
GET /api/users
```

Expected:

```text
401 Unauthorized
```

---

### Test 2 — Logged in as normal user

```http
GET /api/users
```

Expected:

```text
403 Forbidden
```

Why?

```text
Authentication ✅
Authorization ❌
```

---

### Test 3 — Logged in as admin

```http
GET /api/users
```

Expected:

```text
200 OK
```

---

### Test 4 — Normal user tries DELETE

```http
DELETE /api/users/:id
```

Expected:

```text
403 Forbidden
```

---

### Test 5 — Admin deletes user

```http
DELETE /api/users/:id
```

Expected:

```text
200 OK
```

---

# 12. The important lesson of Step 13

Before Phase 2:

```text
Request
   ↓
Controller
```

Now:

```text
Request
   ↓
Authentication
   ↓
Authorization
   ↓
Validation
   ↓
Controller
   ↓
Database
```

And different routes can have different security requirements.

For example:

```text
/auth/register
     ↓
Public
```

```text
/auth/me
     ↓
Authentication
```

```text
/users
     ↓
Authentication
     ↓
Admin authorization
```

```text
/tasks/:id
     ↓
Authentication
     ↓
Ownership authorization
```

This is the architecture we're trying to make second nature.

---

## Step 13 checklist

```text
□ Remove POST /api/users registration route
□ Registration remains /api/auth/register

□ Protect GET /api/users
□ Protect GET /api/users/:id
□ Protect PATCH /api/users/:id
□ Protect DELETE /api/users/:id

□ Test without login → 401
□ Test as normal user → 403
□ Test as admin → 200

□ Understand authentication vs authorization
□ Understand why middleware order matters
```

**Implement and test Step 13.** After that, we'll move to the next part of the project rather than adding random features.