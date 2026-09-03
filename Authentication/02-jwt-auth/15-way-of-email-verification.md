
----

# Email OTP Verification During Registration

## Option 1 — Verify Email Before Creating User

### Flow

```text
POST /register
      ↓
Validate username/email/password
      ↓
Generate OTP
      ↓
Store OTP securely
      ↓
Send OTP to email
      ↓
POST /verify-email
      ↓
Verify OTP
      ↓
Create User
      ↓
Create Session
      ↓
Issue Access + Refresh Tokens
```

### Characteristics

- User is **not created** until email ownership is verified.
    
- OTP data is temporarily associated with the registration attempt.
    
- Failed/abandoned registrations leave no unverified `User` document.
    
- Successful verification creates the account and authenticates the user.
    

### Advantages

- `User` collection contains only verified accounts.
    
- No inactive/unverified accounts to manage.
    
- Clean account lifecycle.
    

### Disadvantages

- Registration data must be temporarily stored somewhere until verification.
    
- Need to handle abandoned registration attempts and expiration.
    
- More complex if the user wants to change their email/password before verification.
    

---

# Option 2 — Create User, Then Verify Email

### Flow

```text
POST /register
      ↓
Validate data
      ↓
Create User
isEmailVerified: false
      ↓
Generate + send OTP
      ↓
POST /verify-email
      ↓
Verify OTP
      ↓
isEmailVerified = true
      ↓
Create Session
      ↓
Issue Access + Refresh Tokens
```

### Characteristics

User exists immediately:

```js
{
  email: "user@example.com",
  password: "<hashed>",
  isEmailVerified: false
}
```

After successful verification:

```js
isEmailVerified: true
```

The application then allows access to resources requiring verified email.

### Advantages

- Simple account lifecycle.
    
- Registration data is already persisted.
    
- Easy to support **resend OTP**, retry verification, and future email-verification flows.
    
- User identity exists even while verification is pending.
    

### Disadvantages

- Unverified accounts remain in the database.
    
- Requires cleanup/handling of abandoned unverified accounts.
    
- Unique email/username records may prevent another registration using the same values.
    

---

# Industry Recommendation

For a normal application, **Option 2 is a very practical design**:

```text
Create account
     ↓
isEmailVerified = false
     ↓
Verify email
     ↓
isEmailVerified = true
```

OWASP recommends verifying email ownership before enabling account use and recommends verification codes/tokens be **cryptographically random, time-limited, and single-use**. ([OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/Email_Validation_and_Verification_Cheat_Sheet.html?utm_source=chatgpt.com "Email Validation and Verification - OWASP Cheat Sheet Series"))

So for **our JWT project**, we'll use **Option 2**.

---

# Our Final Architecture

### User

```js
isEmailVerified: {
  type: Boolean,
  default: false,
}
```

### Registration

```text
/register
   ↓
Create User
   ↓
isEmailVerified = false
   ↓
Generate OTP
   ↓
Store hashed OTP
   ↓
Send OTP
```

**No normal application access yet.**

### Verification

```text
/verify-email
   ↓
Find OTP
   ↓
Check expiration
   ↓
Check attempts
   ↓
Compare OTP
   ↓
Mark user verified
   ↓
Invalidate OTP
   ↓
Create session
   ↓
Issue JWT pair
```

### Protected resources

```text
Request
   ↓
Access Token
   ↓
authenticate
   ↓
requireVerifiedEmail
   ↓
Controller
```

This gives us separate responsibilities:

```text
authenticate
    → Is the user authenticated?

requireVerifiedEmail
    → Has the user's email been verified?
```

---

# OTP Security Standards

For our implementation:

|Requirement|Decision|
|---|---|
|Generation|Cryptographically secure random generator|
|Storage|Hash OTP; don't store plaintext|
|Lifetime|Short TTL, e.g. 5 minutes|
|Usage|Single-use|
|Attempts|Strict maximum|
|Resend|Generate new OTP and invalidate/replace old one|
|Logging|Never log OTP|
|Rate limiting|Required|
|Successful verification|Immediately invalidate OTP|
|Account access|Block until email verification|

OWASP specifically recommends short TTLs, single-use OTPs, strict attempt limits, secure random generation, and not storing OTPs in long-term plaintext. ([OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html?utm_source=chatgpt.com "Multifactor Authentication - OWASP Cheat Sheet Series"))

### Important distinction

**Email verification ≠ strong MFA.**

It establishes that the user has access to the email account, but email itself is considered a relatively weak authentication factor. ([OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html?utm_source=chatgpt.com "Multifactor Authentication - OWASP Cheat Sheet Series"))

---

## Decision for our project

We'll implement:

> **Option 2 — Create User → `isEmailVerified: false` → Email OTP → Verify → `isEmailVerified: true` → issue JWT/session → allow protected resources.**

And we'll keep the implementation **industry-like without unnecessary complexity**.

[OWASP Email Validation and Verification Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Email_Validation_and_Verification_Cheat_Sheet.html?utm_source=chatgpt.com)

[OWASP Multifactor Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html?utm_source=chatgpt.com)