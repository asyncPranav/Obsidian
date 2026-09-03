
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

# Step 7 — Modify `register()`

Now we connect everything we created:

```text
User Model
    ↓
OTP Model
    ↓
OTP Generator
    ↓
Email Service
```

Our registration flow becomes:

```text
POST /register
      ↓
Check existing user
      ↓
Hash password
      ↓
Create User
isEmailVerified = false
      ↓
Generate 6-digit OTP
      ↓
Hash OTP
      ↓
Delete previous verification OTP
      ↓
Store new OTP
      ↓
Send OTP email
      ↓
Registration successful
```

**Important:** We will **NOT** create a session, access token, or refresh token here.

---

## 7.1 Add these imports

Open:

```text
src/controllers/auth.controller.js
```

You already have:

```js
import bcrypt from "bcrypt";
import mongoose from "mongoose";

import userModel from "../models/user.model.js";
import sessionModel from "../models/session.model.js";

import ApiError from "../utils/ApiError.js";
import * as token from "../utils/token.util.js";
import * as cookie from "../utils/cookie.util.js";
```

Add these three imports:

```js
import otpModel from "../models/otp.model.js";
import generateOtp from "../utils/otp.util.js";
import sendEmail from "../services/email.service.js";
```

So the imports become:

```js
import bcrypt from "bcrypt";
import mongoose from "mongoose";

import userModel from "../models/user.model.js";
import sessionModel from "../models/session.model.js";
import otpModel from "../models/otp.model.js";

import ApiError from "../utils/ApiError.js";
import * as token from "../utils/token.util.js";
import * as cookie from "../utils/cookie.util.js";
import generateOtp from "../utils/otp.util.js";
import sendEmail from "../services/email.service.js";
```

---

# 7.2 Replace the existing `register()`

Your old `register()` was creating:

```text
User
 ↓
Session
 ↓
Access Token
 ↓
Refresh Token
```

Replace **only the `register()` function** with this:

```js
const register = async (req, res, next) => {
  try {
    const { username, email, password } = req.body;

    // 1. Check if user already exists
    const isAlreadyRegistered = await userModel.findOne({
      $or: [{ username }, { email }],
    });

    if (isAlreadyRegistered) {
      throw new ApiError(409, "User already registered");
    }

    // 2. Hash password
    const hashedPassword = await bcrypt.hash(password, 10);

    // 3. Create user
    const newUser = await userModel.create({
      username,
      email,
      password: hashedPassword,
      isEmailVerified: false,
    });

    // 4. Generate OTP
    const otp = generateOtp();

    // 5. Hash OTP
    const hashedOtp = await bcrypt.hash(otp, 10);

    // 6. Remove any previous verification OTP
    await otpModel.deleteMany({
      email,
      purpose: "email_verification",
    });

    // 7. Store new OTP
    await otpModel.create({
      email,
      otpHash: hashedOtp,
      purpose: "email_verification",
      expiresAt: new Date(Date.now() + 5 * 60 * 1000),
    });

    // 8. Send OTP email
    await sendEmail(
      email,
      "Verify your email",
      `Your email verification OTP is ${otp}. It will expire in 5 minutes.`,
      `
        <h2>Verify your email</h2>
        <p>Your email verification OTP is:</p>
        <h1>${otp}</h1>
        <p>This OTP will expire in 5 minutes.</p>
      `,
    );

    // 9. Registration successful
    return res.status(201).json({
      message: "Registration successful. Please verify your email.",
      user: {
        id: newUser._id,
        username: newUser.username,
        email: newUser.email,
        isEmailVerified: newUser.isEmailVerified,
      },
    });
  } catch (error) {
    next(error);
  }
};
```

---

# 7.3 Understand each part

### 1. Create user

```js
const newUser = await userModel.create({
  username,
  email,
  password: hashedPassword,
  isEmailVerified: false,
});
```

The user exists, but:

```text
isEmailVerified = false
```

Therefore the account isn't verified yet.

---

### 2. Generate OTP

```js
const otp = generateOtp();
```

Example:

```text
583214
```

This is the **plain OTP** temporarily held in memory.

We need it to send to the user.

---

### 3. Hash OTP

```js
const hashedOtp = await bcrypt.hash(otp, 10);
```

MongoDB receives:

```text
583214
```

