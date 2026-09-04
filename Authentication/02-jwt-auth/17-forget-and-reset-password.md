

----

# Step 13 — Forgot Password & Reset Password

This step adds a **secure password recovery system** using the same OTP infrastructure you already built for email verification.

The design follows your existing architecture and OWASP password-reset guidance: reset codes should be cryptographically random, securely stored, single-use, short-lived, and protected against brute-force and excessive requests.

---

## 13.1 Objective

Allow a user who has forgotten their password to securely set a new password **without knowing the old password**.

### Features

- Request password-reset OTP
    
- Send OTP through email
    
- Hash OTP before storing
    
- 10-minute OTP expiry
    
- Maximum 3 OTP verification attempts
    
- 60-second resend/request cooldown
    
- Only latest OTP remains valid
    
- Single-use OTP
    
- Hash new password with bcrypt
    
- Invalidate all existing sessions after password reset
    
- **Do not automatically log the user in after reset**
    

---

# 13.2 Complete Flow

### Forgot Password

```text
POST /forgot-password
        ↓
Validate email
        ↓
Find user
        ↓
Check existing reset OTP
        ↓
Check 60-second cooldown
        ↓
Generate 6-digit OTP
        ↓
Hash OTP
        ↓
Delete previous password-reset OTP
        ↓
Store new OTP
        ↓
Send OTP via email
        ↓
Return success
```

### Reset Password

```text
POST /reset-password
        ↓
Validate email + OTP + newPassword
        ↓
Find OTP
        ↓
Check OTP exists
        ↓
Check attempt limit
        ↓
Check expiry
        ↓
Verify OTP
        ↓
Find user
        ↓
Hash new password
        ↓
Update password
        ↓
Revoke all existing sessions
        ↓
Delete used OTP
        ↓
Send password-changed email
        ↓
Return success
```

---

# 13.3 Why We Use the Existing OTP Model

We already have:

```js
purpose: "email_verification"
```

Instead of creating another model such as:

```text
passwordResetOtp.model.js
```

we reuse:

```text
otp.model.js
```

and add:

```js
"password_reset"
```

to the `purpose` enum.

This gives us:

```text
Otp
├── email_verification
└── password_reset
```

This is cleaner and avoids duplicate OTP logic.

---

# 13.4 Modify OTP Model

Change:

```js
purpose: {
  type: String,
  enum: ["email_verification"],
  required: [true, "OTP purpose is required"],
},
```

to:

```js
purpose: {
  type: String,
  enum: ["email_verification", "password_reset"],
  required: [true, "OTP purpose is required"],
},
```

The rest of the model remains unchanged.

---

# 13.5 Forgot Password Endpoint

### Endpoint

```http
POST /api/auth/forgot-password
```

### Request

```json
{
  "email": "user@example.com"
}
```

### Responsibilities

The controller should:

1. Receive email.
    
2. Find the user.
    
3. Check whether a reset OTP already exists.
    
4. Enforce the 60-second cooldown.
    
5. Generate a new OTP.
    
6. Hash the OTP.
    
7. Delete the previous password-reset OTP.
    
8. Store the new OTP.
    
9. Send OTP through email.
    
10. Return a response.
    

---

# 13.6 Important Security Rule — Account Enumeration

A production-grade forgot-password endpoint should **not reveal whether an email is registered**.

Bad:

```json
{
  "message": "User not found"
}
```

An attacker could submit thousands of emails and discover which accounts exist.

Therefore the preferred response is:

```json
{
  "message": "If an account exists with this email, a password reset OTP has been sent."
}
```

Even if the email doesn't exist, the response remains the same.

### Important distinction

For an **industry-standard implementation**, don't expose account existence.

---

# 13.7 Forgot Password OTP

Use the same utility:

```js
generateOTP();
```

It generates:

```text
6-digit cryptographically secure OTP
```

Example:

```text
583214
```

Do not use:

```js
Math.random()
```

Your existing implementation uses Node's cryptographically secure random generator, which is the correct approach.

---

# 13.8 Store OTP Securely

Never store:

```js
otp: "583214"
```

Instead:

```js
const hashedOtp = await bcrypt.hash(otp, 10);
```

Store:

```js
{
  email,
  otpHash: hashedOtp,
  purpose: "password_reset",
  expiresAt: ...
}
```

Therefore, even if the OTP collection is exposed, the actual OTP isn't directly stored.

---

# 13.9 OTP Expiry

Use:

```text
10 minutes
```

Example:

```js
expiresAt: new Date(Date.now() + 10 * 60 * 1000)
```

The OTP is valid only until:

```text
expiresAt
```

After that:

```text
OTP → invalid
```

---

# 13.10 OTP Attempt Limit

Keep your existing rule:

