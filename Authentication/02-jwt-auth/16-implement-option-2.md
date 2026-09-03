
---

#### Here we are implementing option-02 of email verification
`Check these notes` - [[15-way-of-email-verification]]

Yes. Let's implement **Option 2** in our existing project, one step at a time.

We will **not change the whole authentication architecture**. We'll add email verification on top of what already works.

## 1. Final flow

```text
/register
   ↓
Validate registration data
   ↓
Create User
isEmailVerified = false
   ↓
Generate OTP
   ↓
Store OTP securely
   ↓
Send OTP to email
   ↓
Registration successful
```

Then:

```text
/verify-email
   ↓
Verify OTP
   ↓
isEmailVerified = true
   ↓
Create Session
   ↓
Issue Access + Refresh Tokens
```

OWASP recommends that email ownership be verified before enabling account use, and OTPs should be short-lived, single-use, securely generated, and protected against brute-force attempts. ([OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/Email_Validation_and_Verification_Cheat_Sheet.html?utm_source=chatgpt.com "Email Validation and Verification - OWASP Cheat Sheet Series"))

---

# Step 1 — Modify `User` model

Add this field to `user.model.js`:

```js
isEmailVerified: {
  type: Boolean,
  default: false,
},
```

So the relevant part becomes:

```js
const userSchema = new mongoose.Schema(
  {
    username: {
      type: String,
      required: [true, "Username is required"],
      unique: true,
      trim: true,
      lowercase: true,
    },

    email: {
      type: String,
      required: [true, "Email is required"],
      unique: true,
      trim: true,
      lowercase: true,
      validate: {
        validator: (value) => validator.isEmail(value),
        message: "Invalid email format",
      },
    },

    password: {
      type: String,
      required: [true, "Password is required"],
      minlength: [6, "Password must be at least 6 characters long"],
      // ...
    },

    isEmailVerified: {
      type: Boolean,
      default: false,
    },
  },
  {
    timestamps: true,
  },
);
```

Existing users in MongoDB will effectively have `undefined` for this new field unless migrated, so for our development database we can handle that as we go. New registrations will explicitly get the default `false`.

---

# Step 2 — Create OTP model

Create:

```text
src/models/otp.model.js
```

We want the OTP record to answer:

> Who is this OTP for, what is its purpose, is it still valid, and how many attempts have been made?

Initial schema:

```js
import mongoose from "mongoose";

const otpSchema = new mongoose.Schema(
  {
    email: {
      type: String,
      required: [true, "Email is required"],
      lowercase: true,
      trim: true,
    },

    otpHash: {
      type: String,
      required: [true, "OTP hash is required"],
    },

    purpose: {
      type: String,
      enum: ["email_verification"],
      required: [true, "OTP purpose is required"],
    },

    expiresAt: {
      type: Date,
      required: [true, "OTP expiry is required"],
    },

    attempts: {
      type: Number,
      default: 0,
    },
  },
  {
    timestamps: true,
  },
);

const Otp = mongoose.model("Otp", otpSchema);

export default Otp;
```

### Why these fields?

|Field|Purpose|
|---|---|
|`email`|Associates OTP with registration account|
|`otpHash`|Stores OTP securely instead of plaintext|
|`purpose`|Prevents one OTP record being reused for unrelated flows|
|`expiresAt`|Makes OTP time-limited|
|`attempts`|Enables brute-force protection|
|`timestamps`|Tracks creation/update time|

OWASP specifically recommends short TTLs, strict attempt limits, single use, and avoiding plaintext OTP storage. ([OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html?utm_source=chatgpt.com "Multifactor Authentication - OWASP Cheat Sheet Series"))

---

# Step 3 — OTP generation utility

Create:

```text
src/utils/otp.util.js
```

**Don't use:**

```js
Math.random()
```

for security-sensitive OTP generation.

Node provides a cryptographically secure random generator, which is appropriate for this purpose.

We'll use:

```js
import crypto from "node:crypto";

const generateOtp = () => {
  return crypto.randomInt(100000, 1000000).toString();
};

export default generateOtp;
```

This generates a **6-digit OTP**:

```text
100000 → 999999
```

The OTP should never be logged in production. OWASP explicitly recommends not logging OTP values. ([OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html?utm_source=chatgpt.com "Multifactor Authentication - OWASP Cheat Sheet Series"))

---

# Step 4 — Don't implement the email sender yet

At this point, **stop here**.

We have established:

```text
User
 └── isEmailVerified

Otp
 ├── email
 ├── otpHash
 ├── purpose
 ├── expiresAt
 └── attempts

otp.util.js
 └── generateOtp()
```

Next we'll implement the actual registration modification:

```text
/register
    ↓
Create User(false)
    ↓
Generate OTP
    ↓
bcrypt.hash(OTP)
    ↓
Create OTP record
    ↓
Send email
```

Then we'll build `/verify-email`.

### One architectural decision

I suggest we **finish the database + OTP generation logic first**, and only then integrate an email provider. That way, you understand the authentication flow independently of the email service.

So **your immediate task is only these two changes**:

1. Add `isEmailVerified` to `User`.
    
2. Create `otp.model.js` and `otp.util.js`.
    

Once you've done and tested that, we'll modify your existing `register()` controller.