❌ No.

Instead it receives something like:

```text
$2b$10$..............
```

✅

So even if someone gets access to your OTP collection, they don't directly see the OTP.

---

### 4. Delete previous OTP

```js
await otpModel.deleteMany({
  email,
  purpose: "email_verification",
});
```

Suppose the user somehow requests another verification OTP later.

We don't want:

```text
OTP 1 → valid
OTP 2 → valid
OTP 3 → valid
```

Instead, the previous verification OTP is removed before creating the new one.

---

### 5. Store OTP

```js
await otpModel.create({
  email,
  otpHash: hashedOtp,
  purpose: "email_verification",
  expiresAt: new Date(Date.now() + 5 * 60 * 1000),
});
```

The OTP is valid for:

```text
5 minutes
```

MongoDB roughly contains:

```text
email
otpHash
purpose: "email_verification"
expiresAt
attempts: 0
```

---

### 6. Send email

```js
await sendEmail(...)
```

The plain OTP is used here:

```js
${otp}
```

The user receives something like:

```text
Subject: Verify your email

Your email verification OTP is:

583214

This OTP will expire in 5 minutes.
```

The plain OTP is **not stored in MongoDB**.

---

# 7.4 What changed from your old registration?

### ❌ Old

```text
/register
   ↓
Create User
   ↓
Create Session
   ↓
Generate Access Token
   ↓
Generate Refresh Token
   ↓
Set Cookie
   ↓
Return Access Token
```

### ✅ New

```text
/register
   ↓
Create User
   ↓
isEmailVerified = false
   ↓
Generate OTP
   ↓
Hash OTP
   ↓
Store OTP
   ↓
Send Email
   ↓
Return success
```

Therefore, after registration:

- ❌ No session
    
- ❌ No access token
    
- ❌ No refresh token
    
- ❌ No refresh-token cookie
    
- ✅ User created
    
- ✅ OTP created
    
- ✅ Email sent
    
- ✅ `isEmailVerified = false`
    

---

# 7.5 Test it in Postman

Send:

```http
POST /api/auth/register
```

Body:

```json
{
  "username": "testuser",
  "email": "your-email@gmail.com",
  "password": "12345678"
}
```

You should receive:

```json
{
  "message": "Registration successful. Please verify your email.",
  "user": {
    "id": "...",
    "username": "testuser",
    "email": "your-email@gmail.com",
    "isEmailVerified": false
  }
}
```

### Check MongoDB

User:

```text
isEmailVerified: false
```

OTP collection:

```text
email: your-email@gmail.com
otpHash: $2b$10$...
purpose: email_verification
expiresAt: ...
attempts: 0
```

And your Gmail inbox should contain the OTP.

---

## ⚠️ One important thing

If `sendEmail()` fails, it now throws the error:

```js
throw error;
```

So this:

```js
await sendEmail(...)
```

will go to:

```js
catch (error) {
  next(error);
}
```

That's why changing the email service to re-throw the error was important.

---

### Step 7 checkpoint

Don't implement `/verify-email` yet.

First confirm these **four things**:

```text
[ ] User created with isEmailVerified: false
[ ] OTP stored as a hash
[ ] OTP expires after 5 minutes
[ ] OTP actually arrives in Gmail
```

Once these work, **Step 8 will be `/verify-email`**, where the OTP is checked and only then we create the session and issue the JWT tokens.


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


---


# Step 9 — Prevent Unverified Users from Logging In

Now that `/verify-email` is working, we need to protect the `/login` endpoint.

Currently, an unverified user can still do:

```text
/register
   ↓
isEmailVerified = false
   ↓
/login
   ↓
Session created ❌
   ↓
JWT issued ❌
```

We want:

```text
/register
   ↓
isEmailVerified = false
   ↓
/login
   ↓
Check email verification
   ↓
NOT verified → reject ❌
```

Only this should be allowed:

```text
/register
   ↓
/verify-email
   ↓
isEmailVerified = true
   ↓
/login
   ↓
Session + JWT
```

---

## 9.1 Modify `login()`

Open:

```text
src/controllers/auth.controller.js
```

Find this section:

```js
const user = await userModel.findOne({ email }).select("+password");

if (!user) {
  throw new ApiError(401, "Invalid email or password");
}
```

Immediately **after** the user existence check, add:

