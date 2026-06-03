
---

# API Development with Express.js — Complete Beginner Notes

---

## 1. What Is an API?

Before writing a single line of code, you need to have a crystal-clear mental model of what an API actually is — because this word gets thrown around constantly and its meaning often gets lost.

**API stands for Application Programming Interface.** The key word is _interface_. An interface is a defined boundary through which two things communicate. A light switch is an interface between you and the electrical system in your wall — you don't need to know how the wiring works, you just flip the switch.

An API is that same idea applied to software. It is a defined set of rules and endpoints that allows one piece of software to talk to another. When your React frontend needs user data, it doesn't dig directly into the database — it talks to the API, and the API talks to the database and hands back only what was asked for.

```
Without API (bad):
  Frontend → directly queries database → gets raw data → tries to display it
  Problem: frontend now knows the database structure, security is impossible,
           and every client needs its own database logic

With API (good):
  Frontend → HTTP request to API → API queries database → API sends clean JSON
  Result: frontend knows nothing about the database, one source of truth,
          any client (web, mobile, CLI) can talk to the same API
```

So an API acts as a **middleman and gatekeeper** — it exposes exactly the functionality you choose to expose, in exactly the format you design, with exactly the security rules you enforce.

---

## 2. What Is a REST API?

The APIs we build with Express are almost always **REST APIs** (or RESTful APIs). REST stands for **Representational State Transfer** — a set of architectural principles for designing APIs that are simple, scalable, and predictable.

You don't need to memorize the formal definition. What matters is the practical principles:

**Resources are nouns, not verbs.** Your endpoints should represent things (resources), not actions. The HTTP method tells you the action.

```
❌ Bad (verb-based):
  GET  /getUsers
  POST /createUser
  GET  /deleteUser?id=42

✅ Good (noun-based):
  GET    /users         → get all users
  POST   /users         → create a user
  DELETE /users/42      → delete user with id 42
```

**HTTP methods carry meaning.** REST uses standard HTTP methods to express what operation you want to perform on a resource.

```
GET    → Read   (safe, no side effects)
POST   → Create (adds something new)
PUT    → Replace (completely replace a resource)
PATCH  → Update (partially update a resource)
DELETE → Delete (remove a resource)
```

**Statelessness.** Every request must contain all the information needed to process it. The server should not rely on memory of previous requests. Authentication tokens or session IDs must be sent with every request that needs them.

**Consistent response format.** Your API should always respond in a predictable format so clients know what to expect. JSON is the standard.

---

## 3. Why Build APIs with Express?

Express is arguably the most popular framework for building Node.js APIs. Here is why it is such a strong choice:

It is **minimal and unopinionated** — Express gives you the tools and gets out of your way. You structure your project exactly how you want, which means you understand every piece of your architecture.

It is **extremely fast to set up** — you can have a working API endpoint in under 10 lines of code.

It has a **massive ecosystem** — middleware for authentication, validation, file uploads, rate limiting, logging, CORS, and virtually anything else already exists as an npm package.

It **maps directly to HTTP** — when you write `app.get("/users", handler)`, you are quite literally defining "when a GET request hits /users, run this handler." There is no magic, no framework abstraction — just HTTP, clear as day.

---

## 4. The Anatomy of an API Request and Response

Every API interaction has the same two parts: a request going in and a response coming out. Understanding every component of both is fundamental.

### The Request Object (`req`)

When a client sends a request to your Express API, everything about that request is available on the `req` object:

```
GET /api/users/42?include=posts&format=full
Headers: Authorization: Bearer token123
         Content-Type: application/json
Body: { "name": "Pranav" }
```

```js
req.params.id        // "42"              → from the URL path :id
req.query.include    // "posts"           → from ?include=posts
req.query.format     // "full"            → from &format=full
req.headers["authorization"] // "Bearer token123" → from request headers
req.body.name        // "Pranav"          → from the JSON body (needs express.json() middleware)
req.method           // "GET"             → the HTTP method
req.path             // "/api/users/42"   → the URL path
req.ip               // "127.0.0.1"       → client IP address
```

### The Response Object (`res`)

The `res` object is how you send data back to the client. The most important methods are:

```js
res.json({ data: "..." })    // send JSON response (sets Content-Type automatically)
res.status(404).json({...})  // set status code AND send JSON
res.send("plain text")       // send plain text
res.sendStatus(204)          // send status code with no body (e.g., successful DELETE)
res.set("X-Custom", "value") // set a response header
res.redirect("/new-path")    // redirect to another URL
```

---

## 5. HTTP Status Codes — The Language of APIs

Status codes are how your API communicates the _outcome_ of a request. Clients — whether a frontend app or another service — rely on these codes to know how to handle the response. Using them correctly is a mark of a professional API.

The codes are grouped into ranges, and each range has a meaning. The `2xx` range means success. The `3xx` range means redirection. The `4xx` range means the client made an error. The `5xx` range means the server made an error.

Here are the ones you will use constantly:

`200 OK` is the generic success response. Use it for successful GET, PUT, and PATCH requests.

`201 Created` is specifically for when a new resource was successfully created. Always return this after a successful POST that creates something, and ideally include the created resource in the response body.

`204 No Content` means success but there is nothing to send back. Use this for successful DELETE operations where there is no resource left to return.

`400 Bad Request` means the client sent something malformed — missing required fields, wrong data types, invalid values. This is a client error.

`401 Unauthorized` means the client is not authenticated. They haven't provided credentials (or provided invalid ones). The word "unauthorized" is historically misleading — it really means "unauthenticated."

