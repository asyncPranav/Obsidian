
---


# JWT Authentication in Express.js — Complete Beginner Notes

---

## 1. The Problem JWT Solves — Start With Why

Before learning any technology, always start with the problem it solves. You already know session-based authentication. When a user logs in, the server creates a session, stores it in a database or memory, and sends the browser a session ID cookie. Every request sends that cookie, the server looks up the session in its storage, and says "yes, I know this person."

This works well for traditional web apps. But it has a fundamental architectural problem: **the server must maintain state.**

Think about what "maintaining state" means at scale:

```
User logs in on Server A → session stored on Server A
User makes next request → hits Server B (load balancer) → Server B
has no session → user gets logged out

Solution: share session storage between all servers → adds a Redis
or database dependency just to keep users logged in
```

As soon as you scale beyond one server, sessions become complicated. And there is another problem: sessions do not work well when your backend serves multiple types of clients. A session cookie works in a browser, but a mobile app or a command-line tool cannot easily handle cookies. They need a simpler, more portable way to send credentials with every request.

**JWT (JSON Web Token) solves both problems.** Instead of the server storing anything, the server gives the client a self-contained token after login. The token itself carries the user's identity. Any server that knows the secret can verify the token without consulting a database or shared storage. The client — browser, mobile app, CLI — just sends this token with every request, typically in a header.

```
Session-based (stateful):
  Server stores: { sessionId: "abc", userId: 42, role: "admin" }
  Client holds: cookie with sessionId only
  Every request: server must look up sessionId in storage

JWT-based (stateless):
  Server stores: nothing about the user session
  Client holds: token containing { userId: 42, role: "admin" }
  Every request: server just verifies the token's signature
```

---

## 2. What Exactly Is a JWT?

A JSON Web Token is a compact, URL-safe string that carries a JSON payload. It has three parts separated by dots:

```
header.payload.signature

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6NDIsInJvbGUiOiJhZG1pbiIsImlhdCI6MTcxNzAwMDAwMCwiZXhwIjoxNzE3MDg2NDAwfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

Each part is individually **Base64URL encoded** — not encrypted, just encoded. This means anyone can decode and read the payload without knowing any secret. This is critical to understand and we will explore it deeply in a moment.

### Part 1: Header

The header describes the token itself — what type it is and which signing algorithm is used.

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

`alg` is the algorithm used to create the signature. `HS256` is HMAC-SHA256, which uses a single secret key. `RS256` uses a public/private key pair and is used in more advanced setups. `typ` just declares this is a JWT.

Base64URL encode this JSON → first part of the token.

### Part 2: Payload (Claims)

The payload contains the actual data — called **claims**. Claims are statements about the user and other metadata.

```json
{
  "id":   42,
  "role": "admin",
  "iat":  1717000000,
  "exp":  1717086400
}
```

**Registered claims** are standardized fields that JWT libraries understand automatically:

`iss` (issuer) — who created the token (e.g., "myapp.com"). `sub` (subject) — who the token is about (usually the user ID). `aud` (audience) — who the token is intended for. `exp` (expiration) — when the token expires (Unix timestamp). The library automatically rejects expired tokens. `iat` (issued at) — when the token was created (Unix timestamp). `nbf` (not before) — token is not valid before this time.

**Custom claims** are any data you add yourself:

```json
{
  "id":       42,
  "username": "pranav",
  "role":     "admin",
  "iat":      1717000000,
  "exp":      1717086400
}
```

Base64URL encode this JSON → second part of the token.

### Part 3: Signature

The signature is what makes the token trustworthy. It is computed using the encoded header, the encoded payload, and a secret key:

```
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)
```

The secret is a string only your server knows. When a token comes back from a client, you re-compute the signature using the same formula and compare it to the signature in the token. If they match — the token has not been tampered with. If they don't — the token is rejected.

```
Server signs:
  Header: {"alg":"HS256","typ":"JWT"}
  Payload: {"id":42,"role":"admin","exp":...}
  Secret: "my-super-secret"
  → Signature: "SflKxwRJSMeKKF2QT4..."

Client sends token back later.
Server re-computes signature with same secret.
Re-computed matches stored? → VALID ✓
Re-computed doesn't match? → TAMPERED ✗ → Rejected
```

---

## 3. The Critical Distinction: Signed vs Encrypted

This is the most important thing to understand about JWTs and the source of the most dangerous beginner mistakes.

**A JWT is signed, NOT encrypted.**

The payload is encoded with Base64URL which is completely reversible. Anyone who gets hold of a JWT can decode and read the payload instantly. You can try it yourself at jwt.io.

```
Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
       eyJpZCI6NDIsInJvbGUiOiJhZG1pbiJ9.
       SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

Decode the payload (anyone can do this without the secret):
→ { "id": 42, "role": "admin" }
```

What the signature provides is **integrity and authenticity**, not **confidentiality**:

- **Integrity**: the payload has not been modified since the server created the token.
- **Authenticity**: this token was created by a server that knows the secret.
- **Confidentiality**: ✗ NOT provided. The payload is readable by anyone.

Practical consequences:

```
✅ Safe to store in JWT:
  User ID
  Username
  Role (user, admin, editor)
  Email
  Subscription plan

❌ NEVER store in JWT:
  Password (even hashed)
  Credit card numbers
  Social security numbers
  Private API keys
  Any personally sensitive data you wouldn't put on a billboard
