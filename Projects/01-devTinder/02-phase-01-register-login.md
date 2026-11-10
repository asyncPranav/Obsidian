
----

Yes. Now we build the **Login Controller** using the exact same session-based JWT architecture as your register controller.

The key difference is that during login we must fetch the password explicitly because your User schema has:

```js
select: false
```

### `auth.controller.js`

Replace the empty `login` function with:

```js
const login = async (req, res, next) => {
  try {
    const { email, password } = req.body;

    // 1. Find user and explicitly include password
    const user = await userModel
      .findOne({ email })
      .select("+password");

    if (!user) {
      throw new ApiError(401, "Invalid email or password");
    }

    // 2. Compare entered password with hashed password
    const isPasswordValid = await bcrypt.compare(
      password,
      user.password,
    );

    if (!isPasswordValid) {
      throw new ApiError(401, "Invalid email or password");
    }

    // 3. Generate session ID
    const sessionId = new mongoose.Types.ObjectId();

    // 4. Generate refresh token
    const refreshToken = generateRefreshToken(
      user._id,
      sessionId,
    );

    // 5. Hash refresh token before storing it
    const hashedRefreshToken = await bcrypt.hash(
      refreshToken,
      10,
    );

    // 6. Create session
    await sessionModel.create({
      _id: sessionId,
      user: user._id,
      refreshToken: hashedRefreshToken,
      ip: req.ip,
      userAgent: req.get("User-Agent"),
      expiresAt: new Date(
        Date.now() + 15 * 24 * 60 * 60 * 1000,
      ),
    });

    // 7. Set refresh token in HTTP-only cookie
    setRefreshTokenCookie(res, refreshToken);

    // 8. Generate access token
    const accessToken = generateAccessToken(
      user._id,
      sessionId,
    );

    // 9. Return safe user data + access token
    return res.status(200).json({
      status: "success",
      message: "Login successful",
      data: {
        user: {
          id: user._id,
          firstName: user.firstName,
          lastName: user.lastName,
          email: user.email,
          age: user.age,
          gender: user.gender,
          about: user.about,
          skills: user.skills,
          photoUrl: user.photoUrl,
          isPremium: user.isPremium,
          membershipType: user.membershipType,
        },
        accessToken,
      },
    });
  } catch (error) {
    next(error);
  }
};
```

Your complete controller now becomes:

```js
import mongoose from "mongoose";
import bcrypt from "bcrypt";

import userModel from "../models/user.model.js";
import sessionModel from "../models/session.model.js";

import ApiError from "../utils/ApiError.util.js";
import {
  generateAccessToken,
  generateRefreshToken,
} from "../utils/jwt.util.js";
import { setRefreshTokenCookie } from "../utils/cookie.util.js";

const register = async (req, res, next) => {
  // your existing register controller
};

const login = async (req, res, next) => {
  try {
    const { email, password } = req.body;

    // 1. Find user and explicitly include password
    const user = await userModel
      .findOne({ email })
      .select("+password");

    if (!user) {
      throw new ApiError(401, "Invalid email or password");
    }

    // 2. Compare password
    const isPasswordValid = await bcrypt.compare(
      password,
      user.password,
    );

    if (!isPasswordValid) {
      throw new ApiError(401, "Invalid email or password");
    }

    // 3. Generate session ID
    const sessionId = new mongoose.Types.ObjectId();

    // 4. Generate refresh token
    const refreshToken = generateRefreshToken(
      user._id,
      sessionId,
    );

    // 5. Hash refresh token
    const hashedRefreshToken = await bcrypt.hash(
      refreshToken,
      10,
    );

    // 6. Create session
    await sessionModel.create({
      _id: sessionId,
      user: user._id,
      refreshToken: hashedRefreshToken,
      ip: req.ip,
      userAgent: req.get("User-Agent"),
      expiresAt: new Date(
        Date.now() + 15 * 24 * 60 * 60 * 1000,
      ),
    });

    // 7. Set refresh token cookie
    setRefreshTokenCookie(res, refreshToken);

    // 8. Generate access token
    const accessToken = generateAccessToken(
      user._id,
      sessionId,
    );

    // 9. Return response
    return res.status(200).json({
      status: "success",
      message: "Login successful",
      data: {
        user: {
          id: user._id,
          firstName: user.firstName,
          lastName: user.lastName,
          email: user.email,
          age: user.age,
          gender: user.gender,
          about: user.about,
          skills: user.skills,
          photoUrl: user.photoUrl,
          isPremium: user.isPremium,
          membershipType: user.membershipType,
        },
        accessToken,
      },
    });
  } catch (error) {
    next(error);
  }
};

export { register, login };
```

### Understand the important parts

#### 1. Why `.select("+password")`?

Your schema has:

```js
password: {
  type: String,
  required: true,
  select: false,
}
```

So:

```js
await userModel.findOne({ email });
```

does **not** return the password.

For login, we need the hash to perform:

```js
bcrypt.compare(password, user.password);
```

Therefore:

