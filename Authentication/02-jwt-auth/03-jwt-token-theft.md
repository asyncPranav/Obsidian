
---

# JWT Authentication & Token Storage — Complete Notes

## 1. The Big Picture

A modern authentication system often uses two credentials:

```text
Access Token
    ↓
Short-lived
    ↓
Used to access protected APIs


Refresh Token
    ↓
Longer-lived
    ↓
Used to obtain a new access token
```

A common browser architecture is:

```text
                    LOGIN
                      │
                      ▼
                   Server
                      │
             ┌────────┴────────┐
             ▼                 ▼
       Access Token       Refresh Token
             │                 │
             ▼                 ▼
          Memory          HttpOnly Cookie
             │                 │
             ▼                 │
        API requests             │
                                 │
                                 ▼
                             /refresh
                                 │
                                 ▼
                         New Access Token
                                 │
                                 ▼
                              Memory
```

This is likely the architecture your tutor is teaching.

---

# 2. What Is a JWT?

JWT stands for:

> **JSON Web Token**

A JWT is a compact format for carrying claims between parties.

A typical JWT looks like:

```text
xxxxx.yyyyy.zzzzz
```

It has three parts:

```text
Header.Payload.Signature
```

### Header

Contains information such as the signing algorithm:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### Payload

Contains claims:

```json
{
  "sub": "USER_A_ID",
  "iat": 1750000000,
  "exp": 1750000900
}
```

### Signature

Used to verify that the token was created by a trusted signer and hasn't been modified.

---

# 3. What Does JWT Actually Give Us?

A signed JWT can provide:

```text
Integrity
+
Authenticity of the issuer/signature
+
Claims such as user ID and expiration
```

It does **not automatically provide**:

```text
Confidentiality
+
Protection against token theft
+
Proof of the physical identity of the person using it
```

This distinction is extremely important.

---

# 4. The Bearer Token Concept

Access tokens are commonly used as **bearer tokens**.

Bearer essentially means:

> **Whoever possesses the valid token can present it as a credential.**

Think of it like a key:

```text
House key
    ↓
Whoever possesses the key
    ↓
Can potentially open the door
```

The lock doesn't know whether the person holding the key is the actual owner.

Similarly:

```text
Valid access token
       ↓
Server accepts credential
```

The server normally doesn't know the physical person holding that credential.

---

# 5. User A and User B Example

Suppose User A logs in.

The server creates:

```text
ACCESS_TOKEN_A
```

The JWT contains:

```json
{
  "sub": "USER_A_ID"
}
```

User A sends:

```http
Authorization: Bearer ACCESS_TOKEN_A
```

The server verifies it:

```js
const decoded = jwt.verify(token, config.jwtSecret);
```

Then:

```js
decoded.sub
```

is:

```text
USER_A_ID
```

So the server knows:

```text
This credential represents User A.
```

---

## Now User B steals the token

User B obtains:

```text
ACCESS_TOKEN_A
```

and sends:

```http
Authorization: Bearer ACCESS_TOKEN_A
```

The server sees:

```text
Valid token
+
sub = USER_A_ID
```

Therefore:

```text
User B
   ↓
Stolen Access Token A
   ↓
Server
   ↓
Sees User A
```

This is **token theft/replay**.

JWT itself cannot normally determine:

```text
"This is actually User B."
```

because User B possesses User A's valid bearer credential.

---

# 6. What Does `jwt.verify()` Actually Do?

When you write:

```js
const decoded = jwt.verify(token, config.jwtSecret);
```

you're asking the JWT library to verify the token according to your configuration.

Typically this includes things such as:

```text
Signature
Expiration
Not-before
Issuer/audience, if configured
```

It does **not** prove:

```text
The physical person is User A.
```

It proves something closer to:

```text
This is a valid credential representing User A.
```

So:

```text
jwt.verify()
       ↓
Valid credential
       ↓
NOT
       ↓
Proof of physical identity
```

---

