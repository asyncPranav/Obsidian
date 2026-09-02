
---


# Refresh Token Rotation — Problem, Solution, and Correct Order

## 1. The Problem in the Old Code

Our original refresh-token code was:

```js

// auth.controller.js -> in `refreshToken` controller

// Generate a new access token
const newAccessToken = token.generateAccessToken(
  decoded.sub,
  decoded.sid,
);

// Generate a new refresh token
const newRefreshToken = token.generateRefreshToken(
  decoded.sub,
  decoded.sid,
);

// Set the new refresh token in the HTTP-only cookie
cookie.setRefreshTokenCookie(res, newRefreshToken);
```

The problem is that we generated a **new refresh token** but did not update its hash in the database.

### What happens?

Initially:

```text
Cookie:
Refresh Token A

Database:
hash(Refresh Token A)
```

After `/refresh` with the old code:

```text
Cookie:
Refresh Token B

Database:
hash(Refresh Token A)
```

Now the client sends Refresh Token B on the next refresh request.

Our code does:

```js
bcrypt.compare(
  refreshToken,
  session.refreshToken,
);
```

Which becomes:

```text
bcrypt.compare(
  Refresh Token B,
  hash(Refresh Token A)
)
```

Result:

```text
false
```

So the next refresh request fails.

---

# 2. The Solution — Refresh Token Rotation

When we generate a new refresh token, we must also:

1. Generate the new refresh token.
    
2. Hash the new refresh token.
    
3. Replace the old hash in the session.
    
4. Save the session.
    
5. Put the new refresh token in the cookie.
    

The flow is:

```text
Old Refresh Token
        ↓
Verify JWT
        ↓
Find Session using sid + sub
        ↓
Check revoked
        ↓
Check expiry
        ↓
bcrypt.compare()
        ↓
Generate NEW Refresh Token
        ↓
Hash NEW Refresh Token
        ↓
Replace old hash in DB
        ↓
Save Session
        ↓
Generate NEW Access Token
        ↓
Set NEW Refresh Token Cookie
        ↓
Return NEW Access Token
```

---

# 3. Correct New Code

```js
const refreshToken = async (req, res, next) => {
  try {
    // Get the refresh token from the HTTP-only cookie
    const refreshToken = req.cookies.refreshToken;

    if (!refreshToken) {
      throw new ApiError(401, "No refresh token provided");
    }

    // Verify the refresh token
    // jwt.verify() checks the token's signature and expiration
    const decoded = token.verifyRefreshToken(refreshToken);

    // Find the session using the user ID and session ID
    // sub = user ID
    // sid = session ID
    const session = await sessionModel.findOne({
      _id: decoded.sid,
      user: decoded.sub,
    });

    // If no matching session exists, reject the request
    if (!session) {
      throw new ApiError(401, "Session not found");
    }

    // Check whether the session has been revoked
    if (session.revoked) {
      throw new ApiError(401, "Session has been revoked");
    }

    // Check whether the session has expired
    if (session.expiresAt < new Date()) {
      throw new ApiError(401, "Session has expired");
    }

    // Compare the actual refresh token from the cookie
    // with the hashed refresh token stored in the database
    const isRefreshTokenValid = await bcrypt.compare(
      refreshToken,
      session.refreshToken,
    );

    if (!isRefreshTokenValid) {
      throw new ApiError(401, "Invalid refresh token");
    }

    // Generate a NEW refresh token
    const newRefreshToken = token.generateRefreshToken(
      decoded.sub,
      decoded.sid,
    );

    // Hash the NEW refresh token before storing it in the database
    const hashedRefreshToken = await bcrypt.hash(
      newRefreshToken,
      10,
    );

    // Replace the old refresh token hash
    // with the hash of the new refresh token
    session.refreshToken = hashedRefreshToken;

    // Save the updated session
    await session.save();

    // Generate a NEW access token
    const newAccessToken = token.generateAccessToken(
      decoded.sub,
      decoded.sid,
    );

    // Set the NEW refresh token in the HTTP-only cookie
    cookie.setRefreshTokenCookie(res, newRefreshToken);

    // Return the new access token to the frontend
    return res.status(200).json({
      message: "Access token refreshed successfully",
      accessToken: newAccessToken,
    });
  } catch (error) {
    next(error);
  }
};
```

