
---


# Session-Based Authentication in Express.js — Complete Beginner Notes

---

## 1. What Is Authentication? (And Why It's Different From Authorization)

Before writing a single line of code, let's get two words right that developers confuse constantly.

**Authentication** answers: _"Who are you?"_ It is the process of verifying that a user is who they claim to be — typically by checking a username and password.

**Authorization** answers: _"What are you allowed to do?"_ It is the process of checking whether an authenticated user has permission to perform a specific action or access a specific resource.

```
Authentication:  "I am Pranav, here's my password" → Server verifies → "Yes, that's Pranav"
Authorization:   "Can Pranav access /admin?"        → Server checks role → "No, Pranav is not admin"
```

You cannot have authorization without authentication — you need to know _who_ someone is before you decide _what_ they can do.

---

## 2. What Is Session-Based Authentication?

Session-based authentication is the classic, battle-tested approach to keeping a user "logged in" across multiple HTTP requests. Since HTTP is stateless (every request is anonymous by default), we need a mechanism to remember the user between requests.

The mechanism works like this: when a user successfully logs in, the server creates a **session** — a small storage space on the server — and sends the browser a **session ID cookie**. Every subsequent request from that browser automatically includes this cookie. The server reads the cookie, looks up the session, and knows exactly who is making the request.

```
                    ┌─── BROWSER ───────┐           ┌─── SERVER ──────────────────────┐
                    │                   │           │                                 │
  User submits ───► │ POST /login       │ ────────► │ 1. Find user in DB              │
  username +        │ {email, password} │           │ 2. Compare password with bcrypt │
  password          │                   │ ◄──────── │ 3. Create session in store      │
                    │ Receives cookie:  │           │ 4. Send session ID cookie       │
                    │ sid=abc123        │           │                                 │
                    │                   │           │                                 │
  User visits ────► │ GET /dashboard    │ ────────► │ 5. Read cookie → sid=abc123     │
  dashboard         │ Cookie: sid=abc123│           │ 6. Look up session in store     │
                    │                   │ ◄──────── │ 7. req.session = {userId: 42}   │
                    │ Gets their data   │           │ 8. Serve protected route        │
                    │                   │           │                                 │
  User logs ──────► │ POST /logout      │ ────────► │ 9. Destroy session in store     │
  out               │ Cookie: sid=abc123│           │ 10. Clear cookie in browser     │
                    │                   │           │                                 │
                    └───────────────────┘           └─────────────────────────────────┘
```

---

## 3. The Role of bcrypt — Why We Never Store Plain Text Passwords

This is the most important security concept in authentication. **You must never store a user's password in plain text in your database.**

Here's why: if your database is ever breached (hacked, leaked, accidentally exposed), the attacker gets every user's password immediately. Since most people reuse passwords, this compromises not just your app but the user's email, bank, and other accounts too. This is your legal and moral responsibility as a developer.

The solution is **password hashing**. A hash is a one-way mathematical function. You can turn a password into a hash, but you cannot turn the hash back into the password.

```
Password → bcrypt hash → stored in DB
"Secret@123" → "$2b$10$xK9m...8Lp" → stored

Login attempt:
User enters "Secret@123"
bcrypt takes "Secret@123" and the stored hash
bcrypt runs the same algorithm → result matches → ✓ authenticated

User enters "wrongpassword"
bcrypt runs algorithm → result does NOT match → ✗ rejected
```

**bcrypt** is the industry-standard hashing algorithm for passwords. It has two important properties that simpler algorithms like MD5 or SHA256 don't have for passwords:

It is **intentionally slow** — it is designed to take significant computation time, making brute-force attacks impractical. You configure this slowness using a **salt rounds** value (cost factor).

It generates a unique **salt** automatically — a random string added to the password before hashing. This means two users with the same password will have completely different hashes, defeating rainbow table attacks.

