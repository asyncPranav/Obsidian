
---

Absolutely. Let's make **Step 12 from scratch**, matching **your actual project** and the session system you built.

We are using:

```text
Express
   +
express-session
   +
MongoDB session store
```

There is **NO Session model**.

---

# Phase 2 — Step 12

# Logout & Session Invalidation

## 1. Where we are right now

We've already built:

```text
Step 1  → Password hashing
Step 2  → Registration
Step 3  → Login/password verification
Step 4  → Session creation
Step 5  → Session middleware
Step 6  → req.user + session flow
Step 7  → Protected routes
Step 8  → Authentication testing
Step 9  → Stop trusting client userId
Step 10 → Task ownership authorization
Step 11 → Role-based authorization
```

Now:

> **Step 12 = Logout**

---

# 2. First understand what login currently does

When the user logs in:

```http
POST /api/auth/login
```

we verify:

```text
email
   ↓
find user
   ↓
bcrypt.compare()
   ↓
password correct
   ↓
req.session.userId = user._id
```

Then `express-session` stores the session.

Your MongoDB document looks like:

```json
{
  "expires": "...",
  "session": "{\"cookie\":...,\"userId\":\"6a7dd8c1fbdc0ea8ec61891a\"}"
}
```

The important part is:

```text
userId
   ↓
6a7dd8c1fbdc0ea8ec61891a
```

So the server remembers:

```text
Session → User
```

---

# 3. What happens on future requests?

Suppose we call:

```http
GET /api/auth/me
```

The browser/Postman sends the session cookie.

Then:

```text
Cookie
   ↓
express-session
   ↓
req.session
   ↓
req.session.userId
   ↓
User.findById()
   ↓
req.user
```

Therefore:

```js
req.user
```

contains the currently authenticated user.

---

# 4. Now think about logout

If the user clicks logout:

```http
POST /api/auth/logout
```

we need to destroy the session.

The flow should become:

```text
POST /logout
     ↓
Current session
     ↓
req.session.destroy()
     ↓
Session removed from MongoDB
     ↓
Clear session cookie
     ↓
200 OK
```

After that:

```text
GET /auth/me
     ↓
No valid session
     ↓
401 Unauthorized
```

---

# 5. Why can't we just clear the cookie?

Suppose we only do:

```js
res.clearCookie("connect.sid");
```

The client loses the cookie.

But the server-side session could still exist:

```text
MongoDB

session
   ↓
userId = 6a7dd...
```

So proper logout should do **both**:

```text
1. Destroy server-side session
2. Clear client-side cookie
```

---

# 6. Step 12 — Check your auth controller

Open:

```text
src/controllers/auth.controller.js
```

We are going to add a logout controller.

The structure is:

```js
const logout = (req, res, next) => {
  // destroy session

  // clear cookie

  // send response
};
```

---

# 7. Destroy the session

Because we're using `express-session`, we don't manually delete anything from MongoDB.

We use:

```js
req.session.destroy(...)
```

So:

```js
const logout = (req, res, next) => {
  req.session.destroy((err) => {
    if (err) {
      return next(err);
    }

    // continue...
  });
};
```

### What does this mean?

```text
req.session
     ↓
destroy()
     ↓
express-session removes the session
     ↓
MongoDB session store updated
```

You don't need:

```js
Session.findOne(...)
```

or:

```js
Session.deleteOne(...)
```

because **we don't have a Session model**.

---

# 8. Clear the cookie

After destroying the session:

```js
res.clearCookie("connect.sid");
```

So:

```js
const logout = (req, res, next) => {
  req.session.destroy((err) => {
    if (err) {
      return next(err);
    }

    res.clearCookie("connect.sid");

    // response
  });
};
```

### Why `connect.sid`?

`express-session` uses:

```text
connect.sid
```

as the default cookie name.

If your session configuration explicitly uses another name, use that name instead.

For example:

```js
name: "sessionId"
```

would mean:

```js
res.clearCookie("sessionId");
```

**Check your existing session configuration rather than changing it.**

---

# 9. Send success response

Finally:

```js
res.status(200).json({
  status: "success",
  message: "Logged out successfully",
});
```

So the complete controller becomes:

```js
const logout = (req, res, next) => {
  req.session.destroy((err) => {
    if (err) {
      return next(err);
    }

    res.clearCookie("connect.sid");

    res.status(200).json({
      status: "success",
      message: "Logged out successfully",
    });
  });
};
```

That's our logout controller.

---

# 10. Export the controller

At the bottom of:

```text
src/controllers/auth.controller.js
```

make sure `logout` is exported.

For example:

```js
export {
  register,
  login,
  logout,
};
```