```js
if (!user.isEmailVerified) {
  throw new ApiError(
    403,
    "Please verify your email before logging in",
  );
}
```

So it becomes:

```js
const user = await userModel.findOne({ email }).select("+password");

if (!user) {
  throw new ApiError(401, "Invalid email or password");
}

if (!user.isEmailVerified) {
  throw new ApiError(
    403,
    "Please verify your email before logging in",
  );
}
```

---

## 9.2 Why do we check this before the password?

The flow is now:

```text
Find user
   ↓
Does user exist?
   ↓
NO → Invalid email/password
   ↓
YES
   ↓
Email verified?
   ↓
NO → 403
   ↓
YES
   ↓
Compare password
   ↓
Create session
   ↓
Generate tokens
```

This prevents an unverified account from creating a session.

---

# 9.3 Test it

### Test 1 — New unverified account

Create another user:

```json
{
  "username": "testuser2",
  "email": "test@example.com",
  "password": "12345678"
}
```

Don't verify the email.

Then:

```http
POST /api/auth/login
```

```json
{
  "email": "test@example.com",
  "password": "12345678"
}
```

Expected:

```json
{
  "success": false,
  "message": "Please verify your email before logging in"
}
```

There should be:

```text
❌ No session
❌ No access token
❌ No refresh-token cookie
```

---

### Test 2 — Verify the account

Use:

```http
POST /api/auth/verify-email
```

with the OTP you received.

After successful verification:

```text
isEmailVerified: true
```

A session should also be created by your current `/verify-email` implementation.

---

### Test 3 — Login after verification

Now:

```http
POST /api/auth/login
```

```json
{
  "email": "test@example.com",
  "password": "12345678"
}
```

Expected:

```json
{
  "message": "Login successful",
  "user": {
    "id": "...",
    "username": "testuser2",
    "email": "test@example.com"
  },
  "accessToken": "eyJ..."
}
```

And a new session should be created.

---

# Step 9 complete

Your authentication flow is now:

```text
                    REGISTER
                       │
                       ▼
                 Create User
                       │
                       ▼
          isEmailVerified = false
                       │
                       ▼
                  Generate OTP
                       │
                       ▼
                 Send OTP Email
                       │
                       ▼
                VERIFY EMAIL
                       │
                       ▼
          isEmailVerified = true
                       │
                       ▼
                 Create Session
                       │
                       ▼
              Issue JWT Tokens
                       │
                       ▼
                    LOGIN
                       │
              ┌────────┴────────┐
              ▼                 ▼
         Not verified        Verified
              │                 │
              ▼                 ▼
            Reject         Create Session
                              │
                              ▼
                           Issue JWT
```

### One important observation

Your `/verify-email` endpoint **already logs the user in** by creating a session and issuing tokens. Therefore, after successful verification, the user technically doesn't need to immediately call `/login`.

That's intentional in the flow we designed.

**Next step should be Step 10: handle the “resend verification OTP” flow**, including expiry and replacing the previous OTP.


---

# Step 10 — Resend Verification OTP

Now we handle an important real-world case:

> User registered, but the OTP expired or they didn't receive it.

We need:

```text
POST /resend-verification
          ↓
Find user
          ↓
Already verified?
    ├── YES → reject
    └── NO
          ↓
Generate new OTP
          ↓
Hash OTP
          ↓
Delete old OTP
          ↓
Store new OTP
          ↓
Send email
```

## 10.1 Add `resendVerificationEmail()`

Open:

```text
src/controllers/auth.controller.js
```

Add this function below `verifyEmail()`:

```js
const resendVerificationEmail = async (req, res, next) => {
  try {
    const { email } = req.body;

    // 1. Find user
    const user = await userModel.findOne({ email });

    if (!user) {
      throw new ApiError(404, "User not found");
    }

    // 2. Check if email is already verified
    if (user.isEmailVerified) {
      throw new ApiError(400, "Email is already verified");
    }

    // 3. Generate new OTP
    const otp = generateOTP();

    // 4. Hash OTP
    const hashedOtp = await bcrypt.hash(otp, 10);

    // 5. Delete previous OTP
    await otpModel.deleteMany({
      email,
      purpose: "email_verification",
    });

    // 6. Store new OTP
    await otpModel.create({
      email,
      otpHash: hashedOtp,
      purpose: "email_verification",
      expiresAt: new Date(Date.now() + 5 * 60 * 1000),
    });

    // 7. Send new OTP
    await sendEmail(
      email,
      "Verify your email",
      `Your new email verification OTP is ${otp}. It will expire in 5 minutes.`,
      `
        <h2>Verify your email</h2>
        <p>Your new email verification OTP is:</p>
        <h1>${otp}</h1>
        <p>This OTP will expire in 5 minutes.</p>
      `,
    );

    return res.status(200).json({
      message: "Verification OTP sent successfully",
    });
  } catch (error) {
    next(error);
  }
};
```