# 7. Authentication vs Authorization

These are different concepts.

## Authentication

Answers:

> **Who does this credential represent?**

Example:

```text
JWT
 ↓
sub = USER_A
 ↓
Authenticated as User A
```

## Authorization

Answers:

> **What is this authenticated user allowed to do?**

Example:

```text
User A
 ↓
GET /me
 ↓
Allowed
```

But:

```text
User A
 ↓
DELETE /admin/users/B
 ↓
Not allowed
```

So:

```text
Authentication
    ↓
Who are you?

Authorization
    ↓
What are you allowed to do?
```

JWT mainly participates in authentication. You still need authorization rules.

---

# 8. Never Trust a Client-Provided User ID

For `/me`, don't do:

```js
const userId = req.body.userId;
```

or:

```js
const userId = req.query.userId;
```

to determine the authenticated user.

Instead:

```js
const userId = req.user.sub;
```

where `req.user` came from a verified credential.

The flow should be:

```text
Credential
    ↓
Verify
    ↓
Determine identity
    ↓
req.user
    ↓
Controller
```

---

# 9. Use Authentication Middleware

Instead of verifying JWT inside every controller, create middleware.

```js
const authenticate = (req, res, next) => {
  try {
    const authHeader = req.headers.authorization;

    if (!authHeader?.startsWith("Bearer ")) {
      throw new ApiError(401, "Authentication required");
    }

    const token = authHeader.split(" ")[1];

    const decoded = jwt.verify(token, config.jwtSecret);

    req.user = decoded;

    next();
  } catch (error) {
    next(error);
  }
};
```

Then:

```js
router.get("/me", authenticate, getMe);
```

The architecture becomes:

```text
Request
   ↓
authenticate middleware
   ↓
Verify credential
   ↓
req.user
   ↓
Controller
```

---

# 10. Important: Middleware Doesn't Stop Token Theft

Suppose User B has User A's token.

The middleware receives:

```text
TOKEN_A
```

and verifies it successfully.

Therefore:

```text
TOKEN_A
   ↓
Valid
   ↓
req.user = User A
```

The middleware still doesn't know that User B is holding it.

So:

> **Authentication middleware validates the credential; it does not identify the physical human using it.**

---

# 11. Token Storage

For browser applications, there are several ways to store tokens.

The major ones you should understand are:

```text
1. Memory
2. localStorage
3. Cookies
```

Each has different security and usability characteristics.

---

# 12. Storage Option 1 — Memory

This is the approach your tutor is using.

The access token is stored only in the application's memory.

For example:

```js
let accessToken = null;
```

Or using frontend state:

```text
React state
Vue state
Angular service
etc.
```

The important idea is:

```text
Access Token
      ↓
Application memory
```

It is **not persisted in browser storage**.

---

# 13. What Happens When the Page Reloads?

Suppose:

```text
Access Token
     ↓
Memory
```

The user refreshes the page.

The JavaScript application starts again.

The old memory disappears:

```text
Page reload
    ↓
Old JavaScript memory destroyed
    ↓
Access token gone
```

So the frontend needs a way to get a new access token.

That's where the refresh token comes in.

```text
Page reload
    ↓
Access token missing
    ↓
POST /refresh
    ↓
Refresh token cookie
    ↓
New access token
    ↓
Store in memory
```

This is a major advantage of the memory + refresh-token architecture.

---

# 14. Advantages of Memory Storage

### Advantage 1 — Not persistent

When the page/application context is destroyed:

```text
Access token disappears
```

### Advantage 2 — Not sitting in `localStorage`

You aren't intentionally keeping the access token in persistent browser storage.

### Advantage 3 — Good fit for short-lived access tokens

You can use:

```text
Access token → short lifetime
Refresh token → longer lifetime
```

### Advantage 4 — Reduces persistent exposure

An access token isn't sitting in a browser storage mechanism waiting to be read later.

---

# 15. Disadvantages of Memory Storage