`403 Forbidden` means the client IS authenticated but does not have permission to do what they're trying to do. A logged-in regular user trying to access an admin route gets a 403.

`404 Not Found` means the requested resource doesn't exist. Use this when someone requests `/users/999` and user 999 doesn't exist in your database.

`409 Conflict` means the request conflicts with the current state of the server. The classic example is trying to register with an email that already exists.

`422 Unprocessable Entity` means the server understands the request but cannot process it due to semantic errors — often used for validation failures.

`429 Too Many Requests` means the client is being rate-limited.

`500 Internal Server Error` means something broke on the server side — an unhandled exception, a database connection failure, etc. This is your fault, not the client's.

---

## 6. Project Structure — How to Organize an Express API

For anything beyond a trivial demo, a flat `server.js` with everything in one file becomes impossible to maintain. A well-organized Express API follows the **MVC-ish pattern**:

```
my-api/
├── server.js              ← entry point: creates app, registers middleware, starts server
├── app.js                 ← app setup: middleware + routes (separate from server startup)
├── .env                   ← environment variables (never commit this!)
├── config/
│   └── db.js              ← database connection logic
├── models/
│   └── User.js            ← Mongoose schema / data shape
├── controllers/
│   └── userController.js  ← business logic: what happens when a route is hit
├── routes/
│   └── userRoutes.js      ← route definitions: which URL maps to which controller function
├── middleware/
│   ├── auth.js            ← authentication/authorization middleware
│   ├── validate.js        ← validation error handler middleware
│   └── errorHandler.js    ← global error handler
└── validators/
    └── userValidator.js   ← express-validator rules
```

This separation exists for a very practical reason: each file has one job. Routes only care about URLs and HTTP methods. Controllers only care about business logic. Models only care about data shape. When something breaks, you know exactly which file to look in.

---

## 7. The Request-Response Cycle in an Express API

Let's trace what happens from the moment a request hits your server to the moment a response goes back. Understanding this cycle helps you know where to put your logic.

```
Incoming Request: POST /api/users
                         │
                         ▼
              ┌─────────────────────┐
              │  express.json()      │  ← parses JSON body into req.body
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │  cors() middleware   │  ← adds CORS headers for browser clients
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │  morgan() logger     │  ← logs: POST /api/users 201 42ms
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │  Route matching      │  ← finds: router.post("/users", ...)
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │  Auth middleware     │  ← checks JWT/session, populates req.user
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │  Validator           │  ← checks body fields with express-validator
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │  Controller function │  ← your actual business logic
              │  (createUser)        │     queries DB, builds response
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │  Error handler       │  ← catches anything that went wrong
              └──────────┬──────────┘
                         │
                         ▼
              Outgoing Response: 201 { user: {...} }
```

---

## 8. Building the API — Layer by Layer

Let's now build each layer of a real API step by step, understanding every decision.

### 8.1 Route Definitions

Routes are just the mapping from a URL + HTTP method to a handler function. Keep them thin — routes should only say "this URL goes to this controller function."

```js
// routes/userRoutes.js

const express = require("express");
const router = express.Router();
const userController = require("../controllers/userController");
const { protect, restrictTo } = require("../middleware/auth");
const { userCreateValidator, userUpdateValidator } = require("../validators/userValidator");
const validate = require("../middleware/validate");

// Public routes — no authentication needed
router.post("/register", userCreateValidator, validate, userController.register);
router.post("/login",    userController.login);

// Protected routes — must be logged in
router.use(protect); // all routes below this line require authentication

router.get("/",     userController.getAllUsers);
router.get("/me",   userController.getMe);
router.get("/:id",  userController.getUserById);
router.put("/:id",  userUpdateValidator, validate, userController.updateUser);
router.delete("/:id", restrictTo("admin"), userController.deleteUser);

module.exports = router;
```

Notice how `router.use(protect)` is placed in the middle of the route definitions. Every route registered after that line automatically inherits the `protect` middleware. This is a clean pattern that avoids having to add `protect` to every single route individually.

### 8.2 Controllers

Controllers are where your actual business logic lives. A controller function receives `req` and `res`, does its work (usually involving a database call), and sends back a response.