```text
Maximum 3 attempts
```

Example:

```js
if (otpRecord.attempts >= 3) {
  throw new ApiError(
    400,
    "Maximum OTP verification attempts exceeded. Please request a new OTP.",
  );
}
```

For an incorrect OTP:

```js
otpRecord.attempts += 1;
await otpRecord.save();
```

This prevents unlimited guessing of a 6-digit OTP.

---

# 13.11 60-Second Cooldown

The same cooldown mechanism used for email verification should be used here.

```text
OTP lifetime       = 10 minutes
Request cooldown   = 60 seconds
Maximum attempts   = 3
```

These are **three different security controls**.

### `createdAt`

Answers:

> When was this OTP generated?

Used for:

```text
60-second cooldown
```

### `expiresAt`

Answers:

> Is this OTP still valid?

Used for:

```text
10-minute expiration
```

### `attempts`

Answers:

> How many incorrect OTP attempts have occurred?

Used for:

```text
maximum 3 attempts
```

---
**`NOTE`**  : You have to first hit `/forget-password route to get an OTP with `password_reset` purpose then thereafter you can use that OTP in `reset-password` request body to reset password  

---
# 13.12 Complete `forgotPassword()` Controller

Add this function to:

```text
src/controllers/auth.controller.js
```

```js
const forgotPassword = async (req, res, next) => {
  try {
    const { email } = req.body;

    // 1. Find user
    const user = await userModel.findOne({ email });

    // 2. Do not reveal whether the email exists
    if (!user) {
      return res.status(200).json({
        message:
          "If an account exists with this email, a password reset OTP has been sent.",
      });
    }

    // 3. Check if a password reset OTP already exists
    const existingOtp = await otpModel.findOne({
      email,
      purpose: "password_reset",
    });

    // 4. Handle existing OTP
    if (existingOtp) {
      const now = new Date();

      // 4a. If OTP has expired, delete it
      if (existingOtp.expiresAt <= now) {
        await otpModel.deleteOne({
          _id: existingOtp._id,
        });
      } else {
        // 4b. OTP is still valid → check 60-second cooldown
        const cooldown = 60 * 1000;

        const timePassed =
          Date.now() - existingOtp.createdAt.getTime();

        if (timePassed < cooldown) {
          const timeLeft = Math.ceil(
            (cooldown - timePassed) / 1000,
          );

          throw new ApiError(
            429,
            `Please wait ${timeLeft} seconds before requesting a new password reset OTP.`,
          );
        }

        // 4c. Cooldown has passed → delete old OTP
        await otpModel.deleteOne({
          _id: existingOtp._id,
        });
      }
    }

    // 5. Generate new OTP
    const otp = generateOTP();

    // 6. Hash OTP
    const hashedOtp = await bcrypt.hash(otp, 10);

    // 7. Store new OTP
    await otpModel.create({
      email,
      otpHash: hashedOtp,
      purpose: "password_reset",
      expiresAt: new Date(Date.now() + 10 * 60 * 1000),
    });

    // 8. Send OTP email
    await sendEmail(
      email,
      "Reset your password",
      `Your password reset OTP is ${otp}. It will expire in 10 minutes.`,
      `
        <h2>Password Reset</h2>
        <p>Your password reset OTP is:</p>
        <h1>${otp}</h1>
        <p>This OTP will expire in 10 minutes.</p>
        <p>If you did not request a password reset, you can ignore this email.</p>
      `,
    );

    // 9. Success response
    return res.status(200).json({
      message:
        "If an account exists with this email, a password reset OTP has been sent.",
    });
  } catch (error) {
    next(error);
  }
};
```

### Why this controller works

The important part is:

```js
if (!user) {
  return res.status(200).json({
    message:
      "If an account exists with this email, a password reset OTP has been sent.",
  });
}
```

The attacker cannot distinguish:

```text
registered email
       ↓
200
```

from:

```text
unregistered email
       ↓