```

If you need the payload to be unreadable by the client, you need **JWE (JSON Web Encryption)** — a different standard. Most applications do not need this and use regular JWTs with the above guidelines.

---

## 4. The Complete JWT Authentication Flow

Now that you understand the structure, let's trace the complete flow:

```
┌──────────────────────────────────────────────────────────────────┐
│                       REGISTRATION                               │
│                                                                  │
│  Client → POST /auth/register { username, email, password }      │
│  Server → validates input                                        │
│  Server → checks email not already registered                    │
│  Server → hashes password with bcrypt                            │
│  Server → saves user to database                                 │
│  Server → (optionally) signs a JWT and returns it                │
│  Client → stores the token                                       │
└──────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────┐
│                          LOGIN                                   │
│                                                                  │
│  Client → POST /auth/login { email, password }                   │
│  Server → finds user by email in DB                              │
│  Server → bcrypt.compare(submittedPassword, storedHash)          │
│  Server → if match: jwt.sign({ id, role }, secret, { expiresIn })│
│  Server → returns token to client                                │
│  Client → stores token (localStorage, memory, httpOnly cookie)   │
└──────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────┐
│                    AUTHENTICATED REQUEST                         │
│                                                                  │
│  Client → GET /api/profile                                       │
│           Authorization: Bearer eyJhbGciOiJIUzI1NiIs...          │
│                                                                  │
│  Server auth middleware:                                         │
│    1. Extract token from Authorization header                    │
│    2. jwt.verify(token, secret) → decode + verify signature      │
│    3. Check token not expired (exp claim)                        │
│    4. Optionally: fetch user from DB to confirm still exists     │
│    5. Attach user to req.user                                    │
│    6. Call next() → route handler runs                           │
│                                                                  │
│  Server → returns profile data                                   │
└──────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────┐
│                          LOGOUT                                  │
│                                                                  │
│  With JWT: Server does nothing (stateless — there is nothing     │
│  to destroy on the server side)                                  │
│  Client → deletes the token from storage                         │
│                                                                  │
│  Problem: token is still valid until its expiry even after logout│
│  Solution: short expiry times + refresh tokens (covered below)   │
└──────────────────────────────────────────────────────────────────┘
```

---

## 5. Where to Store the JWT on the Client

This is one of the most debated topics in JWT authentication. There are three options and each has tradeoffs:

### Option 1: localStorage

```js
// Storing
localStorage.setItem("token", token);

// Reading for requests
const token = localStorage.getItem("token");
fetch("/api/data", {
  headers: { Authorization: `Bearer ${token}` }
});
```

**Pros:** Simple. Persists across browser sessions and tabs. Easy to implement.

**Cons:** Vulnerable to **XSS attacks**. Any JavaScript running on your page — including injected malicious scripts — can read `localStorage` and steal the token. Once stolen, the attacker has full access until the token expires.

### Option 2: sessionStorage

Same as localStorage but clears when the tab or browser closes. Slightly safer because stolen tokens expire with the session, but still vulnerable to XSS.

### Option 3: httpOnly Cookie (Most Secure)

```js
// Server sets the token as an httpOnly cookie
res.cookie("token", jwtToken, {
  httpOnly: true,   // JS cannot read this — XSS protection
  secure:   true,   // HTTPS only
  sameSite: "strict", // CSRF protection
  maxAge:   7 * 24 * 60 * 60 * 1000, // 7 days
});
```

**Pros:** JavaScript cannot read `httpOnly` cookies — immune to XSS.

**Cons:** Cookies are sent automatically by the browser — you need CSRF protection (sameSite attribute handles most cases). Does not work as naturally for mobile app clients that need to manage tokens manually.

### Option 4: In-Memory (Most Secure for SPAs)

Store the access token in a JavaScript variable in memory (not in any persistent storage). Use a refresh token in an httpOnly cookie to get new access tokens.

**Pros:** Access token is not accessible from storage at all. **Cons:** Token is lost on page refresh — must be re-fetched from the refresh endpoint.

### Comparison Table

```
Storage         XSS Risk    CSRF Risk   Persists    Best For
─────────────────────────────────────────────────────────────
localStorage    HIGH        Low         Yes         Simple demos only
sessionStorage  HIGH        Low         Tab only    Avoid
httpOnly Cookie Low         Medium      Yes         Web apps
In-Memory       Lowest      Low         No          SPAs with refresh
```

**Recommendation:** For web apps — httpOnly cookie. For mobile apps — store securely in the platform's secure storage (Keychain on iOS, Keystore on Android). For learning — localStorage is fine.

---

## 6. Access Tokens and Refresh Tokens

This is where JWT becomes a proper production authentication system.

**The problem with long-lived tokens:** If you give a user a JWT that lasts 30 days and it gets stolen, the attacker has 30 days of access. You cannot revoke it (JWT is stateless — the server stores nothing).

**The solution: access token + refresh token pattern.**

An **access token** is a short-lived JWT (15 minutes to 1 hour). It is sent with every API request. Because it expires quickly, a stolen token becomes useless very soon.

A **refresh token** is a long-lived token (7 days to 30 days) stored in an httpOnly cookie. It is only sent to one specific endpoint: `POST /auth/refresh`. When the access token expires, the client sends the refresh token to get a new access token without requiring the user to log in again.

```
┌─────────────────────────────────────────────────────────────────┐
│                ACCESS + REFRESH TOKEN FLOW                      │
│                                                                 │
│  Login:                                                         │
│    Server creates:                                              │
│      accessToken  (expires in 15 min) → sent in JSON body       │
│      refreshToken (expires in 7 days) → set as httpOnly cookie  │
│                                                                 │
│  Normal requests (first 15 minutes):                            │
│    Client sends: Authorization: Bearer <accessToken>            │
│    Server verifies accessToken → processes request              │
│                                                                 │
│  After 15 minutes (access token expires):                       │
│    Client receives 401 Unauthorized                             │
│    Client sends: POST /auth/refresh                             │
│      (refreshToken cookie is sent automatically)                │
│    Server verifies refreshToken → issues new accessToken        │
│    Client stores new accessToken → retries original request     │
│                                                                 │
│  Logout:                                                        │
│    Server adds refreshToken to a blocklist (in DB or Redis)     │
│    Server clears the refreshToken cookie                        │
│    Client deletes accessToken from storage                      │
│    Even if accessToken is stolen: expires in 15 min max         │
└─────────────────────────────────────────────────────────────────┘
```

This pattern gives you the best of both worlds: the stateless scalability of JWT for most requests, and the ability to truly log users out by invalidating refresh tokens.

---

## 7. Token Revocation — Solving the Stateless Logout Problem

Pure JWT is stateless: once issued, a token is valid until it expires. You cannot "un-issue" a token. This creates a real problem:

- User logs out → access token is still valid for up to 1 hour
- User changes password → old tokens still work
- Admin bans user → user can still make requests with existing token

### Solution 1: Short Expiry (Simplest)

Keep access tokens short-lived (5–15 minutes). A stolen or invalidated token becomes useless very quickly. This is the most common approach.

### Solution 2: Token Blocklist (More Control)

Maintain a set of invalidated token IDs (the `jti` claim, or the full token) in a fast store like Redis. On every request, check whether the token's ID is in the blocklist.

```js
// When user logs out:
const tokenId = decoded.jti; // unique ID in the token
await redis.set(`blocklist:${tokenId}`, "1", "EX", decoded.exp - Date.now()/1000);
// Only store until the token would have expired anyway