```js
// controllers/userController.js

const User = require("../models/User");
const catchAsync = require("../utils/catchAsync"); // explained later
const AppError = require("../utils/appError");     // explained later

// GET /api/users — get all users
exports.getAllUsers = catchAsync(async (req, res, next) => {
  // Support filtering: GET /api/users?role=admin
  const filter = {};
  if (req.query.role) filter.role = req.query.role;

  // Support sorting: GET /api/users?sort=createdAt
  const sort = req.query.sort || "-createdAt"; // default: newest first

  // Support pagination: GET /api/users?page=2&limit=10
  const page  = parseInt(req.query.page)  || 1;
  const limit = parseInt(req.query.limit) || 10;
  const skip  = (page - 1) * limit;

  const [users, total] = await Promise.all([
    User.find(filter).sort(sort).skip(skip).limit(limit),
    User.countDocuments(filter),
  ]);

  res.status(200).json({
    success: true,
    count:   users.length,
    total,
    page,
    pages:   Math.ceil(total / limit),
    data:    { users },
  });
});

// GET /api/users/:id — get one user
exports.getUserById = catchAsync(async (req, res, next) => {
  const user = await User.findById(req.params.id);

  // If no user found, create an error and pass it to next()
  // next() with an argument skips straight to the error handler
  if (!user) {
    return next(new AppError(`User with id ${req.params.id} not found`, 404));
  }

  res.status(200).json({
    success: true,
    data:    { user },
  });
});

// POST /api/users/register — create a new user
exports.register = catchAsync(async (req, res, next) => {
  const { username, email, password } = req.body;

  // Check for duplicate email
  const existing = await User.findOne({ email });
  if (existing) {
    return next(new AppError("An account with this email already exists.", 409));
  }

  // The model's pre-save hook handles password hashing
  const user = await User.create({ username, email, password });

  res.status(201).json({
    success: true,
    message: "Account created successfully!",
    data:    { user }, // password stripped by toJSON() in model
  });
});

// PUT /api/users/:id — update a user
exports.updateUser = catchAsync(async (req, res, next) => {
  // Prevent changing password through this route — use a dedicated route for that
  delete req.body.password;

  const user = await User.findByIdAndUpdate(
    req.params.id,
    req.body,
    {
      new:            true,  // return the updated document
      runValidators:  true,  // run Mongoose schema validators on the update
    }
  );

  if (!user) return next(new AppError("User not found", 404));

  res.status(200).json({
    success: true,
    data:    { user },
  });
});

// DELETE /api/users/:id — delete a user
exports.deleteUser = catchAsync(async (req, res, next) => {
  const user = await User.findByIdAndDelete(req.params.id);

  if (!user) return next(new AppError("User not found", 404));

  // 204 No Content — success but nothing to send back
  res.status(204).json({ success: true, data: null });
});

// GET /api/users/me — get the currently logged-in user
exports.getMe = catchAsync(async (req, res, next) => {
  // req.user is populated by the auth middleware (protect)
  const user = await User.findById(req.user.id);

  res.status(200).json({
    success: true,
    data:    { user },
  });
});
```

### 8.3 Error Handling — The Professional Pattern

Error handling is where most beginner APIs fall apart. The naive approach is to wrap every controller in a try/catch and manually write `res.status(500).json(...)` everywhere. This is repetitive, inconsistent, and easy to forget.

The professional approach has two parts: a `catchAsync` wrapper that automatically catches errors from async functions, and a custom `AppError` class for creating operational errors with status codes.

```js
// utils/catchAsync.js

// This is a higher-order function — it takes an async function and returns
// a new function that wraps it in a try/catch. Any rejected promise or
// thrown error is automatically passed to Express's error handler via next(err).
const catchAsync = (fn) => {
  return (req, res, next) => {
    fn(req, res, next).catch(next);
    //                       ↑
    // .catch(next) is equivalent to .catch(err => next(err))
    // It passes any error to Express's next() which triggers the error handler
  };
};

module.exports = catchAsync;
```

```js
// utils/appError.js

// A custom Error class that carries an HTTP status code.
// This lets you create meaningful errors anywhere in your code:
//   throw new AppError("User not found", 404)
// and the error handler can use the status code to send the right HTTP response.
class AppError extends Error {
  constructor(message, statusCode) {
    super(message); // call the parent Error constructor

    this.statusCode = statusCode;
    this.status = `${statusCode}`.startsWith("4") ? "fail" : "error";

    // isOperational = true means this is an expected, handled error
    // (as opposed to unexpected bugs which would be isOperational = false)
    this.isOperational = true;

    // Captures the stack trace without including AppError itself
    Error.captureStackTrace(this, this.constructor);
  }
}

module.exports = AppError;
```

```js
// middleware/errorHandler.js

const AppError = require("../utils/appError");

// This is the global error handler — it must have exactly 4 parameters
// Express identifies error-handling middleware by this signature
const globalErrorHandler = (err, req, res, next) => {

  err.statusCode = err.statusCode || 500;
  err.status     = err.status     || "error";

  // Handle specific Mongoose errors and convert them to AppErrors
  let error = { ...err, message: err.message };

  // Mongoose cast error — invalid ObjectId (e.g., /users/not-an-id)
  if (err.name === "CastError") {
    error = new AppError(`Invalid ${err.path}: ${err.value}`, 400);
  }

  // Mongoose duplicate key error (unique constraint violated)
  if (err.code === 11000) {
    const field = Object.keys(err.keyValue)[0];
    error = new AppError(`Duplicate value for field: ${field}`, 409);
  }

  // Mongoose validation error (schema validators failed)
  if (err.name === "ValidationError") {
    const messages = Object.values(err.errors).map(e => e.message);
    error = new AppError(`Validation failed: ${messages.join(". ")}`, 400);
  }

  // JWT errors (for JWT-based auth)
  if (err.name === "JsonWebTokenError") {
    error = new AppError("Invalid token. Please log in again.", 401);
  }
  if (err.name === "TokenExpiredError") {
    error = new AppError("Your token has expired. Please log in again.", 401);
  }

  // In development: send full error details for debugging
  if (process.env.NODE_ENV === "development") {
    return res.status(error.statusCode).json({
      success: false,
      status:  error.status,
      message: error.message,
      stack:   err.stack,    // full stack trace — only in development!
    });
  }

  // In production: only send operational errors to the client
  // Never expose stack traces or internal error details in production
  if (error.isOperational) {
    return res.status(error.statusCode).json({
      success: false,
      status:  error.status,
      message: error.message,
    });
  }

  // For unexpected programming errors: log it and send a generic message
  console.error("UNEXPECTED ERROR:", err);
  return res.status(500).json({
    success: false,
    status:  "error",
    message: "Something went wrong. Please try again later.",
  });
};

module.exports = globalErrorHandler;
```