200
```

This prevents basic account enumeration.

---

# 13.13 Reset Password Endpoint

### Endpoint

```http
POST /api/auth/reset-password
```

### Request

```json
{
  "email": "user@example.com",
  "otp": "583214",
  "newPassword": "NewStrongPassword123",
  "confirmPassword": "NewStrongPassword123"
}
```

---

# 13.14 Reset Password Validation

The validator should verify:

### Email

```text
Valid email format
```

### OTP

```text
Exactly 6 digits
```

Example:

```regex
/^\d{6}$/
```

### New password

At minimum:

```text
Minimum 8 characters
```

If your registration validator already has a stronger password rule, **reuse the same rule** here rather than creating a different password policy.

### Confirm password

```text
confirmPassword === newPassword
```

---

# 13.15 Complete `resetPassword()` Controller

Add this function to:

```text
src/controllers/auth.controller.js
```

```js
const resetPassword = async (req, res, next) => {
  try {
    const { email, otp, newPassword } = req.body;

    // 1. Find password reset OTP
    const otpRecord = await otpModel.findOne({
      email,
      purpose: "password_reset",
    });

    if (!otpRecord) {
      throw new ApiError(
        400,
        "Invalid or expired password reset OTP",
      );
    }

    // 2. Check maximum attempts
    if (otpRecord.attempts >= 3) {
      throw new ApiError(
        400,
        "Maximum OTP verification attempts exceeded. Please request a new OTP.",
      );
    }

    // 3. Check OTP expiry
    if (otpRecord.expiresAt <= new Date()) {
      await otpModel.deleteOne({
        _id: otpRecord._id,
      });

      throw new ApiError(
        400,
        "Invalid or expired password reset OTP",
      );
    }

    // 4. Verify OTP
    const isOtpValid = await bcrypt.compare(
      otp,
      otpRecord.otpHash,
    );

    if (!isOtpValid) {
      otpRecord.attempts += 1;
      await otpRecord.save();

      throw new ApiError(
        400,
        "Invalid or expired password reset OTP",
      );
    }

    // 5. Find user
    const user = await userModel.findOne({ email });

    if (!user) {
      throw new ApiError(
        400,
        "Invalid or expired password reset OTP",
      );
    }

    // 6. Hash new password
    const hashedPassword = await bcrypt.hash(
      newPassword,
      10,
    );

    // 7. Update password
    user.password = hashedPassword;
    await user.save();

    // 8. Revoke all existing sessions
    await sessionModel.updateMany(
      { user: user._id },
      { $set: { revoked: true } },
    );

    // 9. Delete used OTP
    await otpModel.deleteOne({
      _id: otpRecord._id,
    });

    // 10. Send password reset confirmation email
    await sendEmail(
      email,
      "Your password was changed",
      "Your password was changed successfully. If you did not make this change, please secure your account immediately.",
      `
        <h2>Password Changed Successfully</h2>
        <p>Your password has been changed successfully.</p>
        <p>
          If you did not make this change, please secure your account immediately.
        </p>
      `,
    );

    // 11. Success response
    return res.status(200).json({
      message:
        "Password reset successfully. Please login again with your new password.",
    });
  } catch (error) {
    next(error);
  }
};
```

---

# 13.16 Reset Password Verification

First find the OTP:

```js
const otpRecord = await otpModel.findOne({
  email,
  purpose: "password_reset",
});
```

Then:

### 1. OTP exists?

```text
No → reject
```

### 2. Attempts exceeded?

```text
3 attempts → reject
```

### 3. OTP expired?

```text
expiresAt < current time → reject
```

### 4. Compare OTP

```js
const isOtpValid = await bcrypt.compare(
  otp,
  otpRecord.otpHash,
);
```

If invalid:

```text
attempts++
```

If valid:

```text
continue password reset
```

---

# 13.17 Update Password

Find the user:

```js
const user = await userModel.findOne({ email });
```

Then hash the new password:

```js
const hashedPassword = await bcrypt.hash(
  newPassword,
  10,
);
```

Update:

```js
user.password = hashedPassword;
await user.save();
```

Never store:

```js
password: newPassword
```

Passwords must always be stored as password hashes.

---

# 13.18 Invalidate Existing Sessions

This is **very important** for your architecture.

Suppose an attacker already has a valid refresh token:

```text
Attacker
   ↓
Valid refresh token
   ↓
Existing session
```

The legitimate user resets their password.

If you don't revoke sessions, the attacker may still be able to refresh their access token.

Therefore after successful password reset:

```js
await sessionModel.updateMany(
  { user: user._id },
  { $set: { revoked: true } },
);
```

This invalidates all existing sessions.

---

# 13.19 Do NOT Automatically Login

After password reset:

```text
Password changed
        ↓
Sessions revoked
        ↓
User must login again
```

Do **not** generate:

```text
Access Token
Refresh Token
```

inside `/reset-password`.

Instead:

```text
POST /reset-password
        ↓
Password successfully changed
        ↓
All sessions revoked
        ↓
User logs in normally
```

This keeps password recovery separate from normal authentication.

---

# 13.20 Delete OTP After Successful Reset

OTP must be single-use.

After successful password reset:

```js
await otpModel.deleteOne({
  _id: otpRecord._id,
});
```

Therefore:

```text
First use → valid
Second use → OTP doesn't exist → rejected
```

This provides replay protection.

---

# 13.21 Email After Password Reset

After successful reset, send a notification email:

```text
Your password has been changed successfully.
```

Do **not** send the new password.

Example:

```text
Subject:
Your password was changed
```

Email should contain:

```text
Your password was successfully changed.