```
User A: password "hello" → salt "abc" → hash "$2b$10$abc...xyz"
User B: password "hello" → salt "def" → hash "$2b$10$def...mno"
Same password, completely different hashes.
```

```bash
npm install bcrypt
```

```js
const bcrypt = require("bcrypt");

// HASHING (when user registers — do this once before saving to DB)
const saltRounds = 10; // higher = slower but more secure. 10–12 is the standard.
const plainPassword = "Secret@123";
const hashedPassword = await bcrypt.hash(plainPassword, saltRounds);
// hashedPassword = "$2b$10$xK9mLp..." — store THIS in your database, never the plain password

// COMPARING (when user logs in)
const isMatch = await bcrypt.compare(plainPassword, hashedPassword);
// isMatch = true  → passwords match, log the user in
// isMatch = false → passwords don't match, reject
```

---

## 4. The Complete Authentication Flow — Theory First

Here is the complete flow of a session-based auth system. Reading this before touching the code will make every line of code make sense:

### Registration Flow

```
1. User submits registration form: { username, email, password }
2. Server validates the input (required fields, email format, password strength)
3. Server checks: does an account with this email already exist?
   → If yes: return error "Email already registered"
4. Server hashes the password with bcrypt
5. Server saves the new user to the database: { username, email, hashedPassword, role }
6. Server responds: "Account created! Please log in."
   (Do NOT automatically log them in here — keep registration and login separate)
```

### Login Flow

```
1. User submits: { email, password }
2. Server validates input
3. Server queries DB: find user where email = submitted email
   → If not found: return 401 "Invalid credentials"
   (Don't say "email not found" — that tells attackers which emails exist)
4. Server compares submitted password with stored hash using bcrypt.compare()
   → If no match: return 401 "Invalid credentials"
5. Server calls req.session.regenerate() → creates a fresh session ID
6. Server stores user info in session: { userId, username, role, isLoggedIn }
7. Session is saved to session store (MongoDB, Redis, etc.)
8. Browser receives Set-Cookie header with the session ID
9. Return success response
```

### Authenticated Request Flow

```
1. Browser sends request with session cookie automatically
2. express-session middleware reads cookie → extracts session ID
3. Middleware queries session store → loads session data
4. req.session is populated with the user's data
5. Auth middleware checks req.session.isLoggedIn
   → If false/missing: return 401
6. Route handler runs with full access to req.session
```

### Logout Flow

```
1. Route handler reads req.session.username (save before destroying)
2. req.session.destroy() → deletes session from session store
3. res.clearCookie() → tells browser to delete the cookie
4. Session ID is now invalid — even if someone had copied it
```

---

## 5. Project Setup

```bash
mkdir session-auth-app
cd session-auth-app
npm init -y
npm install express express-session connect-mongo mongoose bcrypt dotenv express-validator
```

```
project structure:
├── server.js
├── .env
├── config/
│   └── db.js
├── models/
│   └── User.js
├── middleware/
│   └── auth.js
├── routes/
│   ├── authRoutes.js
│   └── protectedRoutes.js
└── validators/
    └── authValidator.js
```

```
# .env
PORT=3000
MONGO_URI=mongodb://localhost:27017/session-auth
SESSION_SECRET=j3k9Lm2pQ8wZvRxN7cH6bT1sY4uAf5gDe0hB
NODE_ENV=development
```

---

## 6. MongoDB Connection

```js
// config/db.js

const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI);
    console.log("MongoDB connected successfully");
  } catch (error) {
    console.error("MongoDB connection failed:", error.message);
    process.exit(1); // stop the server if DB connection fails
  }
};

module.exports = connectDB;
```

---

## 7. The User Model