// In auth middleware, after verifying the token:
const isBlocklisted = await redis.get(`blocklist:${decoded.jti}`);
if (isBlocklisted) return next(new AppError("Token has been invalidated.", 401));
```

This adds a small Redis lookup on every request but gives you true logout capability.

### Solution 3: Refresh Token Rotation

Every time a refresh token is used, issue a new refresh token and invalidate the old one. Store refresh tokens in the database. This means you can revoke all refresh tokens for a user (e.g., on password change) by deleting their database records.

---

## 8. JWT vs Session — When to Use Which

```
USE JWT WHEN:                         USE SESSIONS WHEN:
─────────────────────────────────────────────────────────────────
Multiple servers / microservices      Single server application
Mobile app clients                    Traditional server-rendered
                                        web app (EJS/Pug)
Third-party API integrations          Need immediate token revocation
                                        without extra infrastructure
Public APIs consumed by others        Small-scale personal project
SPA (React/Vue) + separate backend    Team is new to auth
Cross-domain authentication           Need to store large user state
```

Neither is universally better. JWT is the standard for REST APIs consumed by separate clients. Sessions are simpler for traditional server-rendered apps where the backend also serves the HTML.

---

## 9. Installation

```bash
npm install jsonwebtoken bcrypt express express-validator
npm install mongoose dotenv morgan
```

The key package is `jsonwebtoken` — the Node.js implementation of the JWT standard. It provides `jwt.sign()` to create tokens and `jwt.verify()` to verify and decode them.

---

## 10. The `jsonwebtoken` API — Every Method You Need

Understanding the library's API before using it saves a lot of confusion later.

### `jwt.sign(payload, secret, options)`

Creates a new JWT. Returns the token string.

```js
const jwt = require("jsonwebtoken");

// Basic usage
const token = jwt.sign(
  { id: 42, role: "admin" },  // payload — what to store in the token
  "my-secret-key",             // secret — used to sign the token
  { expiresIn: "7d" }          // options
);

// Options you'll use most:
// expiresIn: "15m" | "2h" | "7d" | "30d" | 3600 (seconds)
// issuer:    "myapp.com"
// audience:  "myapp-users"
// jwtid:     uuid()  ← unique ID per token (needed for blocklisting)

// The payload is encoded but NOT encrypted
// Anyone can decode and read it
console.log(token);
// eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6NDIsInJvbGUiOiJhZG1pbiIsImlhdCI6MTcxNzAwMDAwMCwiZXhwIjoxNzE3NjA0ODAwfQ.signature
```

### `jwt.verify(token, secret, [options])`

Verifies the signature AND checks expiry. Returns the decoded payload if valid. **Throws an error** if the token is invalid or expired — you must handle this.

```js
try {
  const decoded = jwt.verify(token, "my-secret-key");
  // decoded = { id: 42, role: "admin", iat: 1717000000, exp: 1717604800 }
  console.log(decoded.id);   // 42
  console.log(decoded.role); // "admin"
} catch (err) {
  if (err.name === "TokenExpiredError") {
    console.log("Token has expired");
  } else if (err.name === "JsonWebTokenError") {
    console.log("Token is invalid:", err.message);
  }
}
```

### `jwt.decode(token)`

Decodes the payload WITHOUT verifying the signature. Never use this for authentication — use only for reading non-security-critical data when you don't have the secret (like a client reading its own token).

```js
// Anyone can call this — no secret needed
const decoded = jwt.decode(token);
// Returns the payload object or null if the token is malformed
// Does NOT check if the signature is valid or if the token is expired
```

---

## 11. Building the JWT Auth System — Layer by Layer

Now let's build the complete authentication system step by step.

### 11.1 Token Creation Helper

```js
// utils/tokenUtils.js

const jwt = require("jsonwebtoken");

// Create an access token — short lived, sent in response body
const createAccessToken = (userId, role) => {
  return jwt.sign(
    { id: userId, role },
    process.env.JWT_ACCESS_SECRET,
    { expiresIn: process.env.JWT_ACCESS_EXPIRES_IN || "15m" }
  );
};

// Create a refresh token — long lived, stored in httpOnly cookie
const createRefreshToken = (userId) => {
  return jwt.sign(
    { id: userId },
    process.env.JWT_REFRESH_SECRET,      // DIFFERENT secret from access token
    { expiresIn: process.env.JWT_REFRESH_EXPIRES_IN || "7d" }
  );
};

// Send both tokens — access in JSON body, refresh in httpOnly cookie
const sendTokens = (user, statusCode, req, res) => {
  const accessToken  = createAccessToken(user._id, user.role);
  const refreshToken = createRefreshToken(user._id);

  // Refresh token → httpOnly cookie (JS cannot read it)
  res.cookie("refreshToken", refreshToken, {
    httpOnly: true,
    secure:   process.env.NODE_ENV === "production",
    sameSite: "strict",
    maxAge:   7 * 24 * 60 * 60 * 1000, // 7 days in ms
  });

  // Remove password from response
  user.password = undefined;

  res.status(statusCode).json({
    success:     true,
    accessToken, // client stores this in memory or localStorage
    data: { user },
  });
};

module.exports = { createAccessToken, createRefreshToken, sendTokens };
```

### 11.2 The Auth Middleware — Protecting Routes

```js
// middleware/authMiddleware.js

const jwt       = require("jsonwebtoken");
const User      = require("../models/User");
const catchAsync = require("../utils/catchAsync");
const AppError   = require("../utils/appError");