---

# 10.2 Export the controller

At the bottom:

```js
export {
  register,
  verifyEmail,
  resendVerificationEmail,
  login,
  getMe,
  refreshTokens,
  logout,
  logoutAll,
  getSessions,
};
```

---

# 10.3 Add the route

Open:

```text
src/routes/auth.routes.js
```

Add `resendVerificationEmail` to the imports:

```js
import {
  register,
  verifyEmail,
  resendVerificationEmail,
  login,
  getMe,
  refreshTokens,
  logout,
  logoutAll,
  getSessions,
} from "../controllers/auth.controller.js";
```

Then add:

```js
router.post(
  "/resend-verification",
  resendVerificationEmail,
);
```

Your public authentication routes should now include:

```js
router.post("/register", register);

router.post("/verify-email", verifyEmail);

router.post(
  "/resend-verification",
  resendVerificationEmail,
);

router.post("/login", login);
```

---

# 10.4 Test it

Register a new account:

```http
POST /api/auth/register
```

Then don't use the OTP.

Request a new one:

```http
POST /api/auth/resend-verification
```

Body:

```json
{
  "email": "your-email@gmail.com"
}
```

Expected:

```json
{
  "message": "Verification OTP sent successfully"
}
```

You should receive a **new OTP** in your email.

---

## 10.5 Check MongoDB

Before resend:

```text
OTP A
expiresAt: ...
attempts: 0
```

After resend:

```text
OTP A → deleted

OTP B
expiresAt: new expiry
attempts: 0
```

This is important because the old OTP should no longer work.

---

# 10.6 Test that old OTP doesn't work

Suppose:

```text
Old OTP = 123456
New OTP = 789012
```

Try:

```json
{
  "email": "your-email@gmail.com",
  "otp": "123456"
}
```

It should return:

```text
Invalid OTP
```

Then use:

```json
{
  "email": "your-email@gmail.com",
  "otp": "789012"
}
```

It should successfully verify the account.

---

# ⚠️ One security issue we haven't solved yet

Right now someone could repeatedly call:

```text
POST /resend-verification
```

and generate hundreds of emails.

That's an **OTP resend abuse/rate-limiting** problem.

For this learning project, we'll keep the basic implementation first. Later, we can add something like:

```text
Maximum 1 resend / 60 seconds
Maximum X resends / hour
```

Don't implement that yet.

---

## Current authentication flow

You now have:

```text
REGISTER
   │
   ├── Create user
   ├── isEmailVerified = false
   ├── Generate OTP
   ├── Hash OTP
   ├── Store OTP
   └── Send email
            │
            ▼
      VERIFY EMAIL
            │
       ┌────┴────┐
       │         │
    Invalid     Valid
       │         │
       ▼         ▼
    attempts   Verify user
                  │
                  ▼
             Create session
                  │
                  ▼
               JWTs


RESEND VERIFICATION
        │
        ▼
Generate new OTP
        │
        ▼
Delete old OTP
        │
        ▼
Store new OTP
        │
        ▼
Send email
```

### Step 10 checkpoint

Test these three cases:

- ✅ Resend OTP successfully
    
- ✅ Old OTP becomes invalid
    
- ✅ New OTP verifies successfully
    

Once that works, the next step is **Step 11 — validation + security hardening for the email-verification endpoints**.


---


## Step 11 — Add Validation to Email Verification

Your email verification flow is working now. Before adding more features, let's make the two new endpoints reject bad input properly.

We need to validate:

### `/verify-email`

Required:

- `email` → valid email
    
- `otp` → exactly **6 digits**
    

### `/resend-verification`

Required:

- `email` → valid email
    