The main downside:

```text
Page reload
    ↓
Access token disappears
```

So you need a refresh mechanism.

That means:

```text
Access token
     +
Refresh token
```

are normally used together.

Also, **memory is not magically immune to XSS**.

If malicious JavaScript is executing inside your application, it may potentially interact with data available to that application while it is running.

So the goal is not:

> "Memory makes XSS impossible."

It doesn't.

The goal is:

> **Don't persist the access token in a browser storage mechanism when you don't need to.**

---

# 16. Storage Option 2 — `localStorage`

You could store a token like:

```js
localStorage.setItem("accessToken", token);
```

and retrieve it:

```js
const token = localStorage.getItem("accessToken");
```

The problem is:

> JavaScript can directly access `localStorage`.

So if your application has an XSS vulnerability:

```text
XSS
 ↓
Malicious JavaScript
 ↓
localStorage.getItem()
 ↓
Access token potentially stolen
```

The attacker could then replay it:

```http
Authorization: Bearer STOLEN_TOKEN
```

---

# 17. Why `localStorage` Is Often Discouraged for Auth Tokens

The issue isn't:

```text
localStorage = automatically insecure
```

The more precise statement is:

> **Authentication tokens stored in `localStorage` are directly accessible to JavaScript, so an XSS vulnerability can potentially expose them.**

This is why many security-conscious browser architectures avoid storing long-lived authentication credentials there.

---

# 18. Does Memory Completely Solve the XSS Problem?

No.

Compare:

### `localStorage`

```text
Token
 ↓
Persistent browser storage
 ↓
JavaScript can read it
```

### Memory

```text
Token
 ↓
Application memory
 ↓
Not persistent
```

But if malicious JavaScript executes while your application is running, memory-resident secrets may still be exposed depending on how the application handles them.

So:

```text
Memory
≠
XSS protection
```

You still need proper XSS prevention.

---

# 19. Storage Option 3 — Cookies

A cookie is another way to store authentication credentials.

Example:

```http
Set-Cookie: refreshToken=...; HttpOnly; Secure; SameSite=Lax
```

Cookies have an important difference:

> The browser can automatically send them with matching requests.

This makes cookies convenient for authentication, but it also creates CSRF considerations.

---

# 20. HttpOnly Cookies

The important cookie attribute is:

```text
HttpOnly
```

An HttpOnly cookie cannot be directly read by JavaScript using:

```js
document.cookie
```

So:

```text
HttpOnly cookie
       ↓
JavaScript cannot directly read value
```

This helps reduce the risk of token extraction through certain XSS attacks.

---

# 21. Secure Cookies

Use:

```text
Secure
```

in production.

It means the cookie should only be transmitted over HTTPS.

Conceptually:

```text
HTTPS
   ↓
Secure cookie
```

This helps prevent the credential from being sent over plain HTTP.

---

# 22. SameSite Cookies

Another important attribute is:

```text
SameSite
```

Common values:

```text
Strict
Lax
None
```

It controls cookie sending in cross-site contexts.

It can help reduce CSRF risk.

For example:

```js
sameSite: "lax"
```

may be appropriate for many same-site applications.

But the correct setting depends on your architecture.

---

# 23. `SameSite=None`

If your architecture genuinely requires cross-site cookie sending:

```js
sameSite: "none"
```

then:

```js
secure: true
```

is required by modern browsers.

You also need to carefully consider CSRF protection.

Don't choose `SameSite=None` just because it makes development easier.

---

# 24. XSS vs CSRF

These are different attacks.

## XSS

Cross-Site Scripting.

Attacker manages to execute JavaScript in your application's origin.

Conceptually:

```text
XSS
 ↓
Malicious JavaScript
 ↓
Potentially reads accessible secrets/data
```

`HttpOnly` helps prevent JavaScript from directly reading an HttpOnly cookie.

---

## CSRF

Cross-Site Request Forgery.

The attacker causes the victim's browser to send an authenticated request to your server.

