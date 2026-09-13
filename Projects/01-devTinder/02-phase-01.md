
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
