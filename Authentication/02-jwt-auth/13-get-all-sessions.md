
---


# Get All Active Sessions / Devices where user is logged in

## 1. What are we building?

We want an authenticated user to be able to see the login sessions currently associated with their account.

Endpoint:

```http
GET /api/auth/sessions
```

Example:

```json
{
  "message": "Active sessions fetched successfully",
  "sessions": [
    {
      "id": "68abc...",
      "ip": "192.168.1.10",
      "userAgent": "Chrome on Windows",
      "createdAt": "2026-09-03T...",
      "expiresAt": "2026-09-18T...",
      "current": true
    },
    {
      "id": "68def...",
      "ip": "10.0.0.5",
      "userAgent": "Chrome on Android",
      "createdAt": "2026-09-02T...",
      "expiresAt": "2026-09-17T...",
      "current": false
    }
  ]
}
```

The frontend can use this to show:

```text
Your active sessions

✓ Chrome / Windows     Current
  Chrome / Android
  Firefox / Mac
```

---

# 2. Why can we do this?

Every successful login creates a session:

```text
User
 ├── Session A → Laptop
 ├── Session B → Phone
 └── Session C → Tablet
```

Our `Session` collection already stores information about these sessions:

```text
Session
├── _id
├── user
├── refreshToken
├── ip
├── userAgent
├── revoked
├── expiresAt
└── timestamps
```

Therefore, we don't need another model.

We simply query the user's sessions.

---

# 3. Which sessions should we return?

We normally want **active sessions**, not sessions that have already been revoked.

So the query is:

```js
{
  user: req.user.sub,
  revoked: false
}
```

This means:

> Find sessions belonging to the authenticated user that have not been revoked.

For example:

```text
Database

Session A → user123 → revoked: false
Session B → user123 → revoked: false
Session C → user123 → revoked: true
Session D → user456 → revoked: false
```

For `user123`, the endpoint returns:

```text
Session A
Session B
```

It does not return:

```text
Session C → revoked
Session D → belongs to another user
```

---

# 4. Why does the route require authentication?

The route is:

```js
router.get("/sessions", authenticate, getSessions);
```

It must be protected.

The `authenticate` middleware verifies the access token and creates:

```js
req.user = {
  sub: userId,
  sid: sessionId
};
```

We use:

```js
req.user.sub
```

to determine **which user's sessions to retrieve**.

We must never allow the client to simply send:

```http
GET /api/auth/sessions?userId=123
```

and trust that value.

The authenticated identity comes from the verified access token.

---

# 5. Current session identification

The response should tell the user which session represents their **current device/session**.

We already have:

```js
req.user.sid
```

which identifies the session associated with the current access token.

For every returned session:

```js
session._id.toString() === req.user.sid
```

If it is true:

```json
"current": true
```

Otherwise:

```json
"current": false
```

Example:

```text
Session A → current: true
Session B → current: false
Session C → current: false
```

This allows the frontend to display something like:

```text
Chrome / Windows    Current
Chrome / Android
Safari / iPhone
```

---

# 6. Never return the refresh token

Our session contains:

```js
refreshToken
```

but this field must **not** be returned to the frontend.

Even though we store a bcrypt hash rather than the raw refresh token, there is no reason to expose that authentication-related value.

We can exclude it with:

```js
.select("-refreshToken")
```

An even safer approach is to explicitly select only the fields the API needs.

For example:

```js
.select("_id ip userAgent createdAt expiresAt")
```

This is often preferable for security-sensitive responses because newly added sensitive fields won't accidentally become part of the API response.

---

# 7. Controller

A clean implementation is:

```js
const getSessions = async (req, res, next) => {
  try {
    // Get active sessions belonging to the authenticated user.
    const sessions = await sessionModel
      .find({
        user: req.user.sub,
        revoked: false,
      })
      .select("_id ip userAgent createdAt expiresAt")
      .sort({ createdAt: -1 });

    // Add information telling the frontend
    // which session is currently being used.
    const formattedSessions = sessions.map((session) => ({
      id: session._id,
      ip: session.ip,
      userAgent: session.userAgent,
      createdAt: session.createdAt,
      expiresAt: session.expiresAt,

      // req.user.sid came from the verified access token.
      current: session._id.toString() === req.user.sid,
    }));

    return res.status(200).json({
      message: "Active sessions fetched successfully",
      sessions: formattedSessions,
    });
  } catch (error) {
    next(error);
  }
};
```

---

# 8. Why do we use `.sort({ createdAt: -1 })`?

```js
.sort({ createdAt: -1 })
```

means:

> Show the newest sessions first.

For example:

```text
Session C → created recently
Session B → created earlier
Session A → created oldest
```

The API returns:

```text
C
B
A
```

This is useful for a session-management page because recent logins appear first.

---

# 9. Why use `map()` after the database query?

MongoDB gives us the complete Mongoose documents.

But the API should return only the information the frontend actually needs.