### 8.4 Consistent API Response Format

One of the hallmarks of a well-designed API is **consistency**. Every response — success or failure — should follow the same structure so clients always know what to expect.

```js
// A good consistent response shape:

// Success response:
{
  "success": true,
  "data": {
    "user": { "id": "...", "username": "pranav" }
  }
}

// Success with pagination:
{
  "success": true,
  "count": 10,
  "total": 47,
  "page": 2,
  "pages": 5,
  "data": {
    "users": [...]
  }
}

// Error response:
{
  "success": false,
  "status": "fail",
  "message": "User with id 999 not found"
}
```

The key principle is that `data` always wraps the actual payload, `success` is always a boolean, and errors always have a `message`. This makes writing frontend code much cleaner because you can always check `response.data.success` to know if the request worked.

---

## 9. Filtering, Sorting, and Pagination

These three features are what separate a toy API from a production-ready one. Without them, `GET /users` returns all 10,000 users in your database every time — that is unusable.

```js
// A reusable API features class that handles filtering, sorting, and pagination
// This encapsulates the query-building logic in one place

class APIFeatures {
  constructor(query, queryString) {
    // query      → the Mongoose query object (e.g., User.find())
    // queryString → req.query (e.g., { role: "admin", sort: "-createdAt", page: "2" })
    this.query       = query;
    this.queryString = queryString;
  }

  filter() {
    // Copy queryString and remove non-filter fields
    const queryObj = { ...this.queryString };
    const excludedFields = ["page", "sort", "limit", "fields"];
    excludedFields.forEach(field => delete queryObj[field]);

    // Support advanced filters: ?age[gte]=18 → { age: { $gte: 18 } }
    // We replace "gte", "gt", "lte", "lt" with "$gte", "$gt", "$lte", "$lt"
    let queryStr = JSON.stringify(queryObj);
    queryStr = queryStr.replace(/\b(gte|gt|lte|lt)\b/g, match => `$${match}`);

    this.query = this.query.find(JSON.parse(queryStr));
    return this; // return this for chaining
  }

  sort() {
    if (this.queryString.sort) {
      // ?sort=price,createdAt → sort("price createdAt")
      const sortBy = this.queryString.sort.split(",").join(" ");
      this.query = this.query.sort(sortBy);
    } else {
      this.query = this.query.sort("-createdAt"); // default: newest first
    }
    return this;
  }

  limitFields() {
    if (this.queryString.fields) {
      // ?fields=name,email → only return name and email fields
      const fields = this.queryString.fields.split(",").join(" ");
      this.query = this.query.select(fields);
    } else {
      this.query = this.query.select("-__v"); // exclude __v by default
    }
    return this;
  }

  paginate() {
    const page  = parseInt(this.queryString.page)  || 1;
    const limit = parseInt(this.queryString.limit) || 10;
    const skip  = (page - 1) * limit;

    this.query = this.query.skip(skip).limit(limit);
    return this;
  }
}

module.exports = APIFeatures;
```

Using this class in a controller becomes elegantly readable:

```js
exports.getAllUsers = catchAsync(async (req, res) => {
  const features = new APIFeatures(User.find(), req.query)
    .filter()
    .sort()
    .limitFields()
    .paginate();

  const users = await features.query;

  res.status(200).json({
    success: true,
    count:   users.length,
    data:    { users },
  });
});
```

Now your API supports all of these out of the box:

- `GET /api/users?role=admin` — filter by role
- `GET /api/users?sort=-createdAt` — sort newest first
- `GET /api/users?fields=name,email` — only return specific fields
- `GET /api/users?page=2&limit=5` — get page 2, 5 per page
- `GET /api/users?age[gte]=18` — filter users 18 or older

---

## 10. Rate Limiting — Protecting Your API

Rate limiting prevents abuse — a single client cannot hammer your API with thousands of requests. It is also your first line of defense against brute-force attacks on login endpoints.

```bash
npm install express-rate-limit
```

```js
const rateLimit = require("express-rate-limit");

// General API limiter: 100 requests per 15 minutes per IP
const apiLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max:      100,
  message: {
    success: false,
    error:   "Too many requests from this IP. Please try again after 15 minutes.",
  },
  standardHeaders: true,  // send rate limit info in RateLimit-* headers
  legacyHeaders:   false,
});

// Stricter limiter for auth endpoints — prevents brute force
const authLimiter = rateLimit({
  windowMs: 60 * 60 * 1000, // 1 hour
  max:      10,              // only 10 login attempts per hour
  message: {
    success: false,
    error:   "Too many login attempts. Please try again in 1 hour.",
  },
});

// Apply general limiter to all API routes
app.use("/api", apiLimiter);

// Apply strict limiter to auth routes only
app.use("/api/auth/login",    authLimiter);
app.use("/api/auth/register", authLimiter);
```

---

## 11. CORS — Making Your API Accessible to Browsers

When a browser makes a request to a different domain (or even a different port on the same domain), it applies **Cross-Origin Resource Sharing** rules. Without proper CORS headers from your server, the browser blocks the response even if the server processed the request successfully.

```bash
npm install cors
```

```js
const cors = require("cors");

// In development: allow all origins
app.use(cors());

// In production: only allow your specific frontend domain
app.use(cors({
  origin:      "https://yourfrontend.com",
  methods:     ["GET", "POST", "PUT", "PATCH", "DELETE"],
  credentials: true, // allow cookies to be sent cross-origin (needed for session auth)
  allowedHeaders: ["Content-Type", "Authorization"],
}));

// If you have multiple allowed origins:
const allowedOrigins = ["https://yourapp.com", "https://admin.yourapp.com"];
app.use(cors({
  origin: (origin, callback) => {
    if (!origin || allowedOrigins.includes(origin)) {
      callback(null, true);
    } else {
      callback(new Error("Not allowed by CORS"));
    }
  },
}));
```

