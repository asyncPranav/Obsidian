
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

---


Perfect. **Foundation is complete.** Now we move to the actual registration flow.

## Step 5 — Modify `register()`

Our current flow is:

```text
/register
   ↓
Create User
   ↓
Create Session
   ↓
Issue JWT
```

We now change it to:

```text
/register
   ↓
Check existing user
   ↓
Hash password
   ↓
Create User
   ↓
Generate OTP
   ↓
Hash OTP
   ↓
Store OTP
   ↓
Send OTP
   ↓
Return registration success
```

### Important change

We **will not create a session or issue JWT during registration**.

The user is:

```js
isEmailVerified: false
```

until OTP verification succeeds.

---

### Before we modify the controller

There's one piece we need first: **how we're going to send the OTP email**.

For development, we have two good approaches:

1. **Actually send email** using an email service such as Resend/Nodemailer.
    
2. **Temporarily log the OTP** to the terminal so we can finish and test the authentication logic before integrating email delivery.
    

---
Yes. Since **we are using Nodemailer**, and you already have Google OAuth configuration in your `config.js`, we can build `email.service.js` using **Gmail OAuth2** rather than `SMTP_PASSWORD`.

Your existing environment variables are:

```env
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
GOOGLE_REFRESH_TOKEN=
GOOGLE_USER=
```

## Step 6 — Create `email.service.js`

First make sure Nodemailer is installed:

```bash
npm install nodemailer
```

Then create:

```text
src/services/email.service.js
```

Use:

```js
import nodemailer from "nodemailer";
import config from "../config/config.js";

const transporter = nodemailer.createTransport({
  service: "gmail",
  auth: {
    type: "OAuth2",
    user: config.googleUser,
    clientId: config.googleClientId,
    clientSecret: config.googleClientSecret,
    refreshToken: config.googleRefreshToken,
  },
});

const sendVerificationEmail = async (email, otp) => {
  await transporter.sendMail({
    from: config.googleUser,
    to: email,
    subject: "Verify your email",
    text: `Your email verification OTP is ${otp}. It will expire in 5 minutes.`,
  });
};

export { sendVerificationEmail };
```

### What is happening here?

```text
config.js
   │
   ├── googleUser
   ├── googleClientId
   ├── googleClientSecret
   └── googleRefreshToken
          ↓
     Nodemailer
          ↓
       Gmail
          ↓
    User's email
```

The important part is:

```js
auth: {
  type: "OAuth2",
  user: config.googleUser,
  clientId: config.googleClientId,
  clientSecret: config.googleClientSecret,
  refreshToken: config.googleRefreshToken,
}
```

Nodemailer uses your **Google OAuth2 refresh token** to authenticate instead of storing your Gmail password.

### Your `.env`

It should therefore have something like:

```env
GOOGLE_CLIENT_ID=your_client_id
GOOGLE_CLIENT_SECRET=your_client_secret
GOOGLE_REFRESH_TOKEN=your_refresh_token
GOOGLE_USER=yourgmail@gmail.com
```

**Don't send these actual values here**, especially the refresh token.

---

## Step 6.1 — One small test

Before connecting this to registration, we should test that the email service itself works.

Temporarily add this at the bottom of `email.service.js`:

```js
sendVerificationEmail(
  "your-test-email@example.com",
  "123456",
)
  .then(() => console.log("Verification email sent successfully"))
  .catch((error) => console.error("Email sending failed:", error.message));
```

Run your server.

If everything is configured correctly, you should receive the email.

**After testing, remove that test code.**

Then we'll move to **Step 7 — modify `register()` to call `sendVerificationEmail(email, otp)`**.