So:

```js
sessions.map(...)
```

converts database documents into an API-friendly response.

Database:

```text
Session document
```

↓

API response:

```json
{
  "id": "...",
  "ip": "...",
  "userAgent": "...",
  "createdAt": "...",
  "expiresAt": "...",
  "current": true
}
```

This separation is useful because your database structure doesn't have to exactly match your public API structure.

---

# 10. Route

Import the controller:

```js
import {
  register,
  login,
  getMe,
  refreshToken,
  logout,
  logoutAll,
  getSessions,
} from "../controllers/auth.controller.js";
```

Then:

```js
router.get("/sessions", authenticate, getSessions);
```

So our authentication routes now conceptually look like:

```text
POST /api/auth/register
POST /api/auth/login
POST /api/auth/refresh-token

GET  /api/auth/me

GET  /api/auth/sessions

POST /api/auth/logout
POST /api/auth/logout-all
```

---

# 11. Complete request flow

```text
GET /api/auth/sessions
        │
        ▼
Authorization: Bearer <accessToken>
        │
        ▼
authenticate middleware
        │
        ▼
jwt.verify()
        │
        ▼
req.user = { sub, sid }
        │
        ▼
Find sessions where:
user = req.user.sub
revoked = false
        │
        ▼
Select safe fields
        │
        ▼
Sort newest first
        │
        ▼
Mark current session
using req.user.sid
        │
        ▼
Return sessions
```

---

# 12. Example database state

Suppose the user has:

```text
Session A
user: user123
revoked: false
userAgent: Chrome / Windows

Session B
user: user123
revoked: false
userAgent: Chrome / Android

Session C
user: user123
revoked: true
userAgent: Firefox / Mac

Session D
user: user456
revoked: false
```

If `user123` requests:

```http
GET /api/auth/sessions
```

the result contains only:

```text
Session A
Session B
```

If the access token belongs to Session B:

```json
{
  "sessions": [
    {
      "id": "A",
      "userAgent": "Chrome / Windows",
      "current": false
    },
    {
      "id": "B",
      "userAgent": "Chrome / Android",
      "current": true
    }
  ]
}
```

---

# 13. Testing in Postman

## Step 1 — Create multiple sessions

Log in from multiple sessions.

For testing, you can create:

```text
Login 1 → Session A
Login 2 → Session B
Login 3 → Session C
```

Make sure they belong to the same user.

---

## Step 2 — Request sessions

```http
GET /api/auth/sessions
```

Header:

```http
Authorization: Bearer <accessToken>
```

---

## Step 3 — Verify the response

You should see multiple sessions:

```text
Session A
Session B
Session C
```

Only one should have:

```json
"current": true
```

when using the access token belonging to that session.

---

## Step 4 — Test after logout

Logout from Session B:

```http
POST /api/auth/logout
```

Session B becomes:

```text
revoked: true
```

Request sessions again:

```http
GET /api/auth/sessions
```

Session B should no longer appear because our query uses:

```js
revoked: false
```

---

# 14. Important distinction: "sessions" vs "devices"

Technically, the database stores **sessions**, not physical devices.

For example:

```text
Chrome on Windows
```

is information from the `userAgent`.

So our endpoint is more accurately:

```text
GET /api/auth/sessions
```

The UI may present these sessions to the user as "devices" or "logged-in devices".

A session is the actual security concept.

---

# 15. What this feature enables later

Once we can list sessions, we can build session management:

```text
GET /api/auth/sessions
        ↓
View logged-in sessions
        ↓
Choose a session
        ↓
Revoke that specific session
```

For example:

```http
DELETE /api/auth/sessions/:sessionId
```

could later mean:

> Log out this particular device/session.

Then our authentication features become:

```text
View sessions
     │
     ├── Logout current session
     │
     ├── Logout specific session
     │
     └── Logout all sessions
```

---

# 16. Practical security rules

For this endpoint:

- Require authentication.
    
- Use `req.user.sub` to determine the user.
    
- Never trust a client-provided user ID.
    
- Never return the refresh token or its hash.
    
- Return only the session information needed by the frontend.
    
- Use `req.user.sid` to identify the current session.
    
- Treat `Session` as the server-side source of truth for login sessions.
    
- Consider pagination if a user can accumulate many sessions over time.
    

---

# 17. Mental Model

```text
Session collection
       ↓
stores every login
       ↓
GET /sessions
       ↓
authenticated user's ID
       ↓
find that user's non-revoked sessions
       ↓
remove sensitive fields
       ↓
identify current session using sid
       ↓
return session list
```

### Remember

```text
sub → Which user?
sid → Which current session?
revoked → Is the session revoked?
```

And the endpoint:

```text
GET /api/auth/sessions
```

means:

> **"Show me the active sessions belonging to the authenticated user."**

It does **not** mean:

> "Show me all sessions in the database."

The authenticated user's identity determines which sessions can be returned.