---

## 12. Request Logging with Morgan

Logging is essential for understanding what is happening in your API. Morgan is the standard HTTP request logger for Express.

```bash
npm install morgan
```

```js
const morgan = require("morgan");

// In development: "dev" format shows METHOD URL STATUS TIME
// GET /api/users 200 42.361 ms - 1842
app.use(morgan("dev"));

// In production: "combined" format includes IP, user agent, timestamp — good for log analysis
app.use(morgan("combined"));
```

---

## 13. Environment Variables and Configuration

Never hardcode secrets, database URLs, or port numbers. Use environment variables so your code works across different environments (development, staging, production) without modification.

```
# .env
PORT=3000
NODE_ENV=development
MONGO_URI=mongodb://localhost:27017/myapi
JWT_SECRET=j3k9Lm2pQ8wZvRxN7cH6bT1sY4uAf5gD
JWT_EXPIRES_IN=7d
```

```js
// Load .env early — before any other code runs
require("dotenv").config();

// Access anywhere in the project
process.env.PORT          // "3000"
process.env.MONGO_URI     // "mongodb://..."
process.env.JWT_SECRET    // "j3k9Lm..."
process.env.NODE_ENV      // "development"
```

---

## 14. Real-World Example — A Complete Blog Post API

Now let's put everything together into a complete, production-aware REST API for a blog platform. It covers CRUD operations, authentication, pagination, filtering, error handling, rate limiting, and CORS.

```
Project structure:
├── server.js
├── app.js
├── .env
├── config/
│   └── db.js
├── models/
│   ├── User.js
│   └── Post.js
├── controllers/
│   ├── authController.js
│   └── postController.js
├── routes/
│   ├── authRoutes.js
│   └── postRoutes.js
├── middleware/
│   ├── auth.js
│   ├── validate.js
│   └── errorHandler.js
├── validators/
│   └── postValidator.js
└── utils/
    ├── catchAsync.js
    ├── appError.js
    └── apiFeatures.js
```

```
# .env
PORT=3000
NODE_ENV=development
MONGO_URI=mongodb://localhost:27017/blog-api
JWT_SECRET=j3k9Lm2pQ8wZvRxN7cH6bT1sY4uAf5gDe0hB
JWT_EXPIRES_IN=7d
```

```js
// config/db.js

const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    const conn = await mongoose.connect(process.env.MONGO_URI);
    console.log(`MongoDB connected: ${conn.connection.host}`);
  } catch (err) {
    console.error("DB connection failed:", err.message);
    process.exit(1);
  }
};

module.exports = connectDB;
```

```js
// models/User.js

const mongoose = require("mongoose");
const bcrypt   = require("bcrypt");

const userSchema = new mongoose.Schema({
  username: { type: String, required: true, trim: true, minlength: 3 },
  email:    { type: String, required: true, unique: true, lowercase: true },
  password: { type: String, required: true, minlength: 8 },
  role:     { type: String, enum: ["user", "admin"], default: "user" },
}, { timestamps: true });

// Hash password before saving
userSchema.pre("save", async function (next) {
  if (!this.isModified("password")) return next();
  this.password = await bcrypt.hash(this.password, 12);
  next();
});

// Compare passwords
userSchema.methods.correctPassword = async function (candidate, stored) {
  return await bcrypt.compare(candidate, stored);
};

// Never expose password in responses
userSchema.methods.toJSON = function () {
  const obj = this.toObject();
  delete obj.password;
  return obj;
};

module.exports = mongoose.model("User", userSchema);
```

```js
// models/Post.js

const mongoose = require("mongoose");

const postSchema = new mongoose.Schema({
  title: {
    type:     String,
    required: [true, "Post title is required"],
    trim:     true,
    minlength: [5,  "Title must be at least 5 characters"],
    maxlength: [100, "Title cannot exceed 100 characters"],
  },
  content: {
    type:     String,
    required: [true, "Post content is required"],
    minlength: [20, "Content must be at least 20 characters"],
  },
  tags: {
    type:    [String],
    default: [],
  },
  published: {
    type:    Boolean,
    default: false,
  },
  author: {
    type: mongoose.Schema.Types.ObjectId,
    ref:  "User",          // reference to the User model
    required: true,
  },
  views: {
    type:    Number,
    default: 0,
  },
}, { timestamps: true });

// Virtual field: word count (not stored in DB, computed on the fly)
postSchema.virtual("wordCount").get(function () {
  return this.content.split(" ").length;
});

// Ensure virtuals are included when converting to JSON
postSchema.set("toJSON",   { virtuals: true });
postSchema.set("toObject", { virtuals: true });

module.exports = mongoose.model("Post", postSchema);
```

```js
// utils/catchAsync.js
module.exports = (fn) => (req, res, next) => fn(req, res, next).catch(next);
```

```js
// utils/appError.js
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode  = statusCode;
    this.status      = `${statusCode}`.startsWith("4") ? "fail" : "error";
    this.isOperational = true;
    Error.captureStackTrace(this, this.constructor);
  }
}
module.exports = AppError;
```