```js
// models/User.js

const mongoose = require("mongoose");
const bcrypt = require("bcrypt");

const userSchema = new mongoose.Schema(
  {
    username: {
      type: String,
      required: [true, "Username is required"],
      trim: true,
      minlength: [3, "Username must be at least 3 characters"],
      maxlength: [20, "Username cannot exceed 20 characters"],
    },
    email: {
      type: String,
      required: [true, "Email is required"],
      unique: true,       // no two users can have the same email
      lowercase: true,    // always store email in lowercase
      trim: true,
    },
    password: {
      type: String,
      required: [true, "Password is required"],
      minlength: [6, "Password must be at least 6 characters"],
    },
    role: {
      type: String,
      enum: ["user", "admin"],  // only these two values are allowed
      default: "user",
    },
  },
  {
    timestamps: true, // automatically adds createdAt and updatedAt fields
  }
);

// ─── Pre-save Hook: Hash Password Before Saving ──────────────────────────────
// This Mongoose "hook" runs automatically BEFORE every .save() call
// It ensures the password is ALWAYS hashed before it touches the database
userSchema.pre("save", async function (next) {
  // "this" refers to the document being saved

  // Only hash if the password field was actually modified
  // (This prevents re-hashing on other profile updates)
  if (!this.isModified("password")) {
    return next();
  }

  try {
    const salt = await bcrypt.genSalt(10); // generate a salt with 10 rounds
    this.password = await bcrypt.hash(this.password, salt); // replace plain with hash
    next();
  } catch (error) {
    next(error);
  }
});

// ─── Instance Method: Compare Passwords ─────────────────────────────────────
// This is a custom method we add to every User document
// We can call it as: user.comparePassword("submittedPassword")
userSchema.methods.comparePassword = async function (candidatePassword) {
  // bcrypt.compare handles everything — it extracts the salt from the stored hash
  // and re-runs the hash to see if they match
  return await bcrypt.compare(candidatePassword, this.password);
};

// ─── Prevent Password From Being Sent in Responses ──────────────────────────
// toJSON is called whenever the document is serialized to JSON (e.g., res.json())
// We delete the password field so it NEVER accidentally leaks into a response
userSchema.methods.toJSON = function () {
  const userObject = this.toObject();
  delete userObject.password;
  return userObject;
};

const User = mongoose.model("User", userSchema);
module.exports = User;
```

> 💡 **Why put bcrypt in the model?** The pre-save hook guarantees the password is ALWAYS hashed before saving, no matter which route or service calls `.save()`. You never have to remember to hash manually in your route handlers — the model handles it automatically. This is a much safer pattern.

---

## 8. Input Validators

```js
// validators/authValidator.js

const { body } = require("express-validator");

const registerValidator = [
  body("username")
    .trim()
    .notEmpty().withMessage("Username is required")
    .isLength({ min: 3, max: 20 }).withMessage("Username must be 3–20 characters")
    .isAlphanumeric().withMessage("Username can only contain letters and numbers"),

  body("email")
    .isEmail().withMessage("Please enter a valid email address")
    .normalizeEmail(),

  body("password")
    .isStrongPassword({
      minLength: 8,
      minLowercase: 1,
      minUppercase: 1,
      minNumbers: 1,
      minSymbols: 1,
    })
    .withMessage("Password must be 8+ characters with uppercase, lowercase, number, and symbol"),

  body("confirmPassword")
    .notEmpty().withMessage("Please confirm your password")
    .custom((value, { req }) => {
      // Compare confirmPassword with the password field
      if (value !== req.body.password) {
        throw new Error("Passwords do not match");
      }
      return true;
    }),
];

const loginValidator = [
  body("email")
    .isEmail().withMessage("Please enter a valid email")
    .normalizeEmail(),

  body("password")
    .notEmpty().withMessage("Password is required"),
];

module.exports = { registerValidator, loginValidator };
```

---

## 9. Authentication Middleware

