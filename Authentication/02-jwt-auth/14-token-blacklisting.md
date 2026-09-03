
---

# Access Token Revocation, Stateful vs Stateless Auth & Token Blacklisting

## 1. First Understand: Stateful vs Stateless Authentication

Before discussing token revocation, understand **where authentication state lives**.

### Stateful Authentication

In stateful authentication, the server maintains authentication/session state.

```text
Client
  ↓
Session ID
  ↓
Server
  ↓
Session Store
  ↓
Is session valid?
  ↓
Allow / Reject
```

Example:

```text
Cookie
  ↓
sessionId = ABC123
  ↓
MongoDB / Redis
  ↓
Session exists and is active
```

The server knows whether the session is currently valid.

If the user logs out:

```text
Logout
  ↓
Revoke session
  ↓
Future request
  ↓
Session is invalid
  ↓
401
```

**Main advantage:** immediate server-side revocation.

**Main cost:** authentication depends on server-side state/lookups.

---

## 2. Stateless Authentication

In stateless authentication, the server does not need to maintain authentication state for each request.

A common example is a signed JWT access token.

```text
Client
  ↓
Access Token
  ↓
API Server
  ↓
Verify JWT
  ↓
Allow / Reject
```

The server can validate:

- Signature
    
- Expiration (`exp`)
    
- Claims such as `sub`
    

without querying the session database.

### Advantage

```text
No session lookup
      ↓
Simple validation
      ↓
Easy horizontal scaling
```

### Main limitation

Once a valid JWT is issued, it normally remains valid until its expiration.

```text
JWT
 ↓
exp = 15 minutes
 ↓
Valid until expiration
```

So **immediate revocation is not naturally provided by stateless JWT authentication**.

---

# 3. Our Authentication Architecture

Our application uses a **hybrid architecture**.

```text
                    Authentication
                           │
             ┌─────────────┴─────────────┐
             ↓                           ↓
       Access Token                Refresh Token
          JWT                           JWT
       short-lived                 long-lived
             │                           │
             │                      Session DB
             │                           │
             ↓                           ↓
      Mostly stateless            Server-controlled
```

Our setup:

```text
Access Token  → JWT, 15 minutes
Refresh Token → JWT, 15 days
Session       → MongoDB
```

The access token is **mostly stateless** because normal authentication only verifies the JWT.

The refresh-token flow is **stateful** because the server checks the session in MongoDB.

---

# 4. Now Understand the Logout Problem

Suppose the user logs in.

```text
Access Token
     ↓
valid for 15 minutes

Refresh Token
     ↓
valid according to session/refresh policy
```

The user then logs out.

Our logout operation does:

```text
session.revoked = true
        +
clear refresh-token cookie
```

This successfully prevents the refresh token from being used again.

But what about the existing access token?

```text
Existing Access Token
        ↓
jwt.verify()
        ↓
Signature valid?
        ↓
exp not reached?
        ↓
YES
        ↓
Access allowed
```

The JWT itself does not know that the MongoDB session was revoked.

Therefore:

```text
Logout
  ↓
Session revoked
  ↓
Refresh token ❌
Existing access token ✅
  ↓
until its 15-minute expiration
```

This is the important limitation of our stateless access-token validation.

> **Revoking a refresh session does not automatically revoke an already-issued access JWT.**

---

# 5. Is This Actually a Bug?

Not necessarily.

A short-lived access token is often intentionally designed this way.

For example:

```text
Access Token → 15 minutes
```

If the user logs out:

```text
Worst-case access-token window
≈ remaining lifetime of that token
```

The token may therefore remain usable for a short period.

The architecture accepts this trade-off in exchange for:

- No database lookup on every access-token request
    
- Simple JWT validation
    
- Better scalability
    

If the application requires **immediate access-token revocation**, then additional server-side state is required.

---

# 6. How Can We Solve Immediate Revocation?

There are two common approaches.

### Option A — Check Session State

When validating the access token:

```text
Request
  ↓
Verify JWT
  ↓
Read sid
  ↓
Check session
  ↓
revoked?
 ├── Yes → 401
 └── No  → Allow
```

Because our JWT already contains:

```js
{
  sub: userId,
  sid: sessionId
}
```

the server can use `sid` to find the session.

### Result

When logout sets:

```js
session.revoked = true;
```

future access-token requests can immediately fail.

### Trade-off

The authentication middleware now requires server-side state.

Therefore, the access-token validation is no longer purely stateless.

---

# 7. Option B — Access Token Blacklisting

Instead of checking the entire session on every request, we can maintain a **revocation list** for access tokens.