```js
.select("+password")
```

temporarily includes it.

---

#### 2. Why same error for email and password?

We use:

```js
throw new ApiError(401, "Invalid email or password");
```

for both:

```text
Email doesn't exist ──────┐
                          ├──→ same response
Wrong password ───────────┘
```

This avoids unnecessarily revealing whether an email is registered.

---

#### 3. Why create a NEW session on every login?

Because one user can have multiple devices:

```text
                 User
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
     Laptop     Phone     Tablet
     Session A  Session B  Session C
```

This gives us the ability to later:

- logout one device
    
- logout all devices
    
- revoke a compromised session
    
- track session expiry
    

That's exactly why we're using `sid` in the JWT.

---

### One thing to fix before testing

Make sure your `app.js` has `cookie-parser`, because later the refresh-token cookie will need to be read:

```js
import cookieParser from "cookie-parser";

app.use(express.json());
app.use(cookieParser());
```

And **do not** return:

```js
refreshToken
```

in the JSON response.

Your architecture is:

```text
Access Token
    ↓
Response JSON
    ↓
Client memory

Refresh Token
    ↓
httpOnly cookie
    ↓
Browser automatically sends it
```

That's the design we want.

**Next step after this:** create `auth.routes.js`, connect both `register` and `login` validators, mount `/api/auth`, and test the complete Register → Login flow.



----


Good. Now we connect the **validators → controller → routes → app** and test the complete Register/Login flow.

## 1. Create `auth.routes.js`

Create:

```text
src/routes/auth.routes.js
```

```js
import express from "express";

import {
  register,
  login,
} from "../controllers/auth.controller.js";

import {
  registerValidator,
  loginValidator,
} from "../validators/auth.validator.js";

import validate from "../middlewares/validate.middleware.js";

const router = express.Router();

router.post(
  "/register",
  registerValidator,
  validate,
  register,
);

router.post(
  "/login",
  loginValidator,
  validate,
  login,
);

export default router;
```

### Flow

```text
POST /api/auth/register
        ↓
registerValidator
        ↓
validate
        ↓
register controller
        ↓
User + Session + JWT
```

And:

```text
POST /api/auth/login
        ↓
loginValidator
        ↓
validate
        ↓
login controller
        ↓
Session + JWT
```

---

# 2. Update `app.js`

Your current `app.js` should now become:

```js
import express from "express";
import cookieParser from "cookie-parser";

import authRouter from "./routes/auth.routes.js";
import notFound from "./middlewares/notFound.middleware.js";
import errorHandler from "./middlewares/errorHandler.middleware.js";

const app = express();

app.use(express.json());
app.use(cookieParser());

app.get("/health", (req, res) => {
  res.status(200).json({
    status: "OK",
    message: "DevTinder API is running",
  });
});

app.use("/api/auth", authRouter);

app.use(notFound);
app.use(errorHandler);

export default app;
```

**Important:** your actual middleware filenames may differ.

For example, if your files are:

```text
middlewares/
├── errorHandler.js
├── notFound.js
└── validate.js
```

then use those exact imports.

Don't rename files just because my example uses different names.

---

# 3. Make sure `cookie-parser` is installed

If you haven't installed it:

```bash
npm install cookie-parser
```

Then:

```js
app.use(cookieParser());
```

This gives us:

```js
req.cookies
```

Later the refresh endpoint will use:

```js
req.cookies.refreshToken
```

---

# 4. Verify your cookie utility

Your:

```text
src/utils/cookie.util.js
```

should look roughly like:

```js
const refreshTokenCookieOptions = {
  httpOnly: true,
  secure: process.env.NODE_ENV === "production",
  sameSite: "lax",
  path: "/",
  maxAge: 15 * 24 * 60 * 60 * 1000,
};

const setRefreshTokenCookie = (res, refreshToken) => {
  res.cookie(
    "refreshToken",
    refreshToken,
    refreshTokenCookieOptions,
  );
};

const clearRefreshTokenCookie = (res) => {
  res.clearCookie(
    "refreshToken",
    refreshTokenCookieOptions,
  );
};

export {
  setRefreshTokenCookie,
  clearRefreshTokenCookie,
};
```

### Why `path: "/"`?

We want the browser to send the refresh cookie to the API routes where we'll need it.

Later:

```text
POST /api/auth/refresh
       ↑
refreshToken cookie available

POST /api/auth/logout
       ↑
refreshToken cookie available
```

For this project, `/` keeps things simple.

---

# 5. Check your `config.js`

Your JWT configuration should currently be:

```js
const requiredEnv = [
  "MONGO_URI",
  "PORT",
  "JWT_ACCESS_SECRET",
  "JWT_REFRESH_SECRET",
];
```

and:

```js
const config = {
  mongoUri: process.env.MONGO_URI,
  port: process.env.PORT,
  jwtAccessSecret: process.env.JWT_ACCESS_SECRET,
  jwtRefreshSecret: process.env.JWT_REFRESH_SECRET,
};
```