```js
// middleware/auth.js

// ─── isAuthenticated ─────────────────────────────────────────────────────────
// Checks if the user has a valid session (is logged in)
// Apply this to any route that requires login
const isAuthenticated = (req, res, next) => {
  if (req.session && req.session.isLoggedIn && req.session.userId) {
    return next(); // user is logged in, proceed
  }
  return res.status(401).json({
    success: false,
    message: "Authentication required. Please log in.",
  });
};

// ─── isAdmin ─────────────────────────────────────────────────────────────────
// Checks if the authenticated user has the "admin" role
// ALWAYS use isAuthenticated BEFORE isAdmin — never use isAdmin alone
const isAdmin = (req, res, next) => {
  if (req.session && req.session.role === "admin") {
    return next();
  }
  return res.status(403).json({
    success: false,
    message: "Access denied. Administrator privileges required.",
  });
};

// ─── isGuest ─────────────────────────────────────────────────────────────────
// Prevents logged-in users from accessing login/register pages
// Useful for redirecting already-authenticated users away from auth pages
const isGuest = (req, res, next) => {
  if (req.session && req.session.isLoggedIn) {
    return res.status(400).json({
      success: false,
      message: "You are already logged in.",
    });
  }
  next();
};

module.exports = { isAuthenticated, isAdmin, isGuest };
```

---

## 10. Authentication Routes