Conceptually:

```text
Attacker website
      ↓
Victim's browser
      ↓
Authenticated request
      ↓
Your server
```

Cookies are automatically sent by browsers under their applicable rules, so cookie-based authentication requires CSRF considerations.

Therefore:

```text
HttpOnly
   ↓
Helps protect cookie value from JavaScript

SameSite / CSRF defenses
   ↓
Help protect against unwanted cross-site requests
```

They solve different problems.

---

# 25. The Architecture Your Tutor Is Teaching

Now put everything together.

A common design is:

```text
┌─────────────────────────────────────┐
│              Browser                │
│                                     │
│   Access Token → Memory             │
│                                     │
│   Refresh Token → HttpOnly Cookie   │
└─────────────────────────────────────┘
                 │
                 │ HTTPS
                 ▼
              Server
```

This is a very reasonable architecture.

---

# 26. Login Flow

User logs in:

```http
POST /login
```

Server verifies credentials.

Then server creates:

```text
Access Token
+
Refresh Token
```

The response might conceptually be:

```text
Access Token
    ↓
Frontend memory

Refresh Token
    ↓
HttpOnly cookie
```

The frontend cannot directly read the refresh token.

---

# 27. Using the Access Token

For protected API requests:

```http
GET /me
Authorization: Bearer ACCESS_TOKEN
```

Server:

```text
Request
   ↓
Authentication middleware
   ↓
Verify access token
   ↓
req.user
   ↓
Controller
```

---

# 28. Access Token Expires

Suppose:

```text
Access token = 15 minutes
```

After 15 minutes:

```text
Access Token
     ↓
Expired
```

The frontend can't continue using it.

It calls:

```http
POST /refresh
```

The browser automatically includes the refresh cookie if cookie rules allow it.

---

# 29. Refresh Flow

The server receives:

```text
Refresh Token
```

It validates it.

For a production-oriented design, the server may check:

```text
Is it valid?
Is it expired?
Has it been revoked?
Does the session exist?
Does it belong to the expected user/session?
```

If valid:

```text
Refresh Token
      ↓
New Access Token
```

The server returns the new access token.

The frontend stores it in memory again.

```text
New Access Token
      ↓
Memory
```

---

# 30. Complete Authentication Flow

```text
                     LOGIN
                       │
                       ▼
                    Server
                       │
              ┌────────┴────────┐
              ▼                 ▼
        Access Token       Refresh Token
              │                 │
              ▼                 ▼
           Memory         HttpOnly Cookie
              │
              ▼
       Protected APIs
              │
              ▼
       Access token expires
              │
              ▼
          /refresh
              │
              ▼
      Refresh token checked
              │
              ▼
       New access token
              │
              ▼
           Memory
```

---

# 31. Why Not Make the Access Token Last 30 Days?

Because if it is stolen:

```text
30-day access token
       ↓
Attacker may have access
       ↓
Potentially for a long time
```

Instead:

```text
Access token
   ↓
Short-lived
```

For example:

```text
15 minutes
```

Then:

```text
Refresh token
   ↓
Longer-lived
```

The long-lived credential is more strongly protected.

---

# 32. Why Not Make the Refresh Token the Authorization Header?

Because the refresh token has a different purpose.

Don't use:

```http
Authorization: Bearer REFRESH_TOKEN
```

for normal API requests.

Instead:

```text
Access token
    ↓
Normal protected API requests
```

and:

```text
Refresh token
    ↓
Only obtain new access tokens
```

Think:

```text
Access token = API credential
Refresh token = token renewal credential
```

---

# 33. Refresh Tokens Need a Lifecycle

Don't think of a refresh token as:

```text
Permanent password
```

It should have:

```text
Expiration
+
Revocation
+
Rotation where appropriate
+
Session association
```

A server-side session might look conceptually like:

```text
Session
-------------------------
sessionId
userId
refreshTokenHash
createdAt
expiresAt
revokedAt
```

