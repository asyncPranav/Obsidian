
---

## Big Picture

```text
Frontend sends request to /refresh-token
                ↓
        Refresh Token Cookie
                ↓
      Get token from req.cookies
                ↓
        Is token present?
          ↙           ↘
        NO             YES
        ↓               ↓
      401         Verify Refresh JWT
                        ↓
                Get decoded data
                ┌───────────────┐
                │ sub = user ID │
                │ sid = session │
                └───────────────┘
                        ↓
              Find matching session
                        ↓
                Session exists?
                 ↙          ↘
               NO            YES
               ↓              ↓
             401       Check revoked
                              ↓
                       Revoked?
                       ↙      ↘
                     YES       NO
                      ↓         ↓
                    401    Check expiration
                                ↓
                          Session expired?
                           ↙          ↘
                         YES           NO
                          ↓             ↓
                        401       bcrypt.compare()
                                      ↓
                              Token is valid?
                               ↙          ↘
                             NO            YES
                             ↓              ↓
                           401        Generate NEW
                                      Access Token
                                          ↓
                                  Generate NEW
                                  Refresh Token
                                          ↓
                                    Hash NEW
                                  Refresh Token
                                          ↓
                                  Update Session
                                          ↓
                                  Save Session
                                          ↓
                                  Set NEW Refresh
                                  Token Cookie
                                          ↓
                                  Return NEW
                                  Access Token
                                          ↓
                                       200 OK
```

---

# Step-by-Step Flow

## Step 1 — Get Refresh Token from Cookie

```js
const refreshToken = req.cookies.refreshToken;
```

The browser automatically sends the HTTP-only cookie with the request.

We read it using:

```js
req.cookies.refreshToken
```

### If there is no refresh token:

```js
if (!refreshToken) {
  throw new ApiError(401, "No refresh token provided");
}
```

Flow:

```text
Cookie
  ↓
req.cookies.refreshToken
  ↓
No token?
  ↓
401 Unauthorized
```

---

# Step 2 — Verify the Refresh Token

```js
const decoded = token.verifyRefreshToken(refreshToken);
```

This verifies:

- JWT signature
    
- JWT expiration
    
- Token integrity
    

If verification fails, an error is thrown and goes to:

```js
catch (error) {
  next(error);
}
```

If successful, we get the payload.

For example:

```js
{
  sub: "USER123",
  sid: "SESSION456",
  iat: ...,
  exp: ...
}
```

Remember:

```text
sub → WHO? → User ID

sid → WHICH SESSION? → Session ID
```

---

# Step 3 — Find the Session

```js
const session = await sessionModel.findOne({
  _id: decoded.sid,
  user: decoded.sub,
});
```

We use:

```text
decoded.sid
      +
decoded.sub
```

to find the correct session.

We are **not searching the database using the raw refresh token**.

We first identify the session using:

```text
sid + sub
```

Then we verify that the actual refresh token belongs to that session using `bcrypt.compare()`.

---

# Step 4 — Check Whether Session Exists

```js
if (!session) {
  throw new ApiError(401, "Session not found");
}
```

If the session was deleted or the token points to a nonexistent session:

```text
Session not found
      ↓
401 Unauthorized
```

---

# Step 5 — Check Whether Session Was Revoked

```js
if (session.revoked) {
  throw new ApiError(401, "Session has been revoked");
}
```

For example, during logout:

```text
Session
revoked = true
```

Then even if the refresh JWT itself is cryptographically valid, we reject it.

```text
Valid JWT
   ↓
Session revoked?
   ↓
YES
   ↓
Reject
```

---

# Step 6 — Check Session Expiration

```js
if (session.expiresAt < new Date()) {
  throw new ApiError(401, "Session has expired");
}
```

We check the expiration stored in the database.

```text
Current time > session.expiresAt
        ↓
Session expired
        ↓
401 Unauthorized
```

---

# Step 7 — Compare the Actual Refresh Token

```js
const isRefreshTokenValid = await bcrypt.compare(
  refreshToken,
  session.refreshToken,
);
```

This is very important.

The database contains:

```text
hash(refreshToken)
```

It does **not** contain the original refresh token.

So:

```text
Cookie
  ↓
Actual Refresh Token
  ↓
bcrypt.compare()
  ↓
Database Hash
```

If the comparison fails:

```js
if (!isRefreshTokenValid) {
  throw new ApiError(401, "Invalid refresh token");
}
```

---