```js
// routes/authRoutes.js

const express = require("express");
const router = express.Router();
const { validationResult } = require("express-validator");
const User = require("../models/User");
const { isAuthenticated, isGuest } = require("../middleware/auth");
const { registerValidator, loginValidator } = require("../validators/authValidator");

// ─── Helper: Handle Validation Errors ───────────────────────────────────────
const handleValidation = (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({
      success: false,
      message: "Validation failed",
      errors: errors.array().map(e => ({ field: e.path, message: e.msg })),
    });
  }
  return null; // no errors
};

// ─── REGISTER ────────────────────────────────────────────────────────────────
// POST /auth/register
// Public route — no auth required
// isGuest prevents already logged-in users from registering again
router.post("/register", isGuest, registerValidator, async (req, res) => {
  // Step 1: Check validation errors from express-validator
  const validationError = handleValidation(req, res);
  if (validationError) return; // response already sent

  const { username, email, password } = req.body;

  try {
    // Step 2: Check if email already exists in the database
    const existingUser = await User.findOne({ email });
    if (existingUser) {
      return res.status(409).json({
        success: false,
        message: "An account with this email already exists.",
      });
    }

    // Step 3: Create the user
    // The pre-save hook in the model automatically hashes the password
    // We just pass the plain password — the model handles the hashing
    const newUser = new User({ username, email, password });
    await newUser.save();
    // At this point, newUser.password in the database is the bcrypt hash

    // Step 4: Respond — toJSON() removes the password field automatically
    res.status(201).json({
      success: true,
      message: "Account created successfully! You can now log in.",
      user: newUser, // password is stripped by the toJSON() method we defined
    });

  } catch (error) {
    // Mongoose duplicate key error (safety net if findOne check has a race condition)
    if (error.code === 11000) {
      return res.status(409).json({
        success: false,
        message: "An account with this email already exists.",
      });
    }
    res.status(500).json({ success: false, message: "Server error during registration." });
  }
});

// ─── LOGIN ───────────────────────────────────────────────────────────────────
// POST /auth/login
// Public route
router.post("/login", isGuest, loginValidator, async (req, res) => {
  // Step 1: Validate input
  const validationError = handleValidation(req, res);
  if (validationError) return;

  const { email, password } = req.body;

  try {
    // Step 2: Find the user by email
    const user = await User.findOne({ email });

    // Step 3: If user not found OR password doesn't match, return the SAME error
    // Why the same error? If you say "email not found", attackers can enumerate
    // which emails are registered in your system. Always give a generic message.
    if (!user) {
      return res.status(401).json({
        success: false,
        message: "Invalid email or password.",
      });
    }

    // Step 4: Compare the submitted password with the stored bcrypt hash
    // This is the instance method we defined in the User model
    const isPasswordCorrect = await user.comparePassword(password);

    if (!isPasswordCorrect) {
      return res.status(401).json({
        success: false,
        message: "Invalid email or password.", // same message — don't reveal which part is wrong
      });
    }

    // Step 5: Regenerate session ID (prevents session fixation attacks)
    // This creates a brand new session ID, invalidating any previous one
    req.session.regenerate((err) => {
      if (err) {
        return res.status(500).json({ success: false, message: "Login failed. Please try again." });
      }

      // Step 6: Store essential user info in the session
      // Only store what you need — minimize the session footprint
      // NEVER store the password (even hashed) in the session
      req.session.isLoggedIn = true;
      req.session.userId = user._id.toString(); // convert ObjectId to string
      req.session.username = user.username;
      req.session.email = user.email;
      req.session.role = user.role;
      req.session.loginTime = new Date().toISOString();

      // Step 7: Explicitly save the session to the store
      // express-session usually does this automatically, but calling it explicitly
      // after regenerate ensures the data is persisted before we respond
      req.session.save((saveErr) => {
        if (saveErr) {
          return res.status(500).json({ success: false, message: "Session save failed." });
        }

        res.json({
          success: true,
          message: `Welcome back, ${user.username}!`,
          user: {
            id:       user._id,
            username: user.username,
            email:    user.email,
            role:     user.role,
          },
        });
      });
    });

  } catch (error) {
    res.status(500).json({ success: false, message: "Server error during login." });
  }
});

// ─── LOGOUT ──────────────────────────────────────────────────────────────────
// POST /auth/logout
// Protected route — must be logged in to log out
router.post("/logout", isAuthenticated, (req, res) => {
  const username = req.session.username; // save name before destroying session

  // Step 1: Destroy the session in the session store (MongoDB)
  req.session.destroy((err) => {
    if (err) {
      return res.status(500).json({ success: false, message: "Logout failed. Please try again." });
    }

    // Step 2: Clear the session cookie from the browser
    // The cookie name must match the "name" option you set in session config
    res.clearCookie("sessionId");

    res.json({
      success: true,
      message: `Goodbye, ${username}! You have been logged out successfully.`,
    });
  });
});

// ─── GET CURRENT USER ────────────────────────────────────────────────────────
// GET /auth/me
// Returns the currently logged-in user's info from the session
// Useful for frontend apps to check auth status on page load
router.get("/me", isAuthenticated, async (req, res) => {
  try {
    // Fetch fresh user data from DB (session might have stale info)
    const user = await User.findById(req.session.userId);

    if (!user) {
      // User was deleted from DB but session still exists
      req.session.destroy(() => {});
      return res.status(404).json({ success: false, message: "User not found." });
    }

    res.json({
      success: true,
      user: {
        id:        user._id,
        username:  user.username,
        email:     user.email,
        role:      user.role,
        createdAt: user.createdAt,
      },
    });
  } catch (error) {
    res.status(500).json({ success: false, message: "Server error." });
  }
});

module.exports = router;
```

---

## 11. Protected Routes