This gives the server control over active sessions.

---

# 34. Refresh Token Rotation

A stronger approach is rotation.

Example:

```text
Refresh Token A
       ↓
/refresh
       ↓
A revoked
       ↓
Refresh Token B
       ↓
New Access Token
```

Then:

```text
A → B → C → D
```

If an old refresh token is reused:

```text
A
 ↓
Already used/revoked
 ↓
Suspicious
```

The server can respond according to its security policy, potentially revoking the associated session/token family.

---

# 35. Logout

When the user logs out:

```text
Frontend
   ↓
Delete access token from memory
```

and:

```text
Server
   ↓
Revoke refresh-token/session
   ↓
Clear refresh cookie
```

Conceptually:

```text
Logout
 ├── Access token → removed from memory
 └── Refresh session → revoked
```

---

# 36. Important Logout Limitation

Suppose an attacker already copied the access token.

User A logs out.

The browser deletes the token.

But the attacker still has:

```text
ACCESS_TOKEN_A
```

If your server only performs:

```js
jwt.verify(token, secret);
```

then that access token may remain valid until its expiration.

Therefore:

```text
Browser logout
≠
Instant invalidation of every copied stateless access token
```

Short access-token lifetimes help reduce this window.

If you require immediate invalidation, you need additional server-side revocation/introspection/session controls.

---

# 37. What Happens If the Refresh Token Is Stolen?

This can be more serious.

Suppose:

```text
Attacker steals Refresh Token A
```

They may be able to call:

```text
/refresh
```

and obtain new access tokens.

That's why refresh tokens need stronger protection and lifecycle management.

A common design is:

```text
Refresh token
   ↓
HttpOnly + Secure cookie
   +
Shorter appropriate lifetime
   +
Rotation
   +
Server-side session tracking
   +
Revocation
```

---

# 38. Memory vs `localStorage` vs Cookie

Here's the comparison you should remember:

|Feature|Memory|`localStorage`|HttpOnly Cookie|
|---|---|---|---|
|Persistent across reload|❌|✅|✅|
|JavaScript can directly read|Usually application-accessible|✅|❌|
|Automatically sent by browser|❌|❌|✅|
|Good for short-lived access token|✅|Possible, but more XSS exposure|Possible|
|Good for refresh token|❌|Generally undesirable|✅|
|XSS can directly read value|Application-dependent|✅|❌ for HttpOnly|
|CSRF concern|Usually low for header token|Usually low for header token|**Yes**|
|Requires refresh mechanism|Usually yes|Not necessarily|Depends on architecture|

The key is not:

```text
Memory = secure
localStorage = insecure
Cookie = secure
```

That's too simplistic.

The correct understanding is:

```text
Each storage mechanism has different security properties.
```

---

# 39. Recommended Learning Architecture

For the architecture your tutor is teaching, think:

```text
ACCESS TOKEN
     ↓
Memory
     ↓
Short-lived
     ↓
Authorization header


REFRESH TOKEN
     ↓
HttpOnly + Secure cookie
     ↓
Longer-lived
     ↓
/refresh
     ↓
New access token
```

This is a strong architecture to learn.

---

# 40. Why This Architecture Is Attractive

It combines several useful properties:

```text
Access token
    ↓
Short lifetime
    +
Memory-only storage
```

So the access token isn't intentionally persisted in browser storage.

And:

```text
Refresh token
    ↓
HttpOnly cookie
```

so JavaScript cannot directly read it.

Then:

```text
Refresh token
    ↓
Rotation + revocation
```

can give the server control over longer-lived authentication.

---

# 41. But Remember: No Storage Method Is Magic

Don't memorize:

> "Memory is secure."

Instead remember:

> **Memory avoids persistent browser storage, but it doesn't eliminate XSS or other attacks.**

Don't memorize:

> "`localStorage` is always insecure."

Instead:

> **`localStorage` exposes the token to JavaScript, which makes token theft through XSS more direct.**