---

### 11.1 Install `express-validator`

If you don't already have it in this project:

```bash
npm install express-validator
```

---

### 11.2 Create validator file

Create:

```text
src/validators/
└── auth.validator.js
```

Add:

```js
import { body } from "express-validator";

const verifyEmailValidator = [
  body("email")
    .trim()
    .isEmail()
    .withMessage("Please provide a valid email"),

  body("otp")
    .trim()
    .matches(/^\d{6}$/)
    .withMessage("OTP must be exactly 6 digits"),
];

const resendVerificationOtpValidator = [
  body("email")
    .trim()
    .isEmail()
    .withMessage("Please provide a valid email"),
];

export {
  verifyEmailValidator,
  resendVerificationOtpValidator,
};
```

### 11.3 Create validation middleware

Create:

```text
src/middlewares/validation.middleware.js
```

```js
import { validationResult } from "express-validator";

const validate = (req, res, next) => {
  const errors = validationResult(req);

  if (!errors.isEmpty()) {
    return res.status(400).json({
      success: false,
      errors: errors.array(),
    });
  }

  next();
};

export default validate;
```

The flow becomes:

```text
Request
   ↓
Validator
   ↓
Validation Middleware
   ↓
Controller
```

So the controller doesn't have to worry about things like:

```text
otp = "hello"
otp = "123"
otp = "1234567"
email = "abc"
```

---

## 11.4 Update `auth.routes.js`

Import:

```js

import {
  register,
  login,
  getMe,
  refreshTokens,
  logout,
  logoutAll,
  getSessions,
  verifyEmail,
  resendVerificationOtp,
} from "../controllers/auth.controller.js";

import {
  verifyEmailValidator,
  resendVerificationOtpValidator,
} from "../validators/auth.validator.js";

import validate from "../middlewares/validation.middleware.js";
```

Then:

```js
router.post(
  "/verify-email",
  verifyEmailValidator,
  validate,
  verifyEmail,
);

router.post(
  "/resend-verification",
  resendVerificationOtpValidator,
  validate,
  resendVerificationOtp,
);
```

---

## 11.5 Test it

### Invalid OTP

```http
POST /api/auth/verify-email
```

```json
{
  "email": "asyncpranav@gmail.com",
  "otp": "123"
}
```

You should get a `400`.

---

### Invalid email

```json
{
  "email": "hello",
  "otp": "123456"
}
```

Again → `400`.

---

### Correct request

```json
{
  "email": "asyncpranav@gmail.com",
  "otp": "123456"
}
```

Then the request reaches `verifyEmail()`.

---

### Important

Your controller can now assume:

```js
email → valid email
otp → exactly 6 digits
```

But **do not remove the other checks from `verifyEmail()`**:

```js
if (!otpRecord)
if (otpRecord.attempts >= 3)
if (otpRecord.expiresAt < new Date())
bcrypt.compare(...)
```

Those are business/security checks, not input validation.

### ✅ Checkpoint

At this point you have:

```text
/register
    ↓
Create unverified user
    ↓
Generate + hash OTP
    ↓
Send OTP
    ↓
/verify-email
    ↓
Validate input
    ↓
Check OTP
    ↓
Mark email verified
    ↓
Create session
    ↓
Access + Refresh token


/resend-verification
    ↓
Validate email
    ↓
Generate new OTP
    ↓
Delete old OTP
    ↓
Store new hashed OTP
    ↓
Send email
```

**Stop here and implement Step 11.** After you test both endpoints successfully, the next step should be **OTP resend rate limiting/cooldown**, because right now someone could repeatedly hit `/resend-verification` and send unlimited emails.


---


Absolutely. Let's do **Step 12 — OTP Resend Cooldown from scratch**, using your existing code and keeping it simple.

# Step 12 — OTP Resend Cooldown

### Why are we adding this?

Currently, someone can call:

```text
POST /resend-verification
```

again and again:

```text
Request 1 → OTP sent
Request 2 → OTP sent
Request 3 → OTP sent
Request 4 → OTP sent
...
```

That's bad because it can:

- Spam the user's email
    
- Waste email-service resources
    
- Be abused by attackers
    

We'll add a **60-second cooldown**.

So:

```text
Request OTP
     ↓
OTP sent
     ↓
Wait 60 seconds
     ↓
Request again
     ↓
New OTP sent
```