```js
// routes/protectedRoutes.js

const express = require("express");
const router = express.Router();
const User = require("../models/User");
const { isAuthenticated, isAdmin } = require("../middleware/auth");

// ─── DASHBOARD ────────────────────────────────────────────────────────────────
// GET /api/dashboard
// Any logged-in user can access this
router.get("/dashboard", isAuthenticated, (req, res) => {
  res.json({
    success: true,
    message: `Welcome to your dashboard, ${req.session.username}!`,
    sessionData: {
      userId:    req.session.userId,
      role:      req.session.role,
      loginTime: req.session.loginTime,
    },
  });
});

// ─── PROFILE ─────────────────────────────────────────────────────────────────
// GET /api/profile
// Returns full profile from database (more complete than session data)
router.get("/profile", isAuthenticated, async (req, res) => {
  try {
    const user = await User.findById(req.session.userId);
    if (!user) return res.status(404).json({ success: false, message: "User not found." });

    res.json({ success: true, user });
  } catch (error) {
    res.status(500).json({ success: false, message: "Server error." });
  }
});

// ─── UPDATE PROFILE ───────────────────────────────────────────────────────────
// PUT /api/profile
// Logged-in user updates their own profile
router.put("/profile", isAuthenticated, async (req, res) => {
  try {
    const { username } = req.body;

    const user = await User.findByIdAndUpdate(
      req.session.userId,
      { username },
      { new: true, runValidators: true } // return updated doc, run schema validators
    );

    // Sync the session with the updated username
    req.session.username = user.username;

    res.json({ success: true, message: "Profile updated!", user });
  } catch (error) {
    res.status(500).json({ success: false, message: "Update failed." });
  }
});

// ─── CHANGE PASSWORD ──────────────────────────────────────────────────────────
// PUT /api/change-password
router.put("/change-password", isAuthenticated, async (req, res) => {
  try {
    const { currentPassword, newPassword } = req.body;

    // Fetch user from DB (we need the hashed password for comparison)
    const user = await User.findById(req.session.userId);

    // Verify the current password before allowing a change
    const isMatch = await user.comparePassword(currentPassword);
    if (!isMatch) {
      return res.status(401).json({ success: false, message: "Current password is incorrect." });
    }

    // Update — the pre-save hook will hash the new password automatically
    user.password = newPassword;
    await user.save();

    // Security best practice: invalidate all other sessions after password change
    // This logs the user out everywhere except their current session
    // In a full implementation, you'd track session IDs per user
    res.json({ success: true, message: "Password changed successfully." });
  } catch (error) {
    res.status(500).json({ success: false, message: "Password change failed." });
  }
});

// ─── ADMIN PANEL ──────────────────────────────────────────────────────────────
// GET /api/admin
// Both isAuthenticated AND isAdmin must pass
router.get("/admin", isAuthenticated, isAdmin, async (req, res) => {
  try {
    const users = await User.find({}).select("-password"); // exclude password field
    res.json({
      success: true,
      message: "Admin panel",
      totalUsers: users.length,
      users,
    });
  } catch (error) {
    res.status(500).json({ success: false, message: "Server error." });
  }
});

// ─── DELETE USER (admin only) ─────────────────────────────────────────────────
// DELETE /api/admin/users/:id
router.delete("/admin/users/:id", isAuthenticated, isAdmin, async (req, res) => {
  try {
    // Prevent admin from deleting themselves
    if (req.params.id === req.session.userId) {
      return res.status(400).json({ success: false, message: "You cannot delete your own account." });
    }

    const user = await User.findByIdAndDelete(req.params.id);
    if (!user) return res.status(404).json({ success: false, message: "User not found." });

    res.json({ success: true, message: `User "${user.username}" deleted.` });
  } catch (error) {
    res.status(500).json({ success: false, message: "Delete failed." });
  }
});

module.exports = router;
```

---

## 12. The Main Server File