Don't memorize:

> "Cookies are secure."

Instead:

> **HttpOnly cookies prevent JavaScript from directly reading the cookie, but cookie-based authentication requires proper HTTPS, SameSite, and CSRF considerations.**

That's the technically accurate way to think about it.

---

# 42. Should We Store the Access Token in a Cookie Instead?

That's also a valid architecture.

For example:

```text
Access Token
    ↓
HttpOnly cookie
```

Then the browser automatically sends it.

But now you have to carefully handle:

```text
CSRF
SameSite
Cookie configuration
```

So there isn't one universal answer.

Your tutor's:

```text
Access token → memory
Refresh token → HttpOnly cookie
```

is a very reasonable architecture for a browser application and is useful for learning modern authentication patterns.

---

# 43. Don't Put Sensitive Secrets in JWT Payload

Remember:

```json
{
  "sub": "USER_A",
  "email": "user@example.com"
}
```

is not the same as encrypted data.

A signed JWT is generally readable by whoever possesses it.

Therefore don't put things like:

```text
password
password hash
API secrets
private keys
credit-card secrets
```

into the JWT payload just because the JWT is signed.

---

# 44. Don't Try to Fix Token Theft With IP Address

You might think:

```text
Token belongs to IP A
        ↓
Request from IP B
        ↓
Reject
```

Don't use this as your primary authentication mechanism.

Users can legitimately change:

```text
IP address
Network
Wi-Fi
VPN
Mobile connection
Proxy
```

IP can be a useful **risk signal**, but it isn't reliable proof of identity.

---

# 45. Don't Rely on Device Fingerprinting as Identity

Similarly:

```text
device fingerprint
```

can help with:

```text
Suspicious-login detection
Risk scoring
Session management
```

but:

```text
Same device
≠
Same person
```

and:

```text
Different device
≠
Attacker
```

It should not be treated as a perfect authentication mechanism.

---

# 46. Advanced Protection

For high-security systems, there are sender-constrained credential mechanisms such as:

```text
DPoP
mTLS
```

The basic idea is:

### Normal bearer token

```text
Possess token
    ↓
Use token
```

### Sender-constrained credential

```text
Token
 +
Proof from associated key
        ↓
Server accepts
```

This can make simple token replay more difficult.

But you don't need this for your basic JWT project.

---

# 47. What You Should Build First

Don't implement everything at once.

### Step 1

Learn:

```text
/register
/login
```

### Step 2

Create:

```text
Access JWT
```

### Step 3

Create:

```text
Authentication middleware
```

### Step 4

Build:

```text
GET /me
```

using:

```js
req.user.sub
```

### Step 5

Store access token:

```text
Memory
```

### Step 6

Add:

```text
Refresh token
```

### Step 7

Store refresh token:

```text
HttpOnly + Secure cookie
```

### Step 8

Create:

```text
POST /refresh
```

### Step 9

Implement:

```text
Refresh-token rotation
```

### Step 10

Add:

```text
Session storage
+
Revocation
+
Logout
```

Then learn more advanced security concepts.

---

# 48. Complete Mental Model

Keep this diagram in your head:

```text
                         LOGIN
                           │
                           ▼
                        SERVER
                           │
                ┌──────────┴──────────┐
                │                     │
                ▼                     ▼
          ACCESS TOKEN          REFRESH TOKEN
          Short-lived            Long-lived
                │                     │
                ▼                     ▼
             MEMORY            HttpOnly Cookie
                │                     │
                │                     │
                ▼                     ▼
        Authorization: Bearer     /refresh
                │                     │
                ▼                     ▼
          Protected API          New Access Token
                                      │
                                      ▼
                                   MEMORY
```

And security around it:

```text
                    AUTHENTICATION
                          │
          ┌───────────────┼────────────────┐
          ▼               ▼                ▼
       JWT           Access Token      Refresh Token
     validation       protection         protection
          │               │                │
          │          short lifetime    HttpOnly
          │          memory            Secure
          │                           SameSite
          │                           rotation
          │                           revocation
          │
          ▼
      req.user
          │
          ▼
    AUTHORIZATION
          │
          ▼
   What can user do?
```