// protect: verify access token and attach user to req
const protect = catchAsync(async (req, res, next) => {
  // Step 1: Extract the token from the Authorization header
  // The header looks like: Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
  let token;

  if (
    req.headers.authorization &&
    req.headers.authorization.startsWith("Bearer ")
  ) {
    token = req.headers.authorization.split(" ")[1];
  }
  // Alternative: read from cookie if you store the access token there
  // else if (req.cookies?.accessToken) {
  //   token = req.cookies.accessToken;
  // }

  if (!token) {
    return next(
      new AppError("You are not logged in. Please log in to get access.", 401)
    );
  }

  // Step 2: Verify the token
  // jwt.verify throws if the token is invalid or expired
  // catchAsync will forward the thrown error to our global error handler
  let decoded;
  try {
    decoded = jwt.verify(token, process.env.JWT_ACCESS_SECRET);
  } catch (err) {
    if (err.name === "TokenExpiredError") {
      return next(new AppError("Your session has expired. Please log in again.", 401));
    }
    return next(new AppError("Invalid token. Please log in again.", 401));
  }

  // Step 3: Check if the user still exists in the database
  // What if the user was deleted after the token was issued?
  const user = await User.findById(decoded.id);
  if (!user) {
    return next(
      new AppError("The user belonging to this token no longer exists.", 401)
    );
  }

  // Step 4: Check if user changed password after token was issued
  // If they did, the old token should be considered invalid
  // (This requires a passwordChangedAt field on the User model)
  if (user.passwordChangedAfter(decoded.iat)) {
    return next(
      new AppError("Password was recently changed. Please log in again.", 401)
    );
  }

  // Step 5: Grant access — attach user to request
  req.user = user;
  next();
});

// restrictTo: role-based access control
// Usage: router.delete("/:id", protect, restrictTo("admin"), handler)
const restrictTo = (...roles) => {
  return (req, res, next) => {
    // req.user is set by protect middleware (must run first)
    if (!roles.includes(req.user.role)) {
      return next(
        new AppError(
          `Access denied. This route requires one of these roles: ${roles.join(", ")}`,
          403
        )
      );
    }
    next();
  };
};

module.exports = { protect, restrictTo };
```

---

## 12. Real-World Example — Complete JWT Auth API

Let's build a complete, production-aware JWT authentication API with access tokens, refresh tokens, password change handling, and proper security patterns.

```
Project structure:
├── server.js
├── app.js
├── .env
├── config/
│   └── db.js
├── models/
│   └── User.js
├── controllers/
│   └── authController.js
├── routes/
│   └── authRoutes.js
├── middleware/
│   ├── authMiddleware.js
│   ├── validate.js
│   └── errorHandler.js
├── validators/
│   └── authValidator.js
└── utils/
    ├── catchAsync.js
    ├── appError.js
    └── tokenUtils.js
```

```
# .env
PORT=3000
NODE_ENV=development
MONGO_URI=mongodb://localhost:27017/jwt-auth
JWT_ACCESS_SECRET=acc3ssS3cr3t_ch@ng3_in_pr0d_xK9mLp2Q8w
JWT_ACCESS_EXPIRES_IN=15m
JWT_REFRESH_SECRET=r3fr3shS3cr3t_ch@ng3_in_pr0d_zV7nBt4Y1q
JWT_REFRESH_EXPIRES_IN=7d
```

```js
// config/db.js

const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI);
    console.log("MongoDB connected");
  } catch (err) {
    console.error("DB connection error:", err.message);
    process.exit(1);
  }
};

module.exports = connectDB;
```

```js
// models/User.js

const mongoose = require("mongoose");
const bcrypt   = require("bcrypt");

const userSchema = new mongoose.Schema(
  {
    username: {
      type:      String,
      required:  [true, "Username is required"],
      trim:      true,
      minlength: [3,  "Username must be at least 3 characters"],
      maxlength: [20, "Username cannot exceed 20 characters"],
    },
    email: {
      type:      String,
      required:  [true, "Email is required"],
      unique:    true,
      lowercase: true,
      trim:      true,
    },
    password: {
      type:      String,
      required:  [true, "Password is required"],
      minlength: [8, "Password must be at least 8 characters"],
      select:    false, // never include password in query results by default
    },
    role: {
      type:    String,
      enum:    ["user", "editor", "admin"],
      default: "user",
    },
    // Track when password was last changed
    // Used to invalidate tokens issued before the change
    passwordChangedAt: {
      type:   Date,
      select: false,
    },
    // Store the refresh token hash in the DB for true revocation
    refreshToken: {
      type:   String,
      select: false,
    },
    isActive: {
      type:    Boolean,
      default: true,
      select:  false,
    },
  },
  { timestamps: true }
);

// ─── Pre-save: Hash Password ──────────────────────────────────────────────────
userSchema.pre("save", async function (next) {
  // Only hash if password was actually modified
  if (!this.isModified("password")) return next();

  this.password = await bcrypt.hash(this.password, 12);

  // When password changes, record the timestamp
  // This lets us invalidate tokens issued before this moment
  if (!this.isNew) {
    // Subtract 1 second to ensure tokens issued exactly at change time are caught
    this.passwordChangedAt = new Date(Date.now() - 1000);
  }

  next();
});

// ─── Instance Method: Compare Passwords ──────────────────────────────────────
userSchema.methods.comparePassword = async function (candidatePassword) {
  // "this.password" is not available by default (select: false)
  // To use this method you must explicitly select the password:
  // await User.findOne({ email }).select("+password")
  return await bcrypt.compare(candidatePassword, this.password);
};

// ─── Instance Method: Was password changed after token was issued? ────────────
userSchema.methods.passwordChangedAfter = function (jwtIssuedAt) {
  if (!this.passwordChangedAt) return false; // password never changed

  // Convert passwordChangedAt to seconds (same unit as JWT timestamps)
  const changedAt = parseInt(this.passwordChangedAt.getTime() / 1000, 10);

  // If the password was changed AFTER the token was issued → token is stale
  return changedAt > jwtIssuedAt;
};

// ─── Remove Sensitive Fields From Responses ───────────────────────────────────
userSchema.methods.toJSON = function () {
  const obj = this.toObject();
  delete obj.password;
  delete obj.refreshToken;
  delete obj.passwordChangedAt;
  delete obj.__v;
  return obj;
};

module.exports = mongoose.model("User", userSchema);
```

```js
// utils/catchAsync.js

module.exports = (fn) => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);
```

```js
// utils/appError.js

class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode    = statusCode;
    this.status        = `${statusCode}`.startsWith("4") ? "fail" : "error";
    this.isOperational = true;
    Error.captureStackTrace(this, this.constructor);
  }
}

module.exports = AppError;
```

```js
// utils/tokenUtils.js

const jwt = require("jsonwebtoken");