---

# 4. Does the New Access Token Have to Be Generated After the New Refresh Token?

**No.**

This is an important point.

These two operations are independent:

```js
const newAccessToken = token.generateAccessToken(
  decoded.sub,
  decoded.sid,
);

const newRefreshToken = token.generateRefreshToken(
  decoded.sub,
  decoded.sid,
);
```

or:

```js
const newRefreshToken = token.generateRefreshToken(
  decoded.sub,
  decoded.sid,
);

const newAccessToken = token.generateAccessToken(
  decoded.sub,
  decoded.sid,
);
```

Both are valid.

The access token does **not** need the new refresh token.

The refresh token does **not** need the access token.

Both only need:

```text
userId
sessionId
```

So this is perfectly valid:

```js
// Generate new access token
const newAccessToken = token.generateAccessToken(
  decoded.sub,
  decoded.sid,
);

// Generate new refresh token
const newRefreshToken = token.generateRefreshToken(
  decoded.sub,
  decoded.sid,
);
```

---

# 5. Then Why Did We Generate the Refresh Token First?

Because we need to update the database with the **new refresh-token hash**.

So this sequence is logically convenient:

```text
Generate new refresh token
        ↓
Hash it
        ↓
Update database
        ↓
Save session
        ↓
Generate new access token
```

But generating the access token before the refresh token would also work.

For example:

```js
// Generate new access token
const newAccessToken = token.generateAccessToken(
  decoded.sub,
  decoded.sid,
);

// Generate new refresh token
const newRefreshToken = token.generateRefreshToken(
  decoded.sub,
  decoded.sid,
);

// Hash the new refresh token
const hashedRefreshToken = await bcrypt.hash(
  newRefreshToken,
  10,
);

// Update the database
session.refreshToken = hashedRefreshToken;

await session.save();

// Set the new refresh token cookie
cookie.setRefreshTokenCookie(res, newRefreshToken);
```

This is also correct.

### Therefore:

> **The order between access-token generation and refresh-token generation does not matter.**

What matters is that:

```text
NEW refresh token
       ↓
hash
       ↓
database
```

must happen before we consider the rotation complete.

---

# 6. Why Is Register Different?

Your `register()` code is correct:

```js
// Generate random session ID for the user session
const sessionId = new mongoose.Types.ObjectId();

// Generate refresh token
const refreshToken = token.generateRefreshToken(
  newUser._id,
  sessionId,
);

// Hash refresh token
const hashedRefreshToken = await bcrypt.hash(
  refreshToken,
  10,
);

// Create session
const session = await sessionModel.create({
  _id: sessionId,
  user: newUser._id,
  refreshToken: hashedRefreshToken,
  ip: req.ip,
  userAgent: req.get("User-Agent"),
  expiresAt: new Date(Date.now() + 15 * 24 * 60 * 60 * 1000),
});

// Generate access token
const accessToken = token.generateAccessToken(
  newUser._id,
  session._id,
);
```

There is a **real dependency** here.

Our refresh token contains:

```js
{
  sub: userId,
  sid: sessionId
}
```

Therefore, we need to know the `sessionId` **before** generating the refresh token.

That's why we do:

```js
const sessionId = new mongoose.Types.ObjectId();
```

first.

Then:

```text
Create sessionId
      ↓
Generate refresh token using sessionId
      ↓
Hash refresh token
      ↓
Create DB session using same sessionId
      ↓
Generate access token using same sessionId
```

---

# 7. Why Can't We Generate the Refresh Token First in Register?

Because our refresh token contains:

```js
sid: sessionId
```

For example:

```js
token.generateRefreshToken(
  newUser._id,
  sessionId,
);
```

If we don't have `sessionId` yet, we cannot correctly generate this token.

So this would be wrong:

