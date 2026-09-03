
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

## Step 6 — Create the Email Service

Since you want to **actually send the OTP through Gmail**, we'll use **Nodemailer + Gmail OAuth2**.

Your current architecture already has:

```text
src/
└── services/
    └── email.service.js
```

### 6.1 Install Nodemailer

If you haven't already:

```bash
npm install nodemailer
```

---

### 6.2 Environment variables

You already have the Google OAuth2 configuration:

```env
GOOGLE_CLIENT_ID=...
GOOGLE_CLIENT_SECRET=...
GOOGLE_REFRESH_TOKEN=...
GOOGLE_USER=yourgmail@gmail.com
```

**Do not share these secret values with me.**

Your `config.js` should expose them:

```js
googleClientId: process.env.GOOGLE_CLIENT_ID,
googleClientSecret: process.env.GOOGLE_CLIENT_SECRET,
googleRefreshToken: process.env.GOOGLE_REFRESH_TOKEN,
googleUser: process.env.GOOGLE_USER,
```

And these should be included in your required environment variables.

---

### 6.3 Create `email.service.js`

Create:

```text
src/services/email.service.js
```

Put this inside:

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

// Verify the connection configuration
transporter.verify((error, success) => {
  if (error) {
    console.error("Error connecting to email server:", error);
  } else {
    console.log("Email server is ready to send messages");
  }
});

const sendEmail = async (to, subject, text, html) => {
  try {
    const info = await transporter.sendMail({
      from: `"Pranav" <${config.googleUser}>`,
      to,
      subject,
      text,
      html,
    });

    console.log("Message sent:", info.messageId);

    return info;
  } catch (error) {
    console.error("Error sending email:", error);
    throw error;
  }
};

export default sendEmail;
```

### Why `throw error`?

This part is important:

```js
catch (error) {
  console.error("Error sending email:", error);
  throw error;
}
```

The email service shouldn't silently hide a failure.

Later:

```text
register()
    ↓
sendEmail()
    ↓
Gmail fails
    ↓
error thrown
    ↓
register() catch
    ↓
next(error)
    ↓
global error middleware
```

---

### 6.4 One thing to remove from your version

If you currently have:

```js
console.log("Preview URL: %s", nodemailer.getTestMessageUrl(info));
```

**remove it.**

`getTestMessageUrl()` is useful with Nodemailer's test/Ethereal transport. You're using your real Gmail account, so you don't need it.

---

## What Step 6 accomplishes

We now have a reusable email service:

```text
Controller
    ↓
sendEmail(to, subject, text, html)
    ↓
Nodemailer
    ↓
Gmail OAuth2
    ↓
User's email inbox
```

The controller will later simply do:

```js
await sendEmail(
  email,
  "Verify your email",
  "Your OTP is 123456",
  "<h1>Your OTP is 123456</h1>",
);
```

We **do not modify `register()` yet**.

### Your current structure

```text
src/
├── models/
│   ├── user.model.js
│   ├── session.model.js
│   └── otp.model.js
│
├── utils/
│   ├── otp.util.js
│   └── ...
│
└── services/
    └── email.service.js   ← Step 6
```

**Step 6 is complete when `email.service.js` is created and the Gmail OAuth2 transporter is configured.**

After that, **Step 7 will modify `register()`** to:

```text
Create User
    ↓
Generate OTP
    ↓
Hash OTP
    ↓
Store OTP
    ↓
sendEmail()
    ↓
Return success
```

And importantly, **no session/JWT will be created during registration**.


---


Perfect. **Step 7 is fully tested and working.** Your OTP is actually reaching the user's email, so now we can move to the important part.

# Step 8 — Create `/verify-email`

Now we'll verify the OTP and, **only after successful verification**, create the session and issue JWT tokens.

Our flow:

```text
POST /verify-email
        ↓
Find OTP
        ↓
Check attempts
        ↓
Check expiry
        ↓
Compare OTP with hash
        ↓
        ├── Invalid → increase attempts
        │
        └── Valid
              ↓
        Find User
              ↓
        isEmailVerified = true
              ↓
        Delete OTP
              ↓
        Create Session
              ↓
        Generate Access Token
              ↓
        Generate Refresh Token
              ↓
        Set Refresh Cookie
