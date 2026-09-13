
---

Perfect. **Authenticate middleware + `/me` tested means the access-token side is working.** Now we build the other half of your authentication system: **refresh tokens**.

# Step 2 — Refresh Token Endpoint

We'll create:

```http
POST /api/auth/refresh
```

### What it does

When the short-lived access token expires:

```text
Access Token
15 minutes
     ↓
Expired ❌
     ↓
Client calls /refresh
     ↓
Browser automatically sends
httpOnly refreshToken cookie
     ↓
Verify refresh JWT
     ↓
Validate Session in MongoDB
     ↓
Generate NEW access token
     ↓
Return access token
```

Your refresh token remains in the cookie; we don't return it in JSON.

---

## 1. Create the refresh controller

In your existing:

```text
src/controllers/auth.controller.js
```

add:

```js
const refresh = async (req, res, next) => {
  try {
    // 1. Get refresh token from cookie
    const refreshToken = req.cookies.refreshToken;

    if (!refreshToken) {
      throw new ApiError(401, "Refresh token is required");
    }

    // 2. Verify refresh JWT
    const decoded = verifyRefreshToken(refreshToken);

    const { sub: userId, sid: sessionId } = decoded;

    // 3. Find the session
    const session = await sessionModel
      .findById(sessionId)
      .select("+refreshToken");

    if (!session) {
      throw new ApiError(401, "Invalid session");
    }

    // 4. Check session belongs to the same user
    if (session.user.toString() !== userId) {
      throw new ApiError(401, "Invalid session");
    }

    // 5. Check if session has been revoked
    if (session.revoked) {
      throw new ApiError(401, "Session has been revoked");
    }

    // 6. Check session expiration
    if (session.expiresAt <= new Date()) {
      throw new ApiError(401, "Session has expired");
    }

    // 7. Compare cookie token with hashed token in DB
    const isRefreshTokenValid = await bcrypt.compare(
      refreshToken,
      session.refreshToken,
    );

    if (!isRefreshTokenValid) {
      throw new ApiError(401, "Invalid refresh token");
    }

    // 8. Generate a new access token
    const accessToken = generateAccessToken(
      userId,
      sessionId,
    );

    // 9. Return new access token
    return res.status(200).json({
      status: "success",
      message: "Access token refreshed successfully",
      data: {
        accessToken,
      },
    });
  } catch (error) {
    next(error);
  }
};
```

Then update your export:

```js
export { register, login, refresh };
```

---

# 2. Why each check matters

This is the important part of this implementation.

### Check 1 — Cookie exists

```js
const refreshToken = req.cookies.refreshToken;
```

Because we configured:

```js
httpOnly: true
```

JavaScript running in the browser can't read it, but the browser automatically sends it with the request.

---

### Check 2 — JWT is valid

```js
const decoded = verifyRefreshToken(refreshToken);
```

Your utility verifies:

```text
JWT signature
JWT expiration
correct refresh secret
```

If any fails → `401`.

---

### Check 3 — Session exists

```js
const session = await sessionModel.findById(sessionId);
```

This is what makes your refresh-token system **session-backed**.

A mathematically valid JWT isn't enough.

```text
Valid JWT
    +
Valid DB session
    +
Not revoked
    +
Not expired
    +
Hash matches
    ↓
Valid refresh request
```

---

### Check 4 — User/session relationship

We have:

```js
sub = userId
sid = sessionId
```

So we make sure:

```js
session.user === userId
```

This prevents a mismatched session from being used with another user's token.

---

### Check 5 — Revocation

```js
if (session.revoked)
```

This is why we added:

```js
revoked: {
  type: Boolean,
  default: false,
}
```

During logout we'll set:

```js
revoked: true
```

Then even if somebody possesses the refresh JWT, it won't work anymore.

---

### Check 6 — Database expiration

We already have:

```js
expiresAt
```

in the session.

So we're checking expiration at the database level too.

---

### Check 7 — Hash comparison

Your MongoDB contains:

```text
refreshToken: "$2b$10$..."
```

not the real refresh token.

Therefore:

```js
bcrypt.compare(
  refreshToken,
  session.refreshToken
);
```

proves that the cookie contains the same refresh token that was issued for that session.

---

# 3. Add the route

In:

```text
src/routes/auth.routes.js
```

add:

```js
router.post("/refresh", refresh);
```

You'll need to import it:

```js
import {
  register,
  login,
  refresh,
} from "../controllers/auth.controller.js";
```

So your auth routes should now roughly be:

```js
router.post(
  "/register",
  registerValidator,
  validate,
  register,
);

router.post(
  "/login",
  loginValidator,
  validate,
  login,
);

router.post("/refresh", refresh);

router.get("/me", authenticate, (req, res) => {
  return res.status(200).json({
    status: "success",
    data: {
      user: req.user,
    },
  });
});
```

---

# 4. Test `/refresh`

First login:

```http
POST /api/auth/login
```

Make sure Postman has received/stored the:

```text
refreshToken
```

cookie.

Then:

```http
POST /api/auth/refresh
```

**No Authorization header is required.**

The refresh token comes from the cookie.

Expected:

```json
{
  "status": "success",
  "message": "Access token refreshed successfully",
  "data": {
    "accessToken": "eyJ..."
  }
}
```

---

# 5. Important test

Delete the `refreshToken` cookie from Postman and call:

```http
POST /api/auth/refresh
```

Expected:

```http
401 Unauthorized
```

with:

```text
Refresh token is required
```

Then restore/login again and test `/refresh`.

---

## Current Phase 1 status

```text
Register                       ✅
Login                          ✅
Authenticate middleware        ✅
Protected /me                  ✅
Refresh endpoint               ⏭️ YOU ARE HERE
Logout                         ⏭️
Final security testing         ⏭️
Duplicate-key handling         ⏭️
```

**Build and test `/refresh` now.** After it works, the next step is **logout + session revocation**, which will complete the actual authentication lifecycle.