```js
// We don't have sessionId yet
const refreshToken = token.generateRefreshToken(
  newUser._id,
  sessionId,
);
```

because `sessionId` hasn't been created.

Instead:

```js
const sessionId = new mongoose.Types.ObjectId();

const refreshToken = token.generateRefreshToken(
  newUser._id,
  sessionId,
);
```

Now everything uses the same ID:

```text
sessionId = ABC123

Session:
_id = ABC123

Refresh Token:
sid = ABC123

Access Token:
sid = ABC123
```

---

# 8. Register vs Refresh — The Important Difference

### Register/Login

We are **creating a session for the first time**.

Therefore:

```text
Need sessionId
      ↓
Generate sessionId
      ↓
Generate refresh token using sessionId
      ↓
Store session
      ↓
Generate access token
```

### Refresh

The session **already exists**.

We get the session ID from the existing refresh token:

```js
const decoded = token.verifyRefreshToken(refreshToken);
```

Then:

```js
decoded.sid
```

gives us the existing session ID.

Therefore:

```text
Existing refresh token
        ↓
decoded.sid
        ↓
Find existing session
        ↓
Generate new refresh token using same sid
```

---

# 9. Why Does the New Refresh Token Keep the Same `sid`?

Because we are rotating the **token**, not creating a new login session.

Suppose:

```text
Session A
_id = ABC123
```

First refresh token:

```text
Refresh Token 1
{
  sub: User123,
  sid: ABC123
}
```

After rotation:

```text
Refresh Token 2
{
  sub: User123,
  sid: ABC123
}
```

The token changed:

```text
Refresh Token 1 → Refresh Token 2
```

But the session did not change:

```text
Session ABC123 → Session ABC123
```

So:

> **Same session ID, new refresh token.**

---

# 10. The Complete Rotation Example

Initially:

```text
Session ABC

Database:
refreshToken = hash(RT-1)

Cookie:
RT-1
```

Client calls:

```text
POST /refresh-token
```

Server verifies:

```text
RT-1
 ↓
JWT verification
 ↓
sub = User123
sid = ABC
```

Finds:

```text
Session ABC
```

Then:

```text
bcrypt.compare(
    RT-1,
    hash(RT-1)
)
```

Result:

```text
true
```

Now generate:

```text
RT-2
```

Hash it:

```text
hash(RT-2)
```

Update database:

```text
Session ABC

refreshToken = hash(RT-2)
```

Set cookie:

```text
Cookie = RT-2
```

Generate:

```text
Access Token 2
```

Return:

```json
{
  "accessToken": "Access-Token-2"
}
```

Now:

```text
Cookie:
RT-2

Database:
hash(RT-2)
```

The system is synchronized again.

---

# 11. One More Important Point

The **access token is not stored in the session database**.

We only store the refresh-token hash:

```js
{
  _id: sessionId,
  user: userId,
  refreshToken: hashedRefreshToken,
  revoked: false,
  expiresAt: ...
}
```

The access token is short-lived and stored in frontend memory.

The refresh token is long-lived and is tied to the database session.

So the architecture is:

```text
ACCESS TOKEN
├── Short-lived
├── Frontend memory
├── Authorization header
└── Not stored in DB

REFRESH TOKEN
├── Long-lived
├── HTTP-only cookie
├── Hashed in DB
└── Rotated when /refresh is called

SESSION
├── Stored in MongoDB
├── Has session ID
├── Belongs to a user
├── Stores refresh-token hash
├── Can be revoked
└── Can expire
```

## Final Rule to Remember

There are two different concepts:

### Dependency of data

```text
sessionId must exist
        ↓
refresh token can be generated
```

### Order of operations

```text
Generate access token
Generate refresh token
```

or

```text
Generate refresh token
Generate access token
```

Both are fine.

The **real required order during rotation** is:

```text
Verify old refresh token
        ↓
Find + validate session
        ↓
Compare old refresh token
        ↓
Generate new refresh token
        ↓
Hash new refresh token
        ↓
Update DB
        ↓
Set new refresh-token cookie
```

The access token can be generated before or after these steps because it does not depend on the new refresh token.