Keep whatever other controllers you already have.

---

# 11. Add the logout route

Open:

```text
src/routes/auth.route.js
```

Import `logout`:

```js
import {
  register,
  login,
  logout,
} from "../controllers/auth.controller.js";
```

Then add:

```js
router.post("/logout", authenticate, logout);
```

So conceptually:

```js
router.post("/register", register);

router.post("/login", login);

router.post("/logout", authenticate, logout);
```

---

# 12. Why do we use `authenticate`?

This is important.

We want:

```text
POST /logout
      ↓
authenticate
      ↓
logout
```

The authentication middleware checks the session first.

Conceptually:

```text
req.session.userId
       ↓
Does it exist?
       ↓
YES
       ↓
Find user
       ↓
req.user
       ↓
next()
       ↓
logout controller
```

So logout operates on the current authenticated session.

---

# 13. Complete logout flow

Now the entire request looks like:

```text
POST /api/auth/logout
          │
          ▼
   authenticate middleware
          │
          ▼
   Check session
          │
          ▼
     req.user exists
          │
          ▼
     logout controller
          │
          ▼
   req.session.destroy()
          │
          ▼
Session removed from MongoDB
          │
          ▼
   clear connect.sid
          │
          ▼
       200 OK
```

---

# 14. Test it in Postman

Now we're going to test this properly.

## Test 1 — Login

```http
POST /api/auth/login
```

Use your existing login body.

Expected:

```text
200 OK
```

You should have a session cookie.

---

## Test 2 — `/me`

```http
GET /api/auth/me
```

Expected:

```text
200 OK
```

This proves:

```text
Cookie
 ↓
Session
 ↓
User
```

is working.

---

# 15. Test 3 — Logout

Now:

```http
POST /api/auth/logout
```

Expected:

```json
{
  "status": "success",
  "message": "Logged out successfully"
}
```

Status:

```text
200 OK
```

---

# 16. Test 4 — `/me` after logout

This is the **most important test**.

Immediately call:

```http
GET /api/auth/me
```

Expected:

```text
401 Unauthorized
```

Why?

Because:

```text
Logout
   ↓
req.session.destroy()
   ↓
Session gone
   ↓
authenticate middleware
   ↓
No valid session
   ↓
401
```

---

# 17. Verify MongoDB

Before logout:

```text
sessions collection

Session
  ↓
userId = 6a7dd8...
```

After:

```text
req.session.destroy()
```

the session should no longer exist in the session store.

So:

```text
Before logout:

MongoDB
└── Session → User


After logout:

MongoDB
└── Session ❌
```

---

# 18. Understand 401 vs 403

This is worth locking in now because we've used both authentication and authorization.

### 401 — Unauthenticated

Means:

> "You haven't successfully authenticated."

Examples:

```text
No session
Invalid session
Expired session
Logged out
```

Flow:

```text
Request
  ↓
authenticate
  ↓
No valid session
  ↓
401
```

---

### 403 — Forbidden

Means:

> "I know who you are, but you're not allowed to do this."

Example:

```text
User A
  ↓
tries to access User B's task
  ↓
Authenticated ✅
  ↓
Owner ❌
  ↓
403
```

Or:

```text
student
   ↓
admin-only route
   ↓
403
```

Remember:

```text
401 → Who are you?
403 → I know who you are, but no.
```

---

# 19. Final Step 12 code

Your logout controller:

```js
const logout = (req, res, next) => {
  req.session.destroy((err) => {
    if (err) {
      return next(err);
    }

    res.clearCookie("connect.sid");

    res.status(200).json({
      status: "success",
      message: "Logged out successfully",
    });
  });
};
```

Your route:

```js
router.post("/logout", authenticate, logout);
```

Again, if your cookie has a custom name, replace:

```js
"connect.sid"
```

with your actual cookie name.

---

# Step 12 Checklist

### Implementation

```text
□ Create logout controller
□ Use req.session.destroy()
□ Handle destroy error
□ Clear session cookie
□ Return 200 response
□ Export logout controller
□ Add /logout route
□ Protect /logout with authenticate
```

### Testing

```text
□ Login → 200
□ /auth/me → 200
□ MongoDB session exists
□ Logout → 200
□ MongoDB session removed
□ /auth/me → 401
```

### Concepts

```text
□ Understand what req.session.destroy() does
□ Understand why we clear the cookie
□ Understand why we don't use a Session model
□ Understand 401
□ Understand 403
```

### The one-line mental model

```text
LOGIN  → create session
REQUEST → use session
LOGOUT → destroy session
```

**Implement Step 12 using your existing `express-session + MongoDB` setup, then test the complete Login → `/me` → Logout → `/me` flow.**