```js
// utils/apiFeatures.js
class APIFeatures {
  constructor(query, queryString) {
    this.query       = query;
    this.queryString = queryString;
  }
  filter() {
    const q = { ...this.queryString };
    ["page", "sort", "limit", "fields"].forEach(f => delete q[f]);
    let str = JSON.stringify(q).replace(/\b(gte|gt|lte|lt)\b/g, m => `$${m}`);
    this.query = this.query.find(JSON.parse(str));
    return this;
  }
  sort() {
    const sortBy = this.queryString.sort
      ? this.queryString.sort.split(",").join(" ")
      : "-createdAt";
    this.query = this.query.sort(sortBy);
    return this;
  }
  limitFields() {
    const fields = this.queryString.fields
      ? this.queryString.fields.split(",").join(" ")
      : "-__v";
    this.query = this.query.select(fields);
    return this;
  }
  paginate() {
    const page  = parseInt(this.queryString.page)  || 1;
    const limit = parseInt(this.queryString.limit) || 10;
    this.query  = this.query.skip((page - 1) * limit).limit(limit);
    return this;
  }
}
module.exports = APIFeatures;
```

```js
// middleware/auth.js

const jwt        = require("jsonwebtoken");
const User       = require("../models/User");
const catchAsync = require("../utils/catchAsync");
const AppError   = require("../utils/appError");

// protect: verify JWT and attach user to req.user
exports.protect = catchAsync(async (req, res, next) => {
  // JWT is sent in Authorization header as: Bearer <token>
  let token;
  if (req.headers.authorization?.startsWith("Bearer")) {
    token = req.headers.authorization.split(" ")[1];
  }

  if (!token) {
    return next(new AppError("You are not logged in. Please log in to get access.", 401));
  }

  // Verify the token — throws if invalid or expired
  const decoded = jwt.verify(token, process.env.JWT_SECRET);

  // Check if the user still exists (they might have been deleted after token was issued)
  const user = await User.findById(decoded.id);
  if (!user) {
    return next(new AppError("The user belonging to this token no longer exists.", 401));
  }

  req.user = user; // attach user to request for use in subsequent middleware/routes
  next();
});

// restrictTo: check that authenticated user has one of the allowed roles
exports.restrictTo = (...roles) => {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return next(new AppError("You do not have permission to perform this action.", 403));
    }
    next();
  };
};
```

```js
// middleware/validate.js
const { validationResult } = require("express-validator");

module.exports = (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({
      success: false,
      status:  "fail",
      message: "Validation failed",
      errors:  errors.array().map(e => ({ field: e.path, message: e.msg })),
    });
  }
  next();
};
```

```js
// middleware/errorHandler.js

const AppError = require("../utils/appError");

module.exports = (err, req, res, next) => {
  err.statusCode = err.statusCode || 500;
  err.status     = err.status     || "error";

  let error = Object.assign(Object.create(Object.getPrototypeOf(err)), err);
  error.message = err.message;

  if (err.name === "CastError")
    error = new AppError(`Invalid ${err.path}: ${err.value}`, 400);

  if (err.code === 11000) {
    const field = Object.keys(err.keyValue)[0];
    error = new AppError(`Duplicate value for field "${field}". Please use another value.`, 409);
  }

  if (err.name === "ValidationError") {
    const msgs = Object.values(err.errors).map(e => e.message);
    error = new AppError(msgs.join(". "), 400);
  }

  if (err.name === "JsonWebTokenError")
    error = new AppError("Invalid token. Please log in again.", 401);

  if (err.name === "TokenExpiredError")
    error = new AppError("Your session has expired. Please log in again.", 401);

  if (process.env.NODE_ENV === "development") {
    return res.status(error.statusCode).json({
      success: false,
      status:  error.status,
      message: error.message,
      stack:   err.stack,
    });
  }

  if (error.isOperational) {
    return res.status(error.statusCode).json({
      success: false,
      status:  error.status,
      message: error.message,
    });
  }

  console.error("UNEXPECTED ERROR:", err);
  res.status(500).json({
    success: false,
    status:  "error",
    message: "Something went wrong on our end. Please try again later.",
  });
};
```

```js
// validators/postValidator.js

const { body } = require("express-validator");

exports.createPostValidator = [
  body("title")
    .trim()
    .notEmpty().withMessage("Title is required")
    .isLength({ min: 5, max: 100 }).withMessage("Title must be 5–100 characters"),

  body("content")
    .trim()
    .notEmpty().withMessage("Content is required")
    .isLength({ min: 20 }).withMessage("Content must be at least 20 characters"),

  body("tags")
    .optional()
    .isArray().withMessage("Tags must be an array"),

  body("published")
    .optional()
    .isBoolean().withMessage("Published must be true or false"),
];

exports.updatePostValidator = [
  body("title")
    .optional()
    .trim()
    .isLength({ min: 5, max: 100 }).withMessage("Title must be 5–100 characters"),

  body("content")
    .optional()
    .trim()
    .isLength({ min: 20 }).withMessage("Content must be at least 20 characters"),
];
```