```js
// server.js

const express = require("express");
const session = require("express-session");
const MongoStore = require("connect-mongo");
require("dotenv").config();

const connectDB = require("./config/db");
const authRoutes = require("./routes/authRoutes");
const protectedRoutes = require("./routes/protectedRoutes");

const app = express();

// ─── Connect to MongoDB ───────────────────────────────────────────────────────
connectDB();

// ─── Body Parsing ─────────────────────────────────────────────────────────────
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

// ─── Session Configuration ────────────────────────────────────────────────────
app.use(session({
  name: "sessionId",                               // custom name hides that you use express-session
  secret: process.env.SESSION_SECRET,             // from .env
  resave: false,                                  // don't save if nothing changed
  saveUninitialized: false,                       // don't create empty sessions
  store: MongoStore.create({
    mongoUrl: process.env.MONGO_URI,
    collectionName: "sessions",
    ttl: 60 * 60 * 24 * 7,                       // sessions expire after 7 days
    autoRemove: "native",                         // MongoDB TTL index removes expired sessions
  }),
  cookie: {
    httpOnly: true,                               // JS cannot read cookie
    secure: process.env.NODE_ENV === "production",// HTTPS only in production
    sameSite: "strict",                           // CSRF protection
    maxAge: 1000 * 60 * 60 * 24 * 7,             // 7 days in ms (must match ttl above)
  },
}));

// ─── Routes ───────────────────────────────────────────────────────────────────
app.use("/auth", authRoutes);     // /auth/register, /auth/login, /auth/logout, /auth/me
app.use("/api", protectedRoutes); // /api/dashboard, /api/profile, /api/admin, etc.

// ─── 404 Handler ─────────────────────────────────────────────────────────────
app.use((req, res) => {
  res.status(404).json({ success: false, message: "Route not found." });
});

// ─── Global Error Handler ─────────────────────────────────────────────────────
app.use((err, req, res, next) => {
  console.error("Unhandled error:", err.message);
  res.status(500).json({ success: false, message: "Internal server error." });
});

// ─── Start Server ─────────────────────────────────────────────────────────────
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

---

## 13. Complete Request Flow Trace — Every Request Type

### Registration Request

```
POST /auth/register
Body: { username: "pranav", email: "p@gmail.com", password: "Secret@1", confirmPassword: "Secret@1" }