const createAccessToken = (userId, role) =>
  jwt.sign(
    { id: userId, role },
    process.env.JWT_ACCESS_SECRET,
    { expiresIn: process.env.JWT_ACCESS_EXPIRES_IN }
  );

const createRefreshToken = (userId) =>
  jwt.sign(
    { id: userId },
    process.env.JWT_REFRESH_SECRET,
    { expiresIn: process.env.JWT_REFRESH_EXPIRES_IN }
  );

const sendTokens = (user, statusCode, res) => {
  const accessToken  = createAccessToken(user._id, user.role);
  const refreshToken = createRefreshToken(user._id);

  // Refresh token as httpOnly cookie — JS cannot read this
  res.cookie("refreshToken", refreshToken, {
    httpOnly: true,
    secure:   process.env.NODE_ENV === "production",
    sameSite: "strict",
    maxAge:   7 * 24 * 60 * 60 * 1000, // 7 days
  });

  // Strip sensitive fields before sending
  user.password     = undefined;
  user.refreshToken = undefined;

  res.status(statusCode).json({
    success:     true,
    accessToken, // client stores this (in memory or localStorage)
    expiresIn:   process.env.JWT_ACCESS_EXPIRES_IN,
    data: { user },
  });
};

module.exports = { createAccessToken, createRefreshToken, sendTokens };
```

```js
// middleware/authMiddleware.js

const jwt        = require("jsonwebtoken");
const User       = require("../models/User");
const catchAsync = require("../utils/catchAsync");
const AppError   = require("../utils/appError");

// Verify access token and attach user to req.user
const protect = catchAsync(async (req, res, next) => {
  // 1. Get token from Authorization header
  let token;
  if (req.headers.authorization?.startsWith("Bearer ")) {
    token = req.headers.authorization.split(" ")[1];
  }

  if (!token) {
    return next(new AppError("Not authenticated. Please log in.", 401));
  }

  // 2. Verify token signature and expiry
  let decoded;
  try {
    decoded = jwt.verify(token, process.env.JWT_ACCESS_SECRET);
  } catch (err) {
    if (err.name === "TokenExpiredError") {
      return next(new AppError("Access token expired. Please refresh your token.", 401));
    }
    return next(new AppError("Invalid token. Please log in again.", 401));
  }

  // 3. Confirm user still exists and is active
  const user = await User.findById(decoded.id).select("+passwordChangedAt +isActive");
  if (!user) {
    return next(new AppError("User belonging to this token no longer exists.", 401));
  }
  if (!user.isActive) {
    return next(new AppError("This account has been deactivated.", 401));
  }

  // 4. Confirm password hasn't changed since token was issued
  if (user.passwordChangedAfter(decoded.iat)) {
    return next(new AppError("Password recently changed. Please log in again.", 401));
  }

  req.user = user;
  next();
});

// Role-based access control
const restrictTo = (...roles) => (req, res, next) => {
  if (!roles.includes(req.user.role)) {
    return next(new AppError("You do not have permission to perform this action.", 403));
  }
  next();
};

module.exports = { protect, restrictTo };
```

```js
// middleware/validate.js

const { validationResult } = require("express-validator");

module.exports = (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({
      success: false,
      message: "Validation failed",
      errors:  errors.array().map(e => ({ field: e.path, message: e.msg })),
    });
  }
  next();
};
```

```js
// middleware/errorHandler.js

const AppError = require("../utils/appError");

module.exports = (err, req, res, next) => {
  err.statusCode = err.statusCode || 500;
  err.status     = err.status     || "error";

  // Handle known error types
  let error = { ...err, message: err.message, name: err.name };

  if (err.name === "CastError")
    error = new AppError(`Invalid ${err.path}: ${err.value}`, 400);

  if (err.code === 11000) {
    const field = Object.keys(err.keyValue || {})[0];
    error = new AppError(`${field} already exists. Please use a different value.`, 409);
  }

  if (err.name === "ValidationError") {
    const msgs = Object.values(err.errors).map(e => e.message);
    error = new AppError(msgs.join(". "), 400);
  }

  if (err.name === "JsonWebTokenError")
    error = new AppError("Invalid token. Please log in again.", 401);

  if (err.name === "TokenExpiredError")
    error = new AppError("Token expired. Please log in again.", 401);

  // Development: send detailed error info
  if (process.env.NODE_ENV === "development") {
    return res.status(error.statusCode || 500).json({
      success: false,
      status:  error.status,
      message: error.message || err.message,
      stack:   err.stack,
    });
  }

  // Production: only expose operational errors
  if (error.isOperational) {
    return res.status(error.statusCode).json({
      success: false,
      status:  error.status,
      message: error.message,
    });
  }

  console.error("UNHANDLED ERROR:", err);
  res.status(500).json({
    success: false,
    status:  "error",
    message: "Something went wrong. Please try again later.",
  });
};
```

```js
// validators/authValidator.js

const { body } = require("express-validator");

const registerValidator = [
  body("username")
    .trim()
    .notEmpty().withMessage("Username is required")
    .isLength({ min: 3, max: 20 }).withMessage("Username must be 3–20 characters")
    .isAlphanumeric().withMessage("Username can only contain letters and numbers"),

  body("email")
    .isEmail().withMessage("Please enter a valid email")
    .normalizeEmail(),

  body("password")
    .isStrongPassword({
      minLength:    8,
      minLowercase: 1,
      minUppercase: 1,
      minNumbers:   1,
      minSymbols:   1,
    })
    .withMessage("Password must be 8+ chars with uppercase, lowercase, number and symbol"),

  body("confirmPassword")
    .notEmpty().withMessage("Please confirm your password")
    .custom((val, { req }) => {
      if (val !== req.body.password) throw new Error("Passwords do not match");
      return true;
    }),
];

const loginValidator = [
  body("email").isEmail().withMessage("Valid email required").normalizeEmail(),
  body("password").notEmpty().withMessage("Password is required"),
];

const changePasswordValidator = [
  body("currentPassword").notEmpty().withMessage("Current password is required"),
  body("newPassword")
    .isStrongPassword({ minLength: 8, minLowercase: 1, minUppercase: 1,
                        minNumbers: 1, minSymbols: 1 })
    .withMessage("New password must be strong"),
  body("confirmNewPassword")
    .custom((val, { req }) => {
      if (val !== req.body.newPassword) throw new Error("New passwords do not match");
      return true;
    }),
];