---

# 49. Final Cheat Sheet

### Access Token

```text
Short-lived
Used for API authorization
Usually sent as Bearer token
Can be stored in memory
```

Example:

```http
Authorization: Bearer ACCESS_TOKEN
```

---

### Refresh Token

```text
Longer-lived
Used to obtain new access tokens
Should be strongly protected
Often stored in HttpOnly cookie
Should have expiration/revocation
Prefer rotation in serious applications
```

---

### Memory

```text
Not persistent
Token disappears on page reload
Good fit for short-lived access tokens
Still requires XSS protection
```

---

### `localStorage`

```text
Persistent
JavaScript can read it
XSS can potentially steal stored tokens
Often avoided for sensitive authentication credentials
```

---

### HttpOnly Cookie

```text
JavaScript cannot directly read it
Browser automatically sends it
Good option for refresh tokens
CSRF must be considered
```

---

### Secure Cookie

```text
Sent only over HTTPS
```

---

### SameSite

```text
Controls cross-site cookie behavior
Helps reduce CSRF risk
```

---

### XSS

```text
Attacker executes JavaScript in your origin
```

Protect with things such as:

```text
Input/output handling
CSP
Framework protections
Avoiding unsafe HTML
Careful third-party scripts
```

---

### CSRF

```text
Attacker causes victim's browser
to make an authenticated request
```

Important defenses include:

```text
SameSite cookies
CSRF tokens where appropriate
Origin/Referer validation where appropriate
```

---

### JWT theft

```text
Attacker obtains valid token
        ↓
Attacker can potentially replay it
        ↓
Server sees token's identity
        ↓
Server may treat attacker as that user
```

JWT itself cannot normally distinguish:

```text
User A using Token A
```

from:

```text
User B using stolen Token A
```

---

# 50. The Most Important Things to Remember

If you remember only these **10 points**, remember these:

```text
1. JWT is a credential, not a physical identity detector.

2. A bearer token can be used by whoever possesses it.

3. jwt.verify() validates the token; it doesn't identify the human holding it.

4. Authentication asks "Who are you?"
   Authorization asks "What can you do?"

5. Access tokens should generally be short-lived.

6. Memory storage avoids persistent browser storage but does not magically prevent XSS.

7. localStorage is accessible to JavaScript, so XSS can potentially steal tokens stored there.

8. HttpOnly cookies cannot be directly read by JavaScript, but cookie authentication requires CSRF considerations.

9. Refresh tokens should be strongly protected and preferably have rotation, expiration, and revocation/session controls.

10. Good authentication security is defense in depth:
    HTTPS + secure storage + XSS protection
    + CSRF protection where applicable
    + short-lived access tokens
    + refresh-token lifecycle
    + authorization
    + session/revocation controls.
```

## The final architecture to remember

```text
                BROWSER
                   │
       ┌───────────┴───────────┐
       │                       │
       ▼                       ▼
 ACCESS TOKEN             REFRESH TOKEN
       │                       │
       ▼                       ▼
    MEMORY              HttpOnly Cookie
       │                       │
       │                       │
       ▼                       ▼
 Protected APIs             /refresh
       │                       │
       │                       ▼
       │                New Access Token
       │                       │
       └───────────────────────┘
                               │
                               ▼
                            MEMORY
```

And the fundamental rule behind everything:

> **The access token says, "I am presenting a valid credential for User A." It does not say, "I can prove that the physical person holding this credential is User A."**

That's why your tutor's **memory + refresh-token** architecture and the concepts of **localStorage and cookies** all fit together: the goal isn't to make token theft mathematically impossible; it's to **reduce exposure, keep powerful credentials protected, limit their lifetime, and maintain control over sessions when credentials are compromised.**