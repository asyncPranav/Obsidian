
---

# Session Expiration — Fixed vs Sliding

## 1. What Is Session Expiration?

A **session** represents one active login of a user.

In our application, the session is stored in MongoDB:

```js
{
  _id: sessionId,
  user: userId,
  refreshToken: hashedRefreshToken,
  revoked: false,
  expiresAt: ...
}
```

`expiresAt` tells the server **until when this session is allowed to remain active**.

During refresh, we check it:

```js
if (session.expiresAt < new Date()) {
  throw new ApiError(401, "Session has expired");
}
```

---

# 2. Fixed Expiration

**Fixed expiration** means the session gets an expiration time when it is created, and refreshing the token does **not** move that expiration forward.

Example:

```text
Login
  ↓
Session created
  ↓
Expires in 15 days
  ↓
Refresh
  ↓
Same expiration date
```

Example:

```text
Login:       September 1
Expires:     September 16

Refresh:     September 10
Expires:     September 16  ← unchanged
```

### Simple definition

> **Fixed expiration = the expiration clock starts at login and does not restart.**

### When is it useful?

Fixed/absolute expiration is useful when you want to put a **hard limit** on how long a login can remain active.

Good for:

- Banking/financial applications
    
- Admin panels
    
- Sensitive business systems
    
- High-security applications
    
- Situations where a stolen session must eventually become useless even if it is continuously used
    

---

# 3. Sliding Expiration

**Sliding expiration** means a successful refresh moves the session expiration forward.

Our current code does:

```js
session.expiresAt = new Date(
  Date.now() + 15 * 24 * 60 * 60 * 1000,
);
```

So every successful refresh gives the session another 15 days.

Example:

```text
Login:       September 1
Expires:     September 16

Refresh:     September 10
Expires:     September 25

Refresh:     September 20
Expires:     October 5
```

### Simple definition

> **Sliding expiration = the expiration clock moves forward after successful activity/refresh.**

### When is it useful?

Sliding expiration is useful when you want a better user experience and don't want active users to be logged out simply because an arbitrary calendar deadline was reached.

Good for:

- Normal web applications
    
- Social/media applications
    
- E-commerce
    
- Productivity applications
    
- Applications where users expect to stay logged in
    

---

# 4. Which One Is Better?

There is **no universal winner**.

It depends on the application's security requirements.

|Policy|Security|User experience|Best for|
|---|---|---|---|
|Fixed|Higher|Less convenient|Sensitive applications|
|Sliding|More convenient|Better|Normal consumer applications|

But in real systems, a common practical approach is:

> **Use a sliding/inactivity timeout together with an absolute maximum lifetime.**

This gives you both convenience and a security limit. OWASP recommends considering both idle and absolute session timeouts, with the actual values based on the application's risk.

---

# 5. Practical Industry Approach

Instead of thinking only:

```text
Fixed OR Sliding
```

think:

```text
Idle/Sliding Timeout
        +
Absolute Maximum Lifetime
```

### Idle / Sliding Timeout

Controls how long the session can remain active without sufficient activity.

For example:

```text
No refresh/activity for 15 days
        ↓
Session expires
```

With sliding behavior, successful refresh moves this deadline forward.

### Absolute Lifetime

Sets a maximum lifetime from the original login.

For example:

```text
Login
 ↓
Maximum lifetime = 90 days
 ↓
Even if the user keeps refreshing
 ↓
Session cannot live beyond 90 days
```

This prevents a continuously refreshed session from living forever.

OWASP specifically recommends an idle timeout and an absolute timeout because they address different risks.

---

# 6. Why Use Both?

Imagine a stolen refresh token.

If we only use sliding expiration:

```text
Attacker gets refresh token
        ↓
Keeps refreshing
        ↓
Session expiration keeps moving
        ↓
Session could potentially remain active indefinitely
```

An absolute lifetime puts a maximum limit on that.

For example:

```text
Idle/sliding timeout = 15 days
Absolute lifetime    = 90 days
```

Then:

```text
Day 1
Login
 ↓
Day 10
Refresh → +15 days
 ↓
Day 20
Refresh → +15 days
 ↓
Day 30
Refresh → +15 days
 ↓
...
 ↓
Day 90
Absolute lifetime reached
 ↓
Re-authentication required
```

So sliding expiration provides **convenience**, while absolute expiration provides a **hard security boundary**.

---

# 7. What Should We Use in Our Current Project?

For our current learning project:

```text
Refresh Token → 15 days
Session        → 15-day sliding expiration
```

This is easy to understand and reasonable for a normal application.

Our current code:

```js
session.expiresAt = new Date(
  Date.now() + 15 * 24 * 60 * 60 * 1000,
);
```

means:

> "After every successful refresh, allow this session for another 15 days."

Therefore, our current implementation uses:

**15-day sliding session expiration.**

---

# 8. One Important Security Point About Refresh Tokens

For production systems, don't assume that simply extending the expiration on every refresh is always the best policy.

Modern OAuth security guidance recommends that refresh tokens for public clients either use **refresh-token rotation** or sender-constrained tokens, and recommends limiting refresh-token lifetime so a stolen token cannot be used indefinitely.

Our rotation flow is therefore:

```text
Old Refresh Token
       ↓
Verify
       ↓
Validate Session
       ↓
Compare Hash
       ↓
Generate New Refresh Token
       ↓
Hash New Token
       ↓
Replace DB Hash
       ↓
Set New Cookie
```

And our current session policy is:

```text
Successful refresh
       ↓
Move expiresAt forward 15 days
```

---

# 9. Final Mental Model

Remember these three terms:

### Fixed / Absolute

```text
"Expire at this maximum time."
```

The deadline does not move.

### Sliding / Idle

```text
"Keep me active while I continue using the session."
```

The deadline moves forward after successful activity.

### Practical production approach

```text
Sliding/Idle Timeout
        +
Absolute Maximum Lifetime
        +
Refresh Token Rotation
```

This provides a good balance between **security and user experience**.

## Quick Revision

```text
FIXED
Login → 15 days → Expire
Refresh does NOT extend it.

SLIDING
Login → 15 days
         ↓
      Refresh
         ↓
      +15 days
         ↓
      Refresh
         ↓
      +15 days

PRACTICAL PRODUCTION
Sliding/Idle timeout
        +
Absolute maximum lifetime
        +
Refresh-token rotation
```

**For our project:** keep the **15-day sliding session expiration** for now. Later, when you learn more advanced session security, add an **absolute maximum lifetime** if the application's risk level requires it.