Conceptually:

```text
Request
  ↓
Verify JWT
  ↓
Check token revocation list
  ↓
Revoked?
 ├── Yes → 401
 └── No  → Allow
```

This is called **token blacklisting** or a **deny-list**.

---

# 8. How Blacklisting Works

Give each access token a unique `jti`:

```js
{
  sub: userId,
  sid: sessionId,
  jti: uniqueTokenId
}
```

Meaning:

```text
sub → who is logged in
sid → which session
jti → which exact JWT
```

When the token is revoked:

```text
Blacklist
    ↓
jti
expiresAt
```

Then authentication checks whether that `jti` is revoked.

---

# 9. Why `jti` Instead of `sid`?

This distinction is important.

`sid` identifies the **session**:

```text
Session A
```

`jti` identifies a **specific JWT**:

```text
Access Token #123
```

One session can generate many access tokens:

```text
Session A
 ├── Access Token 1
 ├── Access Token 2
 └── Access Token 3
```

Therefore:

```text
sid → session-level identification
jti → individual-token identification
```

If your requirement is to revoke the entire session, checking `sid`/session state is often more natural.

If you specifically need to revoke an individual JWT, `jti` is appropriate.

---

# 10. Why Use Token Blacklisting?

Use a blacklist when **immediate revocation of individual access tokens is a real requirement**.

### Benefits

- Immediately rejects a still-unexpired JWT
    
- Useful for security incidents
    
- Can revoke a specific token
    
- Keeps JWT-based access tokens
    

For example:

```text
Suspicious token detected
        ↓
Add jti to blacklist
        ↓
Token immediately rejected
```

---

# 11. Why Not Use Token Blacklisting Everywhere?

Because it introduces server-side state into your supposedly stateless access-token system.

Without blacklist:

```text
Request
  ↓
Verify JWT
  ↓
Allow
```

With blacklist:

```text
Request
  ↓
Verify JWT
  ↓
Check blacklist
  ↓
Allow
```

### Costs

- Additional storage
    
- Additional lookup
    
- Expired blacklist entries need cleanup
    
- Distributed servers need shared revocation state
    
- More implementation and operational complexity
    

Therefore, blacklisting should be used because there is a **specific revocation requirement**, not simply because JWT logout is not instantaneous.

---

# 12. Blacklist vs Session Check

Both provide server-side revocation, but they solve slightly different problems.

||Session Check|Token Blacklist|
|---|---|---|
|Checks|Session state|Token revocation|
|Typical identifier|`sid`|`jti`|
|Revokes|Session/device|Specific token|
|Immediate access revocation|Yes|Yes|
|Server-side lookup|Yes|Yes|
|Fits our existing Session model|Very well|Requires additional store|
|Complexity|Lower for our architecture|Higher|

For our application, **checking the existing session state is usually simpler if immediate revocation is required**, because we already have a session database.

---

# 13. What Should We Use in Our Project?

For the current application:

```text
Access Token
→ short-lived JWT
→ 15 minutes

Refresh Token
→ HttpOnly cookie
→ rotation

Session
→ MongoDB
→ revoked + expiresAt
```

Recommended default:

```text
Logout
  ↓
Revoke session
  ↓
Clear refresh cookie
  ↓
Existing access token
  ↓
Allowed until short expiration
```

This is a reasonable architecture when a short revocation window is acceptable.

### If immediate revocation becomes mandatory:

Prefer evaluating:

```text
JWT verification
      +
Session-state check
```

because our application already maintains session records.

Use a dedicated token blacklist when there is a concrete need for **individual-token revocation** or a separate revocation-list design.

---

# 14. Final Mental Model

```text
STATEFUL
   ↓
Server remembers authentication state
   ↓
Easy immediate revocation


STATELESS JWT
   ↓
Server verifies token itself
   ↓
No server lookup normally
   ↓
Harder immediate revocation


OUR SYSTEM
   ↓
Hybrid
   ├── Access JWT → mostly stateless
   └── Refresh + Session → stateful


LOGOUT
   ↓
Revoke session
   ↓
Refresh token stops working


EXISTING ACCESS JWT
   ↓
Still valid until `exp`
   ↓
unless access authentication checks
server-side revocation state


IMMEDIATE ACCESS REVOCATION
   ↓
Either:
   ├── Check session state
   └── Blacklist access-token `jti`
```

### One sentence to remember

> **JWT expiration controls how long a token naturally lives; session revocation controls whether the server allows the session to continue; immediate access-token revocation requires the server to check some revocation state.**