```js
// controllers/authController.js

const jwt        = require("jsonwebtoken");
const User       = require("../models/User");
const catchAsync = require("../utils/catchAsync");
const AppError   = require("../utils/appError");

// Helper: create and sign a JWT token for a user
const signToken = (userId) => {
  return jwt.sign(
    { id: userId },
    process.env.JWT_SECRET,
    { expiresIn: process.env.JWT_EXPIRES_IN }
  );
};

// POST /api/auth/register
exports.register = catchAsync(async (req, res, next) => {
  const { username, email, password } = req.body;

  const existing = await User.findOne({ email });
  if (existing) return next(new AppError("Email already registered.", 409));

  const user  = await User.create({ username, email, password });
  const token = signToken(user._id);

  res.status(201).json({
    success: true,
    message: "Account created successfully!",
    token,                // JWT — client stores this and sends with every request
    data: { user },
  });
});

// POST /api/auth/login
exports.login = catchAsync(async (req, res, next) => {
  const { email, password } = req.body;

  if (!email || !password) {
    return next(new AppError("Please provide email and password.", 400));
  }

  // Select password explicitly — it is excluded by default in the schema
  const user = await User.findOne({ email }).select("+password");

  if (!user || !(await user.correctPassword(password, user.password))) {
    return next(new AppError("Invalid email or password.", 401));
  }

  const token = signToken(user._id);

  res.status(200).json({
    success: true,
    message: `Welcome back, ${user.username}!`,
    token,
    data: { user },
  });
});
```

```js
// controllers/postController.js

const Post        = require("../models/Post");
const catchAsync  = require("../utils/catchAsync");
const AppError    = require("../utils/appError");
const APIFeatures = require("../utils/apiFeatures");

// GET /api/posts — get all published posts with filtering, sorting, pagination
exports.getAllPosts = catchAsync(async (req, res) => {
  // Only show published posts to the public
  // Admins can pass ?published=false to see drafts
  const baseFilter = req.user?.role === "admin"
    ? {}
    : { published: true };

  const features = new APIFeatures(
    Post.find(baseFilter).populate("author", "username email"),
    req.query
  ).filter().sort().limitFields().paginate();

  const [posts, total] = await Promise.all([
    features.query,
    Post.countDocuments(baseFilter),
  ]);

  res.status(200).json({
    success: true,
    total,
    count: posts.length,
    page:  parseInt(req.query.page)  || 1,
    pages: Math.ceil(total / (parseInt(req.query.limit) || 10)),
    data:  { posts },
  });
});

// GET /api/posts/:id — get single post and increment view count
exports.getPost = catchAsync(async (req, res, next) => {
  const post = await Post.findByIdAndUpdate(
    req.params.id,
    { $inc: { views: 1 } }, // atomically increment view count by 1
    { new: true }
  ).populate("author", "username");

  if (!post) return next(new AppError("Post not found.", 404));

  res.status(200).json({
    success: true,
    data:    { post },
  });
});

// POST /api/posts — create a post (authenticated users only)
exports.createPost = catchAsync(async (req, res) => {
  const post = await Post.create({
    ...req.body,
    author: req.user._id, // always use the authenticated user, never trust client-sent author
  });

  res.status(201).json({
    success: true,
    message: "Post created!",
    data:    { post },
  });
});

// PUT /api/posts/:id — update post (only by the author or admin)
exports.updatePost = catchAsync(async (req, res, next) => {
  const post = await Post.findById(req.params.id);

  if (!post) return next(new AppError("Post not found.", 404));

  // Authorization: only the author or an admin can edit the post
  if (post.author.toString() !== req.user._id.toString() && req.user.role !== "admin") {
    return next(new AppError("You can only edit your own posts.", 403));
  }

  // Merge existing post with updates
  Object.assign(post, req.body);
  await post.save();

  res.status(200).json({
    success: true,
    message: "Post updated!",
    data:    { post },
  });
});

// DELETE /api/posts/:id — delete post (only by the author or admin)
exports.deletePost = catchAsync(async (req, res, next) => {
  const post = await Post.findById(req.params.id);

  if (!post) return next(new AppError("Post not found.", 404));

  if (post.author.toString() !== req.user._id.toString() && req.user.role !== "admin") {
    return next(new AppError("You can only delete your own posts.", 403));
  }

  await post.deleteOne();

  // 204: success, no content to send back
  res.status(204).json({ success: true, data: null });
});

// GET /api/posts/my-posts — get all posts by the logged-in user
exports.getMyPosts = catchAsync(async (req, res) => {
  const posts = await Post.find({ author: req.user._id }).sort("-createdAt");

  res.status(200).json({
    success: true,
    count:   posts.length,
    data:    { posts },
  });
});
```

```js
// routes/authRoutes.js

const express        = require("express");
const router         = express.Router();
const authController = require("../controllers/authController");
const { body }       = require("express-validator");
const validate       = require("../middleware/validate");

const registerValidator = [
  body("username").trim().notEmpty().isLength({ min: 3 }),
  body("email").isEmail().normalizeEmail(),
  body("password").isStrongPassword({ minLength: 8, minLowercase: 1, minUppercase: 1, minNumbers: 1, minSymbols: 1 })
    .withMessage("Password must be 8+ chars with upper, lower, number, and symbol"),
];

router.post("/register", registerValidator, validate, authController.register);
router.post("/login",    authController.login);

module.exports = router;
```

```js
// routes/postRoutes.js

const express         = require("express");
const router          = express.Router();
const postController  = require("../controllers/postController");
const { protect, restrictTo } = require("../middleware/auth");
const { createPostValidator, updatePostValidator } = require("../validators/postValidator");
const validate        = require("../middleware/validate");

// Public: anyone can read published posts
router.get("/",    postController.getAllPosts);
router.get("/:id", postController.getPost);

// From here on, user must be logged in
router.use(protect);

router.post("/",         createPostValidator, validate, postController.createPost);
router.get("/my-posts",  postController.getMyPosts);
router.put("/:id",       updatePostValidator, validate, postController.updatePost);
router.delete("/:id",    postController.deletePost);

// Admin only
router.delete("/admin/:id", restrictTo("admin"), postController.deletePost);

module.exports = router;
```

