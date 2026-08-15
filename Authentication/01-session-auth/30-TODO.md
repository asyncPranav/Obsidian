
---
#DOUBT - `How user can update and delete themselves`

Yes — and this is an important distinction.

Right now, based on **our current route design**, a normal user **cannot update/delete themselves through the user routes**, because we made those routes admin-only.

But there are actually **two different concepts** we need to decide.

### 1. Admin managing users

Current design:

```text
PATCH /api/users/:id
DELETE /api/users/:id
```

If protected with:

```js
authenticate,
authorize("admin")
```

then:

```text
User → PATCH /api/users/:id ❌
User → DELETE /api/users/:id ❌

Admin → PATCH /api/users/:id ✅
Admin → DELETE /api/users/:id ✅
```

So yes, **a user currently cannot update/delete himself through these endpoints.**

---

### 2. User managing his own account

In a real application, we'd normally provide separate endpoints:

```text
PATCH /api/auth/me
DELETE /api/auth/me
```

or:

```text
PATCH /api/users/me
DELETE /api/users/me
```

Then the identity comes from:

```js
req.user._id
```

rather than:

```js
req.params.id
```

For example:

```http
PATCH /api/auth/me
```

```json
{
  "name": "New Name"
}
```

Server does:

```js
User.findByIdAndUpdate(
  req.user._id,
  { name },
  ...
)
```

The client **never gets to choose which user to update**.

Same idea for deletion:

```http
DELETE /api/auth/me
```

Server:

```js
User.findByIdAndDelete(req.user._id)
```

---

### But I recommend we DON'T add this yet.

Our learning goal is currently:

```text
Authentication
      ↓
Session
      ↓
req.user
      ↓
Task ownership
      ↓
Role authorization
      ↓
Admin role management
```

We've already learned the important authorization concepts.

So for now, keep:

```text
/api/users/:id
```

as **admin management routes**.

Later, we can add:

```text
/api/auth/me
```

for **self-management**, and that will give us another useful lesson:

> **Authorization isn't only "admin vs user"; it's also "can this authenticated user modify this particular resource?"**

That distinction is exactly what we already practiced with `checkTaskOwnership`.