module.exports = { registerValidator, loginValidator, changePasswordValidator };
```

```js
// controllers/authController.js

const jwt        = require("jsonwebtoken");
const bcrypt     = require("bcrypt");
const User       = require("../models/User");
const catchAsync = require("../utils/catchAsync");
const AppError   = require("../utils/appError");
const { sendTokens, createAccessToken, createRefreshToken } = require("../utils/tokenUtils");

// ─── REGISTER ─────────────────────────────────────────────────────────────────
exports.register = catchAsync(async (req, res, next) => {
  const { username, email, password } = req.body;

  // Check duplicate email
  const existing = await User.findOne({ email });
  if (existing) return next(new AppError("Email already registered.", 409));

  // Create user — pre-save hook hashes the password automatically
  const user = await User.create({ username, email, password });

  // Issue tokens immediately after registration (auto-login on register)
  sendTokens(user, 201, res);
});

// ─── LOGIN ────────────────────────────────────────────────────────────────────
exports.login = catchAsync(async (req, res, next) => {
  const { email, password } = req.body;

  // Must explicitly select password (it has select: false in schema)
  const user = await User.findOne({ email }).select("+password +isActive");

  if (!user || !user.isActive) {
    // Same error for "not found" and "inactive" — don't reveal which
    return next(new AppError("Invalid email or password.", 401));
  }

  const isPasswordCorrect = await user.comparePassword(password);
  if (!isPasswordCorrect) {
    return next(new AppError("Invalid email or password.", 401));
  }

  sendTokens(user, 200, res);
});

// ─── REFRESH TOKEN ────────────────────────────────────────────────────────────
// Called when access token expires — returns a new access token
// The refresh token comes from the httpOnly cookie automatically
exports.refreshToken = catchAsync(async (req, res, next) => {
  const refreshToken = req.cookies?.refreshToken;

  if (!refreshToken) {
    return next(new AppError("No refresh token found. Please log in.", 401));
  }

  // Verify the refresh token using the REFRESH secret (different from access)
  let decoded;
  try {
    decoded = jwt.verify(refreshToken, process.env.JWT_REFRESH_SECRET);
  } catch (err) {
    // Clear invalid cookie
    res.clearCookie("refreshToken");
    if (err.name === "TokenExpiredError") {
      return next(new AppError("Refresh token expired. Please log in again.", 401));
    }
    return next(new AppError("Invalid refresh token. Please log in again.", 401));
  }

  // Confirm user still exists and is active
  const user = await User.findById(decoded.id).select("+isActive");
  if (!user || !user.isActive) {
    res.clearCookie("refreshToken");
    return next(new AppError("User no longer exists or is inactive.", 401));
  }

  // Issue a new access token
  const newAccessToken = createAccessToken(user._id, user.role);

  // Also rotate the refresh token for extra security
  const newRefreshToken = createRefreshToken(user._id);
  res.cookie("refreshToken", newRefreshToken, {
    httpOnly: true,
    secure:   process.env.NODE_ENV === "production",
    sameSite: "strict",
    maxAge:   7 * 24 * 60 * 60 * 1000,
  });

  res.status(200).json({
    success:     true,
    accessToken: newAccessToken,
    expiresIn:   process.env.JWT_ACCESS_EXPIRES_IN,
  });
});

// ─── LOGOUT ───────────────────────────────────────────────────────────────────
exports.logout = catchAsync(async (req, res, next) => {
  // Clear the refresh token cookie
  res.clearCookie("refreshToken", {
    httpOnly: true,
    secure:   process.env.NODE_ENV === "production",
    sameSite: "strict",
  });

  // The access token on the client side must be deleted by the client
  // (from localStorage or memory) — server cannot force this
  // The access token will expire naturally (15 min)

  res.status(200).json({
    success: true,
    message: "Logged out successfully. Please delete your access token.",
  });
});

// ─── GET CURRENT USER ─────────────────────────────────────────────────────────
// Protected — req.user is populated by protect middleware
exports.getMe = catchAsync(async (req, res, next) => {
  // Fetch fresh data from DB (session data could be stale)
  const user = await User.findById(req.user.id);
  if (!user) return next(new AppError("User not found.", 404));

  res.status(200).json({
    success: true,
    data:    { user },
  });
});

// ─── CHANGE PASSWORD ──────────────────────────────────────────────────────────
exports.changePassword = catchAsync(async (req, res, next) => {
  const { currentPassword, newPassword } = req.body;

  // Must select password to compare
  const user = await User.findById(req.user.id).select("+password");

  const isCorrect = await user.comparePassword(currentPassword);
  if (!isCorrect) {
    return next(new AppError("Current password is incorrect.", 401));
  }

  // Update password — pre-save hook hashes it and sets passwordChangedAt
  user.password = newPassword;
  await user.save();

  // Issue new tokens (old access tokens are now invalid because
  // passwordChangedAt was updated and the middleware checks this)
  // Also clear the refresh token cookie so they must log in on other devices
  res.clearCookie("refreshToken");

  sendTokens(user, 200, res);
});

// ─── UPDATE MY PROFILE ────────────────────────────────────────────────────────
exports.updateMe = catchAsync(async (req, res, next) => {
  // Prevent updating password through this route
  if (req.body.password) {
    return next(new AppError("Use /change-password to update your password.", 400));
  }

  // Only allow specific fields to be updated
  const allowedFields = ["username", "email"];
  const updateData = {};
  allowedFields.forEach(field => {
    if (req.body[field] !== undefined) updateData[field] = req.body[field];
  });

  const user = await User.findByIdAndUpdate(
    req.user.id,
    updateData,
    { new: true, runValidators: true }
  );

  res.status(200).json({
    success: true,
    message: "Profile updated successfully.",
    data:    { user },
  });
});

// ─── DEACTIVATE MY ACCOUNT ────────────────────────────────────────────────────
exports.deactivateMe = catchAsync(async (req, res, next) => {
  await User.findByIdAndUpdate(req.user.id, { isActive: false });

  res.clearCookie("refreshToken");

  res.status(200).json({
    success: true,
    message: "Account deactivated. Contact support to reactivate.",
  });
});

