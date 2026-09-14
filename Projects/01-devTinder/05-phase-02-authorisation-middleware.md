

----


# Authorization Middleware — Industry Notes

### Purpose

This middleware implements **authorization** after authentication.

```text
Authentication → Who are you?
Authorization  → What are you allowed to do?
```

`req.user` is expected to be populated by the authentication middleware before these middleware functions run.

---

## 1. `requireRole(...allowedRoles)`

### Purpose

Checks whether the authenticated user's **role** is permitted to access a route.

```js
requireRole("admin")
requireRole("admin", "moderator")
```

### Logic

```text
Request
   ↓
Is req.user available?
   ├── No → 401 Unauthorized
   ↓ Yes
Is req.user.role in allowedRoles?
   ├── No → 403 Forbidden
   ↓ Yes
next()
```

### Key code

```js
allowedRoles.includes(req.user.role)
```

`...allowedRoles` is a **rest parameter**, so:

```js
requireRole("admin", "moderator")
```

becomes:

```js
allowedRoles = ["admin", "moderator"]
```

Then `includes()` checks whether the user's role exists in that array.

### Example

```js
router.delete(
  "/users/:id",
  authenticate,
  requireRole("admin"),
  deleteUser
);
```

Only an authenticated user whose role is `admin` reaches `deleteUser`.

### Concept

**RBAC — Role-Based Access Control**

```text
User → Role → Permission
```

---

# 2. `requireOwnership(paramIdField)`

### Purpose

Ensures that the authenticated user is the **owner of the resource** identified by a route parameter.

Example:

```js
requireOwnership("userId")
```

For:

```http
PATCH /api/users/:userId
```

the middleware compares:

```text
Authenticated user's ID
        ↓
    req.user._id

Resource owner's ID
        ↓
    req.params.userId
```

### Logic

```text
Request
   ↓
Is req.user available?
   ├── No → 401 Unauthorized
   ↓ Yes
Read req.params[paramIdField]
   ↓
Does the parameter exist?
   ├── No → 400 Bad Request
   ↓ Yes
Does user ID == resource user ID?
   ├── No → 403 Forbidden
   ↓ Yes
next()
```

### Key comparison

```js
req.user._id.toString() === resourceUserId
```

`req.user._id` is a MongoDB `ObjectId`, while URL parameters are strings, so `.toString()` normalizes the ID before comparison.

---

# Why `paramIdField` is passed as an argument

Instead of hardcoding:

```js
req.params.userId
```

the middleware is reusable:

```js
requireOwnership("userId")
requireOwnership("ownerId")
requireOwnership("profileId")
```

Internally:

```js
req.params[paramIdField]
```

means:

```js
req.params["userId"]
```

or whichever parameter name was supplied.

---

# 401 vs 403 vs 400

|Status|Meaning|In this middleware|
|---|---|---|
|**401**|Not authenticated|`req.user` doesn't exist|
|**403**|Authenticated but not permitted|Wrong role / not resource owner|
|**400**|Invalid or incomplete request|Required route parameter missing|

### Important distinction

```text
401 → "I don't know who you are."

403 → "I know who you are, but you're not allowed."

400 → "Your request itself is invalid/incomplete."
```

---

# Middleware execution order

These authorization middleware should normally run **after authentication**:

```text
Request
   ↓
Route
   ↓
Authentication Middleware
   ↓
req.user created
   ↓
Authorization Middleware
   ├── requireRole()
   └── requireOwnership()
   ↓
Controller
   ↓
Response
```

### Core principle

> **Authentication establishes identity; authorization enforces access control.**

`requireRole()` answers **"Does this user have the required role?"**

`requireOwnership()` answers **"Does this user own this resource?"**