If you did not make this change, please secure your account immediately.
```

---

# 13.22 Routes

Your `auth.routes.js` should contain:

```js
router.post(
  "/forgot-password",
  forgotPasswordValidator,
  validate,
  forgotPassword,
);

router.post(
  "/reset-password",
  resetPasswordValidator,
  validate,
  resetPassword,
);
```

No `authenticate` middleware should be used.

Why?

Because the user has **forgotten their password** and therefore doesn't have to be authenticated.

---

# 13.23 Validators

Add:

```js
const forgotPasswordValidator = [
  body("email")
    .trim()
    .isEmail()
    .withMessage("Please provide a valid email"),
];
```

And:

```js
const resetPasswordValidator = [
  body("email")
    .trim()
    .isEmail()
    .withMessage("Please provide a valid email"),

  body("otp")
    .trim()
    .matches(/^\d{6}$/)
    .withMessage("OTP must be exactly 6 digits"),

  body("newPassword")
    .isLength({ min: 8 })
    .withMessage("Password must be at least 8 characters"),

  body("confirmPassword")
    .custom((value, { req }) => value === req.body.newPassword)
    .withMessage("Passwords do not match"),
];
```

**Important:** If your registration password validator already has a stronger rule, use that same rule here.

---

# 13.24 Controller Exports

Add both functions to the existing export:

```js
export {
  register,
  login,
  getMe,
  refreshTokens,
  logout,
  logoutAll,
  getSessions,
  verifyEmail,
  resendVerificationOtp,
  forgotPassword,
  resetPassword,
};
```

---

# 13.25 Complete Security Checklist

After Step 13, your password recovery system should satisfy:

|Security requirement|Status|
|---|---|
|Cryptographically random OTP|✅|
|6-digit OTP|✅|
|OTP hashed in DB|✅|
|10-minute expiry|✅|
|Maximum 3 attempts|✅|
|OTP single-use|✅|
|Old OTP replaced|✅|
|60-second cooldown|✅|
|Expired OTP cleanup|✅|
|Password hashed with bcrypt|✅|
|Password confirmation|✅|
|Existing sessions revoked|✅|
|No automatic login|✅|
|Password-reset email|✅|
|Email input validation|✅|
|OTP input validation|✅|
|Consistent forgot-password response|✅|
|No password sent by email|✅|
|No OTP logged in production|✅|

---

# 13.26 Final Architecture

After implementing Step 13:

```text
                    AUTH SYSTEM
                         │
          ┌──────────────┴──────────────┐
          │                             │
       REGISTER                       LOGIN
          │                             │
    Email Verification             Email Verified?
          │                             │
         OTP                           Yes
          │                             │
          ▼                             ▼
     Verify Email                 Create Session
          │                             │
          ▼                             ▼
    Create Session                Access Token
          │                       Refresh Token
          ▼
   Access + Refresh
```

And password recovery:

```text
              FORGOT PASSWORD
                     │
                     ▼
               Enter Email
                     │
                     ▼
              Generate OTP
                     │
                     ▼
                Hash OTP
                     │
                     ▼
               Store OTP
                     │
                     ▼
               Send Email
                     │
                     ▼
                Enter OTP
                     │
                     ▼
               Verify OTP
                     │
                     ▼
              New Password
                     │
                     ▼
              Hash Password
                     │
                     ▼
              Update User
                     │
                     ▼
            Revoke All Sessions
                     │
                     ▼
               Delete OTP
                     │
                     ▼
                Login Again
```

---

# 13.27 Implementation Order

Don't implement everything at once. Do it in this order:

```text
13.1   Modify OTP model
          ↓
13.2   Create forgotPasswordValidator
          ↓
13.3   Create resetPasswordValidator
          ↓
13.4   Implement forgotPassword()
          ↓
13.5   Implement resetPassword()
          ↓
13.6   Add routes
          ↓
13.7   Add controller exports
          ↓
13.8   Test forgot-password
          ↓
13.9   Test reset-password
          ↓
13.10  Test OTP abuse cases
          ↓
13.11  Test session invalidation
```

### Final Step 13 endpoint summary

|Endpoint|Authentication|Purpose|
|---|---|---|
|`POST /forgot-password`|❌|Send password-reset OTP|
|`POST /reset-password`|❌|Verify OTP and change password|

After Step 13, the authentication system will have:

```text
REGISTER
   ↓
EMAIL VERIFICATION
   ↓
LOGIN
   ↓
ACCESS + REFRESH TOKENS
   ↓
SESSION MANAGEMENT
   ↓
FORGOT PASSWORD
   ↓
RESET PASSWORD
```

This keeps the implementation consistent with the OTP system you already built rather than introducing a separate password-reset architecture.