// ─── ADMIN: GET ALL USERS ─────────────────────────────────────────────────────
exports.getAllUsers = catchAsync(async (req, res, next) => {
  const users = await User.find().sort("-createdAt");

  res.status(200).json({
    success: true,
    count:   users.length,
    data:    { users },
  });
});

// ─── ADMIN: UPDATE USER ROLE ──────────────────────────────────────────────────
exports.updateUserRole = catchAsync(async (req, res, next) => {
  const { role } = req.body;

  const user = await User.findByIdAndUpdate(
    req.params.id,
    { role },
    { new: true, runValidators: true }
  );

  if (!user) return next(new AppError("User not found.", 404));

  res.status(200).json({
    success: true,
    message: `User role updated to "${role}".`,
    data:    { user },
  });
});
```

```js
// routes/authRoutes.js

const express = require("express");
const router  = express.Router();
const authController = require("../controllers/authController");
const { protect, restrictTo } = require("../middleware/authMiddleware");
const {
  registerValidator,
  loginValidator,
  changePasswordValidator,
} = require("../validators/authValidator");
const validate = require("../middleware/validate");

// ─── Public Routes ────────────────────────────────────────────────────────────
router.post("/register", registerValidator, validate, authController.register);
router.post("/login",    loginValidator,    validate, authController.login);
router.post("/refresh",  authController.refreshToken); // uses httpOnly cookie
router.post("/logout",   authController.logout);

// ─── Protected Routes (must be logged in) ────────────────────────────────────
router.use(protect); // all routes below require valid access token

router.get( "/me",               authController.getMe);
router.put( "/me",               authController.updateMe);
router.put( "/change-password",  changePasswordValidator, validate, authController.changePassword);
router.delete("/me",             authController.deactivateMe);

// ─── Admin-Only Routes ────────────────────────────────────────────────────────
router.use(restrictTo("admin")); // all routes below require admin role

router.get("/users",               authController.getAllUsers);
router.patch("/users/:id/role",    authController.updateUserRole);

module.exports = router;
```

```js
// app.js

const express      = require("express");
const cors         = require("cors");
const morgan       = require("morgan");
const rateLimit    = require("express-rate-limit");
const helmet       = require("helmet");
const cookieParser = require("cookie-parser");
const authRoutes   = require("./routes/authRoutes");
const errorHandler = require("./middleware/errorHandler");
const AppError     = require("./utils/appError");

const app = express();

// ─── Security Headers ────────────────────────────────────────────────────────
app.use(helmet());

// ─── CORS ────────────────────────────────────────────────────────────────────
app.use(cors({
  origin:      process.env.NODE_ENV === "production"
                 ? "https://yourfrontend.com"
                 : "http://localhost:5173",
  credentials: true, // allow cookies (needed for refresh token cookie)
}));

// ─── Rate Limiting ───────────────────────────────────────────────────────────
app.use("/api/auth", rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max:      20,              // max 20 auth requests per 15 min per IP
  message:  { success: false, error: "Too many requests. Please try again later." },
}));

// ─── Logging ─────────────────────────────────────────────────────────────────
if (process.env.NODE_ENV === "development") app.use(morgan("dev"));

// ─── Body Parsing ────────────────────────────────────────────────────────────
app.use(express.json({ limit: "10kb" })); // limit body to 10kb
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser()); // needed to read refresh token from cookie

// ─── Routes ──────────────────────────────────────────────────────────────────
app.use("/api/auth", authRoutes);

// Health check
app.get("/api/health", (req, res) =>
  res.json({ success: true, message: "JWT Auth API is running" })
);

// Unknown route handler
app.all("*", (req, res, next) =>
  next(new AppError(`Route ${req.method} ${req.originalUrl} not found.`, 404))
);

// Global error handler (must be last)
app.use(errorHandler);

module.exports = app;
```

```js
// server.js

require("dotenv").config();
const app       = require("./app");
const connectDB = require("./config/db");

const PORT = process.env.PORT || 3000;

connectDB().then(() => {
  app.listen(PORT, () => {
    console.log(`JWT Auth API running on http://localhost:${PORT}`);
    console.log(`Environment: ${process.env.NODE_ENV}`);
    console.log("");
    console.log("Endpoints:");
    console.log("  POST   /api/auth/register          → Register");
    console.log("  POST   /api/auth/login             → Login (get tokens)");
    console.log("  POST   /api/auth/refresh           → Refresh access token");
    console.log("  POST   /api/auth/logout            → Logout");
    console.log("  GET    /api/auth/me                → Get my profile (protected)");
    console.log("  PUT    /api/auth/me                → Update profile (protected)");
    console.log("  PUT    /api/auth/change-password   → Change password (protected)");
    console.log("  DELETE /api/auth/me                → Deactivate account (protected)");
    console.log("  GET    /api/auth/users             → All users (admin only)");
    console.log("  PATCH  /api/auth/users/:id/role   → Update role (admin only)");
  });
});
```

---

## 13. Testing the API — Sample Requests

```
# Register
POST /api/auth/register
Content-Type: application/json
{
  "username": "pranav",
  "email": "pranav@gmail.com",
  "password": "Secret@123",
  "confirmPassword": "Secret@123"
}
→ Response: { success: true, accessToken: "eyJ...", data: { user } }
→ Cookie set: refreshToken=eyJ... (httpOnly)

# Login
POST /api/auth/login
Content-Type: application/json
{ "email": "pranav@gmail.com", "password": "Secret@123" }
→ Same response shape

# Access protected route
GET /api/auth/me
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
→ Returns current user profile

# Refresh token (when access token expires)
POST /api/auth/refresh
(no body needed — refresh token sent automatically via cookie)
→ { success: true, accessToken: "new_token..." }

# Change password
PUT /api/auth/change-password
Authorization: Bearer eyJ...
Content-Type: application/json
{
  "currentPassword": "Secret@123",
  "newPassword": "NewSecret@456",
  "confirmNewPassword": "NewSecret@456"
}