# Step 8 — Generate a New Access Token

```js
const newAccessToken = token.generateAccessToken(
  decoded.sub,
  decoded.sid,
);
```

The new access token contains the same:

```text
sub → user ID
sid → session ID
```

We are **not creating a new session**.

We are refreshing the credentials for the existing session.

---

# Step 9 — Generate a New Refresh Token

```js
const newRefreshToken = token.generateRefreshToken(
  decoded.sub,
  decoded.sid,
);
```

This creates a new refresh token.

Important:

```text
OLD refresh token → NEW refresh token
```

But:

```text
OLD session → SAME session
```

The `sid` stays the same.

Example:

```text
Before:

Session ID = ABC123
Refresh Token = RT-1


After rotation:

Session ID = ABC123
Refresh Token = RT-2
```

---

# Step 10 — Hash the New Refresh Token

```js
const hashedNewRefreshToken = await bcrypt.hash(
  newRefreshToken,
  10,
);
```

We never store the raw refresh token in the database.

Instead:

```text
New Refresh Token
        ↓
     bcrypt
        ↓
New Refresh Token Hash
```

---

# Step 11 — Update Existing Session

```js
session.refreshToken = hashedNewRefreshToken;
```

We replace:

```text
OLD hash
```

with:

```text
NEW hash
```

We also extend the session expiration:

```js
session.expiresAt = new Date(
  Date.now() + 15 * 24 * 60 * 60 * 1000
);
```

So the session now contains:

```text
Session
├── _id
├── user
├── refreshToken = hash(NEW refresh token)
├── revoked = false
└── expiresAt = 15 days from now
```

---

# Step 12 — Save the Session

```js
await session.save();
```

This is important because assigning:

```js
session.refreshToken = hashedNewRefreshToken;
```

only changes the Mongoose document in memory.

`save()` actually writes the changes to MongoDB.

---

# Step 13 — Set New Refresh Token Cookie

```js
cookie.setRefreshTokenCookie(res, newRefreshToken);
```

Now the browser receives the new refresh token.

We have:

```text
Cookie
   ↓
NEW refresh token
```

and:

```text
Database
   ↓
hash(NEW refresh token)
```

They are synchronized.

---

# Step 14 — Return New Access Token

```js
return res.status(200).json({
  message: "Access token refreshed successfully",
  accessToken: newAccessToken,
});
```

The frontend receives:

```json
{
  "message": "Access token refreshed successfully",
  "accessToken": "NEW_ACCESS_TOKEN"
}
```

The refresh token is **not returned in JSON**.

It is stored in the HTTP-only cookie.

---

# Final Complete Flow

```text
                    /refresh-token
                           ↓
                Get refreshToken cookie
                           ↓
                   Token exists?
                    ↙          ↘
                  NO            YES
                  ↓              ↓
                401       Verify refresh JWT
                                 ↓
                       decoded.sub + decoded.sid
                                 ↓
                         Find existing session
                                 ↓
                          Session exists?
                           ↙          ↘
                         NO            YES
                         ↓              ↓
                       401       Check revoked
                                        ↓
                                 Check expiration
                                        ↓
                                 bcrypt.compare()
                                        ↓
                                Token matches hash?
                                  ↙          ↘
                                NO            YES
                                ↓              ↓
                              401       Generate NEW
                                        Access Token
                                             +
                                        Generate NEW
                                        Refresh Token
                                             ↓
                                      Hash NEW Refresh
                                          Token
                                             ↓
                                      Update Session
                                             ↓
                                        Save DB
                                             ↓
                                      Set NEW Cookie
                                             ↓
                                    Return NEW Access
                                         Token
                                             ↓
                                          200 OK
```

# The Most Important Idea

There are **three things working together**:

```text
1. JWT
   ↓
   Proves the token was signed by our server
   and has not expired.

2. Session
   ↓
   Tells us whether this login/session
   is still allowed.

3. bcrypt.compare()
   ↓
   Confirms that the exact refresh token
   sent by the client matches the token
   registered for that session.
```

And during rotation:

```text
OLD Refresh Token
       ↓
     verify
       ↓
   find session
       ↓
   compare hash
       ↓
   generate NEW Refresh Token
       ↓
   hash NEW token
       ↓
   replace DB hash
       ↓
   set NEW cookie
```

### One-line memory trick

> **Verify the old token → validate the session → compare the token hash → create new tokens → replace the refresh-token hash → set the new cookie.**