
---


Yes — let's continue the **previous Session Auth API** from exactly where we stopped.

The next step is:

# Step 14 — Admin Self-Protection

The problem is now:

```text
Admin A
   ↓
PATCH /api/users/AdminA_ID/role
   ↓
role = "user"
```

or:

```text
Admin A
   ↓
DELETE /api/users/AdminA_ID
```

If we allow both, an admin could accidentally remove their own administrative access — or, worse, delete the **only admin account** and leave the system with no administrator.

We're going to add safeguards.

---

# 1. Decide the security rules

For our learning project, let's use these rules:

### Rule 1 — Admin cannot change their own role

```text
Admin → PATCH /users/:own-id/role
          ↓
        403
```

This prevents:

```text
admin → user
```

through the admin role-management endpoint.

### Rule 2 — Admin cannot delete their own account

```text
Admin → DELETE /users/:own-id
          ↓
        403
```

This completely avoids accidental self-lockout.

### Rule 3 — Admin can still manage other users

```text
Admin A → change User B role ✅
Admin A → delete User B ✅
```

### Rule 4 — Normal users still cannot use these routes

```text
user → /users/:id/role → 403
user → DELETE /users/:id → 403
```

---

# 2. Why should this be enforced in the controller?

We already have:

```text
authenticate
    ↓
req.user
    ↓
authorize("admin")
    ↓
controller
```

By the time the controller runs, we know:

```text
req.user.role = "admin"
```

Now the controller can compare:

```text
req.user._id
      vs
req.params.id
```

This is a **business rule specific to the operation**, so it's reasonable to enforce it in the controller.

---

# 3. Protect `updateUserRole`

Your role-update controller is conceptually:

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
      data: { user: updatedUser },
    });
  } catch (error) {
    next(error);
  }
};
```

Add the self-check **before** the database update:

```js
const updateUserRole = async (req, res, next) => {
  try {
    const { role } = req.body;

    if (req.user._id.toString() === req.params.id) {
      return next(
        new ApiError(
          403,
          "You cannot change your own role",
        ),
      );
    }

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
      data: { user: updatedUser },
    });
  } catch (error) {
    next(error);
  }
};
```

---

# 4. Why compare IDs like this?

You have:

```js
req.user._id
```

and:

```js
req.params.id
```

The first is a MongoDB `ObjectId`.

The second is a string.

So:

```js
req.user._id === req.params.id
```

isn't the comparison we want.

Instead:

```js
req.user._id.toString() === req.params.id
```

gives:

```text
"64abc123" === "64abc123"
```

---

# 5. Now protect self-deletion

Your existing delete controller is approximately:

```js
const deleteUser = async (req, res, next) => {
  try {
    const deletedUser = await User.findByIdAndDelete(req.params.id);

    if (!deletedUser) {
      return next(new ApiError(404, "User not found"));
    }

    res.status(200).json({
      status: "success",
      message: "user deleted successfully",
      data: { deletedUser },
    });
  } catch (error) {
    next(error);
  }
};
```

Add:

```js
if (req.user._id.toString() === req.params.id) {
  return next(
    new ApiError(
      403,
      "You cannot delete your own account from the admin user-management endpoint",
    ),
  );
}
```

So:

```js
const deleteUser = async (req, res, next) => {
  try {
    if (req.user._id.toString() === req.params.id) {
      return next(
        new ApiError(
          403,
          "You cannot delete your own account from the admin user-management endpoint",
        ),
      );
    }

    const deletedUser = await User.findByIdAndDelete(req.params.id);

    if (!deletedUser) {
      return next(new ApiError(404, "User not found"));
    }

    res.status(200).json({
      status: "success",
      message: "user deleted successfully",
      data: { deletedUser },
    });
  } catch (error) {
    next(error);
  }
};
```

---

# 6. What about the "only admin" problem?

With the rules above, we've solved it automatically.

Imagine:

```text
Database:

Admin A
role = admin

User B
role = user
```

Admin A tries:

```text
PATCH own role → ❌
DELETE own account → ❌
```

Therefore:

```text
Admin A
   ↓
cannot remove their own admin access
```

So there is no path through these admin-management endpoints that can leave the system without an admin because the only admin accidentally removed themselves.

---

# 7. But what if there are two admins?

Suppose:

```text
Admin A
Admin B
User C
```

Admin A still cannot:

```text
change own role ❌
delete self ❌
```

but can:

```text
change B's role ✅
delete B ✅
```

That means Admin A could still intentionally remove Admin B, leaving only themselves.

That's okay for this learning project because **the actor who performed the operation is still an admin**.

The more advanced business rule would be:

> "An admin cannot demote/delete the last remaining admin."

That's another legitimate design, but it's a separate rule.

---

# 8. Advanced version: prevent deleting the last admin

This is worth understanding because you specifically mentioned the "only admin" edge case.

Suppose Admin A tries to demote Admin B:

```text
Admin A
Admin B
```

If Admin B is the **last other admin**, we should potentially prevent:

```text
Admin B → user
```

because then:

```text
Admin A
```

might still be there, so that's fine.

But if Admin A were demoting **the only other admin**, Admin A remains admin, so there's no danger.

The real dangerous situation is:

```text
Only Admin A exists
```

and Admin A tries to:

```text
delete Admin A
```

or:

```text
change Admin A → user
```

which our self-protection already blocks.

So for our project, **self-protection is enough**.

---

# 9. Test this in Postman

Use an admin account.

### Test 1 — Admin changes another user's role

```http
PATCH /api/users/USER_B_ID/role
```

```json
{
  "role": "admin"
}
```

Expected:

```text
200 ✅
```

---

### Test 2 — Admin changes their own role

```http
PATCH /api/users/ADMIN_ID/role
```

```json
{
  "role": "user"
}
```

Expected:

```text
403 ❌
```

Response:

```json
{
  "status": "fail",
  "statusCode": 403,
  "message": "You cannot change your own role"
}
```

---

### Test 3 — Admin deletes another user

```http
DELETE /api/users/USER_B_ID
```

Expected:

```text
200 ✅
```

---

### Test 4 — Admin deletes themselves

```http
DELETE /api/users/ADMIN_ID
```

Expected:

```text
403 ❌
```

---

### Test 5 — Normal user tries role management

```http
PATCH /api/users/ADMIN_ID/role
```

Expected:

```text
403 ❌
```

because:

```text
authenticate ✅
authorize("admin") ❌
```

---

# 10. The architectural lesson

We've now added a new layer:

```text
Authentication
      ↓
"Who are you?"

Role Authorization
      ↓
"Are you an admin?"

Operation-specific rule
      ↓
"Can this admin perform this particular action?"
```

So authorization isn't always just:

```js
authorize("admin")
```

Sometimes you need **business rules after the role check**.

For example:

```text
Admin
  ↓
Can manage users
  ↓
BUT
  ↓
Cannot manage themselves through certain operations
```

That's much closer to real backend authorization.

---

# Step 14 checklist

```text
[ ] Prevent admin from changing own role
[ ] Prevent admin from deleting own account
[ ] Admin can change other users' roles
[ ] Admin can delete other users
[ ] Normal user still gets 403
[ ] Test with only one admin
[ ] Verify admin remains admin
```

After this, your Session Auth API has covered the core learning goals. The next useful step should be a **final security audit of the whole API**, where we inspect things like password changes, session fixation, cookie settings, user deletion vs task cleanup, and authorization gaps before you close the project and move fully to JWT.