Therefore your JWT utility must use:

```js
config.jwtAccessSecret
```

and:

```js
config.jwtRefreshSecret
```

**not**

```js
config.jwtSecret
config.refreshSecret
```

That mismatch was present in your earlier version.

---

# 6. Test Register

Use Postman/Thunder Client.

### Request

```text
POST http://localhost:3000/api/auth/register
```

### Body → JSON

```json
{
  "firstName": "Pranav",
  "lastName": "Singh",
  "email": "pranav@example.com",
  "password": "Strong@123",
  "confirmPassword": "Strong@123",
  "age": 21,
  "gender": "male",
  "about": "Backend developer",
  "skills": ["Node.js", "Express.js", "MongoDB"],
  "photoUrl": "https://example.com/profile.jpg"
}
```

You should receive something like:

```json
{
  "status": "success",
  "message": "User registered successfully",
  "data": {
    "user": {
      "id": "...",
      "firstName": "Pranav",
      "lastName": "Singh",
      "email": "pranav@example.com",
      "age": 21,
      "gender": "male",
      "about": "Backend developer",
      "skills": [
        "Node.js",
        "Express.js",
        "MongoDB"
      ],
      "photoUrl": "https://example.com/profile.jpg",
      "isPremium": false,
      "membershipType": "free"
    },
    "accessToken": "eyJ..."
  }
}
```

And importantly, the response should also contain a:

```text
Set-Cookie
```

header containing the refresh token.

---

# 7. Check MongoDB

You should now have **two documents**.

### `users`

```text
User
├── firstName
├── lastName
├── email
├── password → bcrypt hash
├── age
├── gender
├── ...
└── membershipType
```

### `sessions`

```text
Session
├── _id → sessionId
├── user → User._id
├── refreshToken → bcrypt hash
├── ip
├── userAgent
├── revoked → false
├── expiresAt
├── createdAt
└── updatedAt
```

Notice:

```text
User.password
      ↓
bcrypt hash

Session.refreshToken
      ↓
bcrypt hash
```

Neither plaintext secret should be stored in MongoDB.

---

# 8. Test Login

Now use another request:

```text
POST http://localhost:3000/api/auth/login
```

Body:

```json
{
  "email": "pranav@example.com",
  "password": "Strong@123"
}
```

Expected:

```json
{
  "status": "success",
  "message": "Login successful",
  "data": {
    "user": {
      "id": "...",
      "firstName": "Pranav",
      "lastName": "Singh",
      "email": "pranav@example.com",
      "age": 21,
      "gender": "male",
      "about": "Backend developer",
      "skills": [
        "Node.js",
        "Express.js",
        "MongoDB"
      ],
      "photoUrl": "https://example.com/profile.jpg",
      "isPremium": false,
      "membershipType": "free"
    },
    "accessToken": "eyJ..."
  }
}
```

And another session should be created.

So after:

```text
Register
```

you have:

```text
Session A
```

After:

```text
Login
```

you get:

```text
Session A
Session B
```

That's intentional.

---

# 9. Test the important failure cases

Don't just test the happy path.

### Wrong password

```json
{
  "email": "pranav@example.com",
  "password": "Wrong@123"
}
```

Expected:

```http
401 Unauthorized
```

```json
{
  "status": "fail",
  "statusCode": 401,
  "message": "Invalid email or password",
  "errors": []
}
```

### Non-existing email

```json
{
  "email": "doesnotexist@example.com",
  "password": "Strong@123"
}
```

Should produce the **same**:

```text
401
Invalid email or password
```

This is intentional.

### Weak password during registration

```json
{
  "email": "test@example.com",
  "password": "123"
}
```

Should be rejected by:

```text
registerValidator
       ↓
validate middleware
```

The controller shouldn't even execute.

### Duplicate email

Register the same email again.

Expected:

```http
409 Conflict
```

---

# One important thing before we move on

Your current register controller has this sequence:

```text
Create User
    ↓
Create Session
```

If session creation fails:

```text
User created ✅
Session failed ❌
```

you'd have a user without a session.

For our current project, **don't introduce MongoDB transactions yet**. The architecture is already good, and we can revisit atomicity if/when it becomes necessary.

---

## Phase 1 status now

```text
Phase 1 — JWT Authentication

User Schema                  ✅
Register Validator           ✅
Login Validator              ✅
ApiError                     ✅
Validation Middleware        ✅
Error Handler                ✅
Session Model                ✅
JWT Utility                  ✅
Cookie Utility               ✅
Register Controller           ✅
Login Controller              ✅
Auth Routes                  ✅
Route Mounting               ✅
Register Testing             ⏳
Login Testing                ⏳
Authenticate Middleware      ⏭️
Refresh Token Endpoint       ⏭️
Logout                       ⏭️
```

**Next major step is `authenticate.middleware.js`.**

That middleware will take:

```text
Authorization: Bearer <accessToken>
```

→ verify the access JWT → extract `sub` and `sid` → attach authenticated user/session information to `req` → allow protected routes to continue.