```

## 8.1 Modify `auth.controller.js`

We already imported:

```js
import otpModel from "../models/otp.model.js";
```

So we don't need another import.

Add this new controller **below `register()`**:

```js
const verifyEmail = async (req, res, next) => {
  try {
    const { email, otp } = req.body;

    // 1. Find OTP
    const otpRecord = await otpModel.findOne({
      email,
      purpose: "email_verification",
    });

    if (!otpRecord) {
      throw new ApiError(400, "OTP not found or expired");
    }

    // 2. Check maximum attempts
    if (otpRecord.attempts >= 5) {
      throw new ApiError(429, "Too many invalid OTP attempts");
    }

    // 3. Check expiry
    if (otpRecord.expiresAt < new Date()) {
      throw new ApiError(400, "OTP has expired");
    }

    // 4. Compare entered OTP with stored hash
    const isOtpValid = await bcrypt.compare(
      otp,
      otpRecord.otpHash,
    );

    if (!isOtpValid) {
      otpRecord.attempts += 1;
      await otpRecord.save();

      throw new ApiError(400, "Invalid OTP");
    }

    // 5. Find user
    const user = await userModel.findOne({ email });

    if (!user) {
      throw new ApiError(404, "User not found");
    }

    // 6. Mark email as verified
    user.isEmailVerified = true;
    await user.save();

    // 7. Delete OTP after successful verification
    await otpModel.deleteOne({
      _id: otpRecord._id,
    });

    // 8. Create session
    const sessionId = new mongoose.Types.ObjectId();

    const refreshToken = token.generateRefreshToken(
      user._id,
      sessionId,
    );

    const hashedRefreshToken = await bcrypt.hash(
      refreshToken,
      10,
    );

    const session = await sessionModel.create({
      _id: sessionId,
      user: user._id,
      refreshToken: hashedRefreshToken,
      ip: req.ip,
      userAgent: req.get("User-Agent"),
      expiresAt: new Date(
        Date.now() + 15 * 24 * 60 * 60 * 1000,
      ),
    });

    // 9. Generate access token
    const accessToken = token.generateAccessToken(
      user._id,
      session._id,
    );

    // 10. Set refresh token cookie
    cookie.setRefreshTokenCookie(res, refreshToken);

    return res.status(200).json({
      message: "Email verified successfully",
      user: {
        id: user._id,
        username: user.username,
        email: user.email,
        isEmailVerified: user.isEmailVerified,
      },
      accessToken,
    });
  } catch (error) {
    next(error);
  }
};
```

---

# 8.2 Export `verifyEmail`

At the bottom of `auth.controller.js`, you currently have something like:

```js
export {
  register,
  login,
  getMe,
  refreshTokens,
  logout,
  logoutAll,
  getSessions,
};
```

Add `verifyEmail`:

```js
export {
  register,
  verifyEmail,
  login,
  getMe,
  refreshTokens,
  logout,
  logoutAll,
  getSessions,
};
```

---

# 8.3 Add the route

Open:

```text
src/routes/auth.routes.js
```

Your imports currently look something like:

```js
import {
  register,
  login,
  getMe,
  refreshTokens,
  logout,
  logoutAll,
  getSessions,
} from "../controllers/auth.controller.js";
```

Add `verifyEmail`:

```js
import {
  register,
  verifyEmail,
  login,
  getMe,
  refreshTokens,
  logout,
  logoutAll,
  getSessions,
} from "../controllers/auth.controller.js";
```

Then add this route:

```js
router.post("/verify-email", verifyEmail);
```

So your relevant routes become:

```js
router.post("/register", register);

router.post("/verify-email", verifyEmail);

router.post("/login", login);

router.get("/me", authenticate, getMe);

router.post("/refresh", refreshTokens);

router.get("/sessions", authenticate, getSessions);

router.post("/logout", authenticate, logout);

router.post("/logout-all", authenticate, logoutAll);
```

---

# 8.4 Test `/verify-email`

You already received the OTP in:

```text
asyncpranav@gmail.com
```

Suppose your OTP is:

```text
123456
```

Send:

```http
POST /api/auth/verify-email
```

Body:

```json
{
  "email": "asyncpranav@gmail.com",
  "otp": "123456"
}
```

Use the **actual OTP from your email**, of course.

---

## 8.5 Expected successful response

You should get something like:

```json
{
  "message": "Email verified successfully",
  "user": {
    "id": "6a99a334b6e723896ec57b6a",
    "username": "pranav",
    "email": "asyncpranav@gmail.com",
    "isEmailVerified": true
  },
  "accessToken": "eyJhbGciOi..."
}
```

And Postman should receive the refresh-token cookie.

---

# 8.6 Check MongoDB

### User

Before verification:

```text
isEmailVerified: false
```

After successful verification:

```text
isEmailVerified: true
```

### OTP

Before:

```text
Otp document exists
```

After successful verification:

```text
Otp document deleted
```

### Session

Before verification:

```text
No session
```

After verification:

```text
Session created
refreshToken: hashed
revoked: false
expiresAt: ...
```

So we're finally getting the architecture we wanted:

```text
/register
    ↓
User created
    ↓
Email verification pending
```

then:

```text
/verify-email
    ↓
Email verified
    ↓
Session created
    ↓
Access + Refresh tokens issued
```

---

## ⚠️ Test the security behavior too

Don't test only the successful OTP.

### Wrong OTP

```json
{
  "email": "asyncpranav@gmail.com",
  "otp": "111111"
}
```

Expected:

```json
{
  "success": false,
  "message": "Invalid OTP"
}
```

And MongoDB:

```text
attempts: 1
```

Try another wrong OTP:

```text
attempts: 2
```

After 5 failed attempts:

```text
Too many invalid OTP attempts
```

### Expired OTP

Wait until `expiresAt` passes and try it.

Expected:

```text
OTP has expired
```

### Successful OTP

Use the correct OTP:

```text
isEmailVerified → true
OTP → deleted
Session → created
Access Token → issued
Refresh Cookie → set
```

**Stop after testing Step 8.** Don't modify `login()` yet. Once this works, the next step will be making sure an **unverified user cannot log in**, which completes the email-verification flow.