1. express.json()            → parses body into req.body
2. session middleware         → no cookie, no session (that's fine for register)
3. isGuest middleware         → req.session.isLoggedIn is undefined → not logged in → next()
4. registerValidator          → all fields valid ✓
5. Route handler runs
6. User.findOne({ email })    → no existing user found ✓
7. new User({...}).save()     → pre-save hook fires → bcrypt hashes password
8. User saved to MongoDB:     { username: "pranav", email: "p@gmail.com", password: "$2b$10$..." }
9. toJSON() strips password
10. Response: 201 { success: true, message: "Account created!" }
```

### Login Request

```
POST /auth/login
Body: { email: "p@gmail.com", password: "Secret@1" }

1. express.json()             → parses body
2. session middleware          → no cookie yet → no session
3. isGuest                    → not logged in → next()
4. loginValidator             → valid ✓
5. User.findOne({ email })    → user found ✓
6. user.comparePassword()     → bcrypt.compare("Secret@1", "$2b$10$...") → true ✓
7. req.session.regenerate()   → old session deleted, new session ID "xyz789" created
8. req.session.isLoggedIn = true
   req.session.userId = "64abc..."
   req.session.username = "pranav"
   req.session.role = "user"
9. req.session.save()         → session saved to MongoDB sessions collection
10. Response: 200 { success: true, message: "Welcome back, pranav!" }
    Set-Cookie: sessionId=xyz789; HttpOnly; SameSite=Strict
```

### Authenticated Request

```
GET /api/dashboard
Cookie: sessionId=xyz789

1. express.json()             → no body to parse
2. session middleware          → reads cookie → finds "xyz789"
                               → queries MongoDB sessions collection
                               → loads { isLoggedIn: true, userId: "64abc...", username: "pranav" }
                               → populates req.session
3. isAuthenticated             → req.session.isLoggedIn === true ✓ → next()
4. Route handler runs
5. Response: 200 { message: "Welcome to your dashboard, pranav!" }
```

### Failed Auth Request (no cookie)

```
GET /api/dashboard
(no cookie sent)

1. session middleware          → no cookie → req.session is a new empty session object
2. isAuthenticated             → req.session.isLoggedIn === undefined → FAIL
3. Response: 401 { message: "Authentication required. Please log in." }
   (route handler never runs)
```

### Logout Request

```
POST /auth/logout
Cookie: sessionId=xyz789

1. session middleware          → loads session for "xyz789"
2. isAuthenticated             → logged in ✓ → next()
3. username = req.session.username  → "pranav" (saved before destroy)
4. req.session.destroy()      → MongoDB session document for "xyz789" DELETED
5. res.clearCookie("sessionId") → browser told to delete the cookie
6. Response: 200 { message: "Goodbye, pranav!" }

Now: if "xyz789" is used again → session middleware finds nothing → 401
```

---

## 14. API Endpoints Summary

|Method|Endpoint|Middleware|Description|
|---|---|---|---|
|POST|/auth/register|isGuest, validator|Create new account|
|POST|/auth/login|isGuest, validator|Log in and create session|
|POST|/auth/logout|isAuthenticated|Destroy session|
|GET|/auth/me|isAuthenticated|Get current user|
|GET|/api/dashboard|isAuthenticated|User dashboard|
|GET|/api/profile|isAuthenticated|Full profile from DB|
|PUT|/api/profile|isAuthenticated|Update profile|
|PUT|/api/change-password|isAuthenticated|Change password|
|GET|/api/admin|isAuthenticated, isAdmin|Admin panel|
|DELETE|/api/admin/users/:id|isAuthenticated, isAdmin|Delete a user|

---

## 15. Security Checklist for Session-Based Auth

Work through this before deploying any auth system:

**Passwords**

- [ ] Passwords are hashed with bcrypt (never plain text, never MD5/SHA)
- [ ] bcrypt salt rounds are 10 or higher
- [ ] Password is never stored in the session or returned in any response
- [ ] Same error message for wrong email and wrong password ("Invalid credentials")

**Session Configuration**

- [ ] `secret` is a long random string stored in an environment variable
- [ ] `resave: false` and `saveUninitialized: false` are set
- [ ] `httpOnly: true` on the cookie
- [ ] `secure: true` in production (requires HTTPS)
- [ ] `sameSite: "strict"` or `"lax"` is set
- [ ] Session `maxAge` is set to a reasonable time (not infinite)
- [ ] A real session store is used in production (MongoDB, Redis — not MemoryStore)

**Login/Logout Behavior**

- [ ] `req.session.regenerate()` is called on successful login
- [ ] `req.session.destroy()` is called on logout
- [ ] `res.clearCookie()` is called on logout

**Route Protection**

- [ ] All sensitive routes are protected by `isAuthenticated` middleware
- [ ] Role-based routes check `isAdmin` (or similar) AFTER `isAuthenticated`

---

## 16. Common Mistakes and Fixes

**"My session disappears on every request"** You are probably not calling `req.session.save()` after regenerating, or your session store isn't connected. Check your MongoDB/Redis connection and confirm `SESSION_SECRET` is set in your `.env`.

**"req.session is undefined"** The session middleware is registered after the route that uses it. Order matters in Express — `app.use(session(...))` must come before your route registrations.

**"My session works in development but not in production"** You forgot to set `secure: process.env.NODE_ENV === "production"`. With `secure: true` and no HTTPS, the browser refuses to send the cookie.

**"Login works but re-logging in with the same account causes issues"** You're not calling `req.session.regenerate()` before setting session data. Always regenerate after authentication.

**"Users stay logged in forever"** You haven't set `maxAge` on the cookie or `ttl` on the session store. Always set both, and make sure they match.

---

> ✅ **You now have a complete, production-aware implementation of session-based authentication. You understand not just the "how" but the "why" behind every decision — from bcrypt salt rounds to session regeneration to the same error message for wrong email and wrong password.**
> 
> **The natural next topic is JWT (JSON Web Token) authentication — the modern stateless alternative to sessions. Understanding both gives you the knowledge to choose the right approach for any project: sessions are ideal for server-rendered apps and monoliths; JWTs are ideal for REST APIs consumed by mobile apps or separate frontends.**