---

# 12.1 We already have what we need

Your `Otp` model already has:

```js
timestamps: true,
```

Therefore MongoDB automatically gives each OTP:

```js
createdAt
updatedAt
```

So every OTP document already has the time at which it was created.

For example:

```json
{
  "email": "abc@gmail.com",
  "otpHash": "...",
  "createdAt": "2026-09-03T13:20:00.000Z"
}
```

Therefore, **we don't need to add another field or create another model.** We'll use `createdAt` to determine when the last OTP was created.


---

# 12.2 Modify `resendVerificationOtp()`

Your current function is approximately:

```js
const resendVerificationOtp = async (req, res, next) => {
  try {
    const { email } = req.body;

    const user = await userModel.findOne({ email });

    if (!user) {
      throw new ApiError(404, "User not found");
    }

    if (user.isEmailVerified) {
      throw new ApiError(400, "Email is already verified");
    }

    const otp = generateOTP();
    const hashedOtp = await bcrypt.hash(otp, 10);

    await otpModel.deleteMany({
      email,
      purpose: "email_verification",
    });

    await otpModel.create({
      email,
      otpHash: hashedOtp,
      purpose: "email_verification",
      expiresAt: new Date(Date.now() + 10 * 60 * 1000),
    });

    await sendEmail(
      email,
      "Verify your email",
      `Your new email verification OTP is ${otp}. It will expire in 10 minutes.`,
      `
        <h2>Verify your email</h2>
        <p>Your new email verification OTP is:</p>
        <h1>${otp}</h1>
        <p>This OTP will expire in 10 minutes.</p>
      `,
    );

    return res.status(200).json({
      message: "Verification OTP sent successfully",
    });
  } catch (error) {
    next(error);
  }
};
```

We are going to add the cooldown **before deleting the old OTP**.

---

# 12.3 Find the existing OTP

After:

```js
if (user.isEmailVerified) {
  throw new ApiError(400, "Email is already verified");
}
```

add:

```js
const existingOtp = await otpModel.findOne({
  email,
  purpose: "email_verification",
});
```

Now we know whether an OTP already exists.

---

# 12.4 Check the 60-seconds cooldown

Add:

```js
if (existingOtp) {
  const cooldown = 60 * 1000;

  const timePassed =
    Date.now() - existingOtp.createdAt.getTime();

  if (timePassed < cooldown) {
    throw new ApiError(
      429,
      "Please wait before requesting another OTP",
    );
  }
}
```

### Understand this carefully

```js
const cooldown = 60 * 1000;
```

means:

```text
60 seconds × 1000 milliseconds
= 60,000 milliseconds
```

Then:

```js
Date.now()
```

gives the current time in milliseconds.

And:

```js
existingOtp.createdAt.getTime()
```

gives the time when the OTP was created in milliseconds.

So:

```js
Date.now() - existingOtp.createdAt.getTime()
```

means:

> How much time has passed since the OTP was created?

Then:

```js
if (timePassed < cooldown)
```

means:

> If less than 60 seconds have passed, reject the request.


#### Why HTTP status 429
We use:

```js
429
```

because HTTP `429 Too Many Requests` is designed for situations where a client is making requests too frequently.

So:

```js
throw new ApiError(
  429,
  "Please wait before requesting another OTP",
);
```

is more appropriate than using `400`.

---

# 2.6 Handling an Expired OTP

The cooldown and OTP expiry are **two different concepts**.

```
OTP lifetime       = 10 minutes
Resend cooldown    = 60 seconds
```

When an existing OTP is found, we should first check whether it has expired.

```js
if (existingOtp) {
  const now = new Date();

  if (existingOtp.expiresAt <= now) {
    await otpModel.deleteOne({
      _id: existingOtp._id,
    });
  } else {
    // OTP is still valid → check 60-second cooldown
  }
}
```

### Behavior

#### OTP expired

```
Existing OTP
     ↓
Expired
     ↓
Delete old OTP
     ↓
Generate new OTP
```

#### OTP still valid + cooldown not completed

```
Existing OTP
     ↓
Still valid
     ↓
Less than 60 seconds passed
     ↓
429
```

#### OTP still valid + cooldown completed

