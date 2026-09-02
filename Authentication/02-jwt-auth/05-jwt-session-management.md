

---



# JWT Session Management — Refresh Token + Session ID

## 1. Why do we need a Session?

JWT access tokens are **stateless**. Once generated, the server doesn't normally store them in the database.

But we want features like:

- Logout from one device
    
- Logout from all devices
    
- Revoke a particular login session
    
- View active sessions/devices
    

For this, we maintain a **Session collection** in MongoDB.

### Example

```text
User A

├── Laptop  → Session 1
├── Phone   → Session 2
└── Browser → Session 3
```

**One Session document represents one login session/device.**

---

## 2. What does a Session store?

Our Session model contains:

```text
user
refreshToken (hashed)
ip
userAgent
revoked
expiresAt
createdAt
updatedAt
```

### Important fields

|Field|Purpose|
|---|---|
|`user`|Which user owns this session|
|`refreshToken`|Hashed refresh token associated with session|
|`ip`|IP information for session/device monitoring|
|`userAgent`|Browser/device information|
|`revoked`|Whether this session has been invalidated|
|`expiresAt`|When the session expires|

---

## 3. Why hash the Refresh Token?

Never store the raw refresh token in the database when we can avoid it.

```text
Raw Refresh Token
       ↓
     bcrypt
       ↓
Hashed Token
       ↓
MongoDB
```

The client keeps the **real refresh token** inside an HttpOnly cookie.

MongoDB stores only the **hash**.

### Why?

If the database is leaked:

```text
Raw token stored → attacker may use it directly ❌

Hash stored → attacker cannot directly use the hash as the token ✅
```

When the refresh request comes later:

```text
Cookie refresh token
        ↓
bcrypt.compare()
        ↓
Database hash
```

If they match → session is valid.

---

# 4. Our Current Problem: How do we identify a particular session?

Now we have introduced the `Session` model.

A user can have multiple sessions:

```text
User A

Session 1 → Laptop
Session 2 → Phone
Session 3 → Browser
```

All three sessions belong to the same user.

So `user` alone is not enough to identify **which login session** is being used.

We therefore need a unique **Session ID**.

We will put this Session ID inside the JWT:

```json
{
  "sub": "userId",
  "sid": "sessionId"
}
```

Where:

```text
sub → User ID
sid → Session ID
```

This allows the server to know:

> "This token belongs to User A and was issued for Session 2."

---

# 5. Our Existing Session Model

We currently have:

```js
import mongoose from "mongoose";

const sessionSchema = new mongoose.Schema(
  {
    user: {
      type: mongoose.Schema.Types.ObjectId,
      ref: "User",
      required: [true, "User is required"],
    },

    refreshToken: {
      type: String,
      required: [true, "Refresh token is required"],
    },

    ip: {
      type: String,
    },

    userAgent: {
      type: String,
    },

    revoked: {
      type: Boolean,
      default: false,
    },

    expiresAt: {
      type: Date,
      required: [true, "Session expiry is required"],
    },
  },
  {
    timestamps: true,
  },
);

const Session = mongoose.model("Session", sessionSchema);

export default Session;
```

### Important

The MongoDB document automatically gets its own:

```text
_id
```

That `_id` can serve as our **Session ID**.

So conceptually:

```text
Session document
        ↓
     _id
        ↓
   Session ID
```

---

# 6. Our Existing Token Code

Currently our `utils/token.js` looks like:

```js
import jwt from "jsonwebtoken";
import config from "../config/config.js";

const generateAccessToken = (userId) => {
  return jwt.sign(
    { sub: userId.toString() },
    config.jwtSecret,
    { expiresIn: "15m" },
  );
};

const generateRefreshToken = (userId) => {
  return jwt.sign(
    { sub: userId.toString() },
    config.refreshSecret,
    { expiresIn: "15d" },
  );
};

export {
  generateAccessToken,
  generateRefreshToken,
};
```

Currently the access token only contains:

```json
{
  "sub": "userId"
}
```

It does **not** contain the Session ID.

---

# 7. Our Existing Register Token/Session Code

Currently we are moving toward:

```js
// Generate JWT access token and refresh token

const refreshToken = token.generateRefreshToken(newUser._id);

const accessToken = token.generateAccessToken(newUser._id);

// Create hash of refresh token to store in database
const hashedRefreshToken = await bcrypt.hash(refreshToken, 10);

// Create a session

const session = await sessionModel.create({
  user: newUser._id,
  refreshToken: hashedRefreshToken,
  ip: req.ip,
  userAgent: req.get("User-Agent"),
  expiresAt: new Date(
    Date.now() + 15 * 24 * 60 * 60 * 1000
  ),
});

// Set the refresh token in an HTTP-only cookie

cookie.setRefreshTokenCookie(res, refreshToken);
```

The problem is:

```text
Access Token
    ↓
sub = userId
```

but we don't yet have:

```text
sid = sessionId
```

Therefore the access token cannot tell us **which session it belongs to**.

---

# 8. What are we going to change?

We need to establish this relationship:

```text
                User
                 │
                 ↓
             Session
             ┌───────┐
             │ _id   │ ← Session ID
             │ user  │
             │ hash  │
             └───────┘
                 ↑
                 │
          Access Token
          ┌────────────┐
          │ sub        │ → User ID
          │ sid        │ → Session ID
          └────────────┘
```

The same Session ID must be known by both:

```text
Session document
        +
Access Token
```

---

# 9. The Problem With Simply Creating the Session First

We might think:

```text
Create Session
      ↓
Get session._id
      ↓
Generate Access Token with session._id
```

But the Session itself contains the refresh token.

And our refresh token is generated before creating the session.

So there is a dependency:

```text
Refresh Token
      ↓
Create Session
      ↓
Session _id
      ↓
Access Token with sid
```

But we can solve this cleanly by generating a Session ID ourselves first.

---

# 10. Next Step — Generate Session ID

Our next step is to generate a unique Session ID before creating the tokens/session.

Conceptually:

```text
Generate sessionId
       ↓
Generate refresh token
       ↓
Hash refresh token
       ↓
Create Session using sessionId
       ↓
Generate access token using:
    sub = userId
    sid = sessionId
       ↓
Set refresh-token cookie
       ↓
Send access token
```

The important relationship becomes:

```text
sessionId
   │
   ├────────→ Session
   │
   └────────→ Access Token (`sid`)
```

---

# 11. Why are we doing this?

Because later we want to support:

### Logout current device

```text
Access Token
    ↓
sid
    ↓
Session
    ↓
revoked = true
```

Only that session is revoked.

### Logout all devices

```text
User ID
   ↓
Find all sessions
   ↓
Revoke all
```

Example:

```text
User A

Session 1 ❌
Session 2 ❌
Session 3 ❌
```

---

# 12. Mental Model to Remember

```text
sub = WHO?
sid = WHICH SESSION?
```

For example:

```json
{
  "sub": "64abc...",
  "sid": "72xyz..."
}
```

means:

```text
User → 64abc...

Login Session → 72xyz...
```

This is the connection between our **JWT system** and our **database-backed session management**.

**Next coding step:** modify `token.js` and decide how we'll generate the `sessionId` cleanly before wiring it into `register()`.