```js
// app.js — app setup (separate from server startup for testability)

const express    = require("express");
const cors       = require("cors");
const morgan     = require("morgan");
const rateLimit  = require("express-rate-limit");
const helmet     = require("helmet"); // sets secure HTTP headers
const authRoutes = require("./routes/authRoutes");
const postRoutes = require("./routes/postRoutes");
const errorHandler = require("./middleware/errorHandler");
const AppError   = require("./utils/appError");

const app = express();

// ─── Security Middleware ──────────────────────────────────────────────────────
app.use(helmet()); // sets various HTTP security headers automatically

app.use(cors({
  origin:      process.env.NODE_ENV === "production" ? "https://yourfrontend.com" : "*",
  credentials: true,
}));

// Rate limiting: 100 requests per 15 minutes per IP for all API routes
app.use("/api", rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100,
  message: { success: false, error: "Too many requests. Please slow down." },
}));

// Stricter limit on auth routes
app.use("/api/auth", rateLimit({
  windowMs: 60 * 60 * 1000,
  max: 20,
  message: { success: false, error: "Too many auth attempts. Try again in 1 hour." },
}));

// ─── Logging ──────────────────────────────────────────────────────────────────
if (process.env.NODE_ENV === "development") app.use(morgan("dev"));

// ─── Body Parsing ─────────────────────────────────────────────────────────────
app.use(express.json({ limit: "10kb" })); // limit body size to prevent large payload attacks

// ─── Routes ───────────────────────────────────────────────────────────────────
app.use("/api/auth",  authRoutes);
app.use("/api/posts", postRoutes);

// Health check endpoint — useful for deployment platforms and monitoring
app.get("/api/health", (req, res) => {
  res.status(200).json({ success: true, status: "API is running" });
});

// Handle unknown routes (must come after all route definitions)
app.all("*", (req, res, next) => {
  next(new AppError(`Route ${req.method} ${req.originalUrl} not found.`, 404));
});

// Global error handler — must be last
app.use(errorHandler);

module.exports = app;
```

```js
// server.js — entry point

require("dotenv").config();
const app       = require("./app");
const connectDB = require("./config/db");

const PORT = process.env.PORT || 3000;

connectDB().then(() => {
  app.listen(PORT, () => {
    console.log(`Server running in ${process.env.NODE_ENV} mode on port ${PORT}`);
    console.log("");
    console.log("API Endpoints:");
    console.log("  POST   /api/auth/register  → Register a new account");
    console.log("  POST   /api/auth/login     → Login and get JWT token");
    console.log("  GET    /api/posts          → Get all published posts");
    console.log("  GET    /api/posts/:id      → Get a single post");
    console.log("  POST   /api/posts          → Create a post (auth required)");
    console.log("  PUT    /api/posts/:id      → Update a post (author/admin only)");
    console.log("  DELETE /api/posts/:id      → Delete a post (author/admin only)");
    console.log("  GET    /api/posts/my-posts → Get your own posts (auth required)");
    console.log("  GET    /api/health         → Health check");
  });
});
```

---

## 15. API Endpoints Summary

|Method|Endpoint|Auth Required|Description|
|---|---|---|---|
|POST|/api/auth/register|No|Create a new account|
|POST|/api/auth/login|No|Login, receive JWT|
|GET|/api/posts|No|Get all published posts (supports filtering/sorting/pagination)|
|GET|/api/posts/:id|No|Get a single post, increments view count|
|POST|/api/posts|Yes|Create a new post|
|GET|/api/posts/my-posts|Yes|Get all your own posts|
|PUT|/api/posts/:id|Yes (author/admin)|Update a post|
|DELETE|/api/posts/:id|Yes (author/admin)|Delete a post|
|GET|/api/health|No|Health check|

---

## 16. Sample API Requests

These are the requests you would make to test this API in Postman or via curl:

```
# Register
POST /api/auth/register
Content-Type: application/json
{ "username": "pranav", "email": "p@gmail.com", "password": "Secret@123" }

# Login → returns a token
POST /api/auth/login
Content-Type: application/json
{ "email": "p@gmail.com", "password": "Secret@123" }

# Get all posts (public)
GET /api/posts

# Get posts with filters (public)
GET /api/posts?sort=-views&limit=5&page=1

# Create a post (copy token from login response)
POST /api/posts
Authorization: Bearer <your-token>
Content-Type: application/json
{ "title": "My First Post", "content": "This is the content of my first blog post on this platform.", "tags": ["express", "nodejs"], "published": true }

# Update a post
PUT /api/posts/<post-id>
Authorization: Bearer <your-token>
Content-Type: application/json
{ "title": "Updated Title" }

# Delete a post
DELETE /api/posts/<post-id>
Authorization: Bearer <your-token>
```

---

> ✅ **You now have a thorough, production-aware understanding of Express API development — from the theory of REST to a complete layered architecture with controllers, routes, models, custom error handling, JWT auth, pagination, rate limiting, and CORS. The architecture in the real-world example is exactly what you would use as the foundation of Readnest or any other serious project.**
> 
> **The natural next topics to deepen this foundation are: MongoDB aggregation pipelines (for complex queries like "most viewed posts this week"), Redis caching (to cache frequently-read posts and avoid hitting the database on every request), and API documentation with Swagger/OpenAPI (so other developers can understand and use your API without reading the source code).**