```
Existing OTP
     ↓
Still valid
     ↓
60+ seconds passed
     ↓
Delete old OTP
     ↓
Generate new OTP
```

---

# 12.5 Complete function

Replace your current `resendVerificationOtp()` with this:

```js
const resendVerificationOtp = async (req, res, next) => {
  try {
    const { email } = req.body;

    // 1. Find user
    const user = await userModel.findOne({ email });

    if (!user) {
      throw new ApiError(404, "User not found");
    }

    // 2. Check if email is already verified
    if (user.isEmailVerified) {
      throw new ApiError(400, "Email is already verified");
    }

    // 3. Find existing OTP
    const existingOtp = await otpModel.findOne({
      email,
      purpose: "email_verification",
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
            `Please wait ${timeLeft} seconds before requesting a new verification OTP.`,
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
      purpose: "email_verification",
      expiresAt: new Date(Date.now() + 10 * 60 * 1000),
    });

    // 8. Send new OTP
    await sendEmail(
      email,
      "Verify your email",
      `Your new email verification OTP is ${otp}. It will expire in 10 minutes.`,
      `
        <h2>Verify your email</h2>
        <p>Your new email verification OTP is:</p>
        <h1>${otp}</h1>
        <p>This OTP will expire in 10 minutes.</p>
      `,
    );

    // 9. Success response
    return res.status(200).json({
      message: "Verification OTP sent successfully",
    });
  } catch (error) {
    next(error);
  }
};
```

### Why `deleteOne()` instead of `deleteMany()`?

We intentionally removed:

```js
await otpModel.deleteMany({
  email,
  purpose: "email_verification",
});
```

because our application is designed to maintain **one active OTP per email and purpose**.

We already have the exact OTP document:

```js
existingOtp._id
```

so:

```js
await otpModel.deleteOne({
  _id: existingOtp._id,
});
```

is sufficient and more precise.

`deleteMany()` would only be useful as defensive cleanup if multiple OTP records somehow existed.

---

### Final flow

```
POST /resend-verification
          ↓
     Find user
          ↓
   Email verified?
      ↙       ↘
    Yes        No
     ↓          ↓
   Error    Find OTP
               ↓
          OTP exists?
          ↙         ↘
        No           Yes
        ↓             ↓
    Generate     OTP expired?
    new OTP       ↙       ↘
                Yes        No
                 ↓          ↓
              Delete    Check cooldown
                OTP       ↙       ↘
                         <60s     ≥60s
                           ↓        ↓
                          429    Delete OTP
                                    ↓
                              Generate OTP
                                    ↓
                               Hash OTP
                                    ↓
                               Store OTP
                                    ↓
                               Send Email

```


---

# 12.6 Test in Postman

Use:

```http
POST /api/auth/resend-verification
```

Body:

```json
{
  "email": "your-email@gmail.com"
}
```

### First request

You should get:

```json
{
  "message": "Verification OTP sent successfully"
}
```

And receive the email.

---

### Immediately send the same request again

You should now get:

```json
{
  "success": false,
  "message": "Please wait before requesting another OTP"
}
```

with HTTP status:

```text
429 Too Many Requests
```

---

### Wait 60+ seconds

Send the request again.

It should work:

```json
{
  "message": "Verification OTP sent successfully"
}
```

And you'll receive a new OTP.

---

# Important detail

We check the cooldown **before**:

```js
await otpModel.deleteMany(...)
```

This is important.

If we deleted the OTP first, we'd lose the `createdAt` information needed to determine whether 60 seconds have passed.

The correct order is:

```text
Find existing OTP
       ↓
Check createdAt
       ↓
Is 60 seconds over?
    ↙       ↘
   NO       YES
   ↓         ↓
429       Generate OTP
             ↓
        Delete old OTP
             ↓
        Create new OTP
             ↓
        Send email
```

## ✅ Step 12 checkpoint

After this step your OTP system has:

- ✅ Random 6-digit OTP
    
- ✅ Hashed OTP storage
    
- ✅ 10-minute OTP expiry
    
- ✅ Maximum 3 verification attempts
    
- ✅ Old OTP replaced on resend
    
- ✅ OTP deleted after successful verification
    
- ✅ **60-second resend cooldown**
    
- ✅ Input validation
    

**Implement and test this step only.** Once it works, we'll move to the next security improvement.