# Admin: update user role
PATCH /api/auth/users/64abc.../role
Authorization: Bearer eyJ...  ← must be admin token
Content-Type: application/json
{ "role": "editor" }
```

---

## 14. Complete Flow Trace

```
POST /api/auth/login { email, password }
         │
         ▼
  express.json()     → parses body into req.body
         │
         ▼
  rateLimit()        → checks IP hasn't exceeded 20 req/15min
         │
         ▼
  loginValidator     → validates email format and password presence
         │
         ▼
  validate()         → if errors → 400 response (stops here)
         │
         ▼
  authController.login()
    │
    ├─ User.findOne({ email }).select("+password")
    │    Found: { _id: "64abc", email, password: "$2b$12$...", role: "user" }
    │
    ├─ bcrypt.compare("Secret@123", "$2b$12$...")  → true ✓
    │
    ├─ createAccessToken("64abc", "user")
    │    → jwt.sign({ id: "64abc", role: "user" }, ACCESS_SECRET, { expiresIn: "15m" })
    │    → "eyJhbGc..." (expires in 15 minutes)
    │
    ├─ createRefreshToken("64abc")
    │    → jwt.sign({ id: "64abc" }, REFRESH_SECRET, { expiresIn: "7d" })
    │    → "eyJhbGc..." (expires in 7 days)
    │
    ├─ res.cookie("refreshToken", refreshToken, { httpOnly: true, ... })
    │    → browser receives: Set-Cookie: refreshToken=eyJ...; HttpOnly; SameSite=Strict
    │
    └─ res.json({ success: true, accessToken, data: { user } })

─────────────────────────────────────────────────────

GET /api/auth/me
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
         │
         ▼
  protect middleware
    │
    ├─ Extract token from Authorization header: "Bearer eyJ..."
    │
    ├─ jwt.verify("eyJ...", ACCESS_SECRET)
    │    → decoded: { id: "64abc", role: "user", iat: 17170..., exp: 17170...+900 }
    │    → exp not reached → valid ✓
    │
    ├─ User.findById("64abc")  → user found, isActive: true ✓
    │
    ├─ user.passwordChangedAfter(decoded.iat) → false ✓
    │
    └─ req.user = user → next()
         │
         ▼
  authController.getMe()
    └─ User.findById(req.user.id) → fresh data from DB
    └─ res.json({ success: true, data: { user } })

─────────────────────────────────────────────────────

15 minutes later: access token expires

GET /api/auth/me
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...  (expired)
         │
         ▼
  protect middleware
    ├─ jwt.verify(expiredToken, ACCESS_SECRET)
    │    → throws TokenExpiredError
    └─ next(new AppError("Access token expired...", 401))
         │
         ▼
  Client receives: 401 "Access token expired"

  Client sends:
POST /api/auth/refresh
Cookie: refreshToken=eyJ...  (sent automatically — httpOnly cookie)
         │
         ▼
  authController.refreshToken()
    ├─ jwt.verify(refreshToken, REFRESH_SECRET) → decoded: { id: "64abc" }
    ├─ User.findById("64abc") → still exists ✓
    ├─ createAccessToken("64abc", "user") → new 15-minute token
    ├─ createRefreshToken("64abc") → new 7-day refresh (rotation)
    ├─ res.cookie("refreshToken", newRefresh, { httpOnly: true })
    └─ res.json({ success: true, accessToken: "new_eyJ..." })
```

---

## 15. Security Checklist for JWT Auth

```
Secrets and Configuration:
  ✓ JWT_ACCESS_SECRET is long (32+ chars), random, stored in .env
  ✓ JWT_REFRESH_SECRET is DIFFERENT from JWT_ACCESS_SECRET
  ✓ Neither secret is committed to version control
  ✓ Access token expiry is short (15m–1h)
  ✓ Refresh token expiry is reasonable (7–30 days)

Token Handling:
  ✓ Refresh token stored in httpOnly cookie
  ✓ Access token stored in memory or localStorage (with XSS awareness)
  ✓ CORS configured with credentials: true for cookie support
  ✓ sameSite: "strict" on refresh token cookie

Validation on Every Request:
  ✓ Signature verified with jwt.verify() (never jwt.decode() for auth)
  ✓ Token expiry checked automatically by jwt.verify()
  ✓ User existence confirmed with a DB query
  ✓ Account isActive status checked
  ✓ passwordChangedAt compared against token iat

Password Security:
  ✓ Passwords hashed with bcrypt (cost factor 12)
  ✓ Password field has select: false (never returned in queries)
  ✓ Same error message for wrong email and wrong password
  ✓ Old tokens invalidated after password change (via passwordChangedAt)

Operations:
  ✓ Rate limiting on auth routes
  ✓ helmet() for security headers
  ✓ Body size limit on express.json()
  ✓ Refresh tokens rotated on use
  ✓ refresh token cookie cleared on logout
```

---

## 16. Quick Reference — The Whole System in One View

```
                    ┌─────────────────────────────────────┐
                    │              CLIENT                  │
                    │                                      │
                    │  accessToken  → memory / localStorage│
                    │  refreshToken → httpOnly cookie       │
                    └──────────────┬──────────────────────┘
                                   │
                    ┌──────────────▼──────────────────────┐
                    │              SERVER                  │
                    │                                      │
                    │  POST /auth/login                    │
                    │    verify password (bcrypt)          │
                    │    sign accessToken  (15m)           │
                    │    sign refreshToken (7d)            │
                    │    → accessToken in JSON body        │
                    │    → refreshToken in httpOnly cookie │
                    │                                      │
                    │  GET /api/protected                  │
                    │    read Authorization header         │
                    │    jwt.verify(token, ACCESS_SECRET)  │
                    │    fetch user from DB                │
                    │    req.user = user → next()          │
                    │                                      │
                    │  POST /auth/refresh                  │
                    │    read refreshToken cookie          │
                    │    jwt.verify(token, REFRESH_SECRET) │
                    │    issue new accessToken             │
                    │    rotate refreshToken               │
                    │                                      │
                    │  POST /auth/logout                   │
                    │    clear refreshToken cookie         │
                    │    client deletes accessToken        │
                    └─────────────────────────────────────┘
```

---

> ✅ **You now have a complete, production-grade understanding of JWT authentication — the theory of why JWT exists, the structure of a token, the signed-not-encrypted distinction, the access + refresh token pattern, token revocation strategies, and a fully implemented auth system with password change invalidation and role-based access control.**
> 
> **The natural next topic is connecting this JWT auth system to a resource API — for example, protecting your Blog Post API or Readnest book routes so that only authenticated users can create content and only admins can delete it. That ties together everything from these notes with the Express API Development notes.**