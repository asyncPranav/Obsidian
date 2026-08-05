
---

Absolutely. Below is a **beginner-friendly JWT authentication chapter for Express.js**, starting from zero and gradually building toward a complete working implementation.

# JWT Authentication in Express.js — Complete Beginner Notes

## 1. What are we trying to build?

Imagine we have an application where users can:

```text
Register
   ↓
Login
   ↓
Receive JWT
   ↓
Access protected routes
   ↓
Logout
```

For example:

```text
POST /api/users/register
POST /api/users/login
GET  /api/students
POST /api/students
DELETE /api/students/:id
```

We want:

- Anyone can register.
    
- Anyone can login with valid credentials.
    
- Only authenticated users can access students.
    
- The server can identify which user made the request.
    

JWT helps us accomplish this.

---

# 2. What is Authentication?

**Authentication = "Who are you?"**

Suppose you enter:

```text
username: rahul
password: 123456
```

The server checks whether those credentials belong to a registered user.

If they are correct:

```text
You are Rahul ✅
```

If they're incorrect:

```text
I don't know who you are ❌
```

That's authentication.

---

# 3. What is Authorization?

Authorization is different.

**Authorization = "What are you allowed to do?"**

For example:

```text
Authentication:
Are you logged in?
        ↓
YES

Authorization:
Can you delete this student?
        ↓
YES / NO
```

So:

```text
Authentication → Who are you?
Authorization  → What can you do?
```

JWT is commonly used to help with authentication.

---

# 4. What is JWT?

JWT stands for:

**JSON Web Token**

A JWT is a string that can carry information between the client and server.

It looks roughly like:

```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJ1c2VySWQiOiI2OGFiYyIsInVzZXJuYW1lIjoicmFodWwifQ.
abc123signature
```

You don't normally manually read or modify this string.

---

# 5. Why do we need JWT?

Imagine a user logs in:

```text
username: rahul
password: 123456
```

The server verifies:

```text
Username exists? ✅
Password correct? ✅
```

Now the user wants to access:

```text
GET /api/students
```

How does the server know that the user already logged in?

One solution is:

> Send a JWT with every protected request.

So:

```text
LOGIN
   ↓
Server verifies username/password
   ↓
Server creates JWT
   ↓
Client receives JWT
   ↓
Client sends JWT with future requests
   ↓
Server verifies JWT
   ↓
Request allowed
```

---

# 6. JWT Authentication Flow

This is the most important diagram to understand.

```text
                   REGISTER
                      │
                      ▼
             Save user in database
                      │
                      ▼
                    LOGIN
                      │
                      ▼
             Check username/password
                      │
                ┌─────┴─────┐
                │           │
             Invalid       Valid
                │           │
                ▼           ▼
              401       jwt.sign()
                            │
                            ▼
                         JWT Token
                            │
                            ▼
                     Send token to
                        frontend
                            │
                            ▼
                  Frontend stores token
                            │
                            ▼
              Request protected route
                            │
                            ▼
             Authorization: Bearer TOKEN
                            │
                            ▼
                     auth middleware
                            │
                            ▼
                      jwt.verify()
                            │
                   ┌────────┴────────┐
                   │                 │
                Invalid             Valid
                   │                 │
                   ▼                 ▼
                  401             req.user
                                     │
                                     ▼
                                   next()
                                     │
                                     ▼
                              Protected route
```

If you understand this diagram, you understand the basic JWT architecture.

---

# 7. JWT has three parts

A JWT consists of:

```text
HEADER.PAYLOAD.SIGNATURE
```

For example:

```text
xxxxx.yyyyy.zzzzz
```

There are three pieces:

```text
1. Header
2. Payload
3. Signature
```

---

# 8. JWT Header

The header describes the token.

It may contain:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

Meaning:

```text
alg → algorithm
typ → token type
```

You generally don't manually create this when using `jsonwebtoken`.

The library handles it.

---

# 9. JWT Payload

The payload contains information called **claims**.

For example:

```json
{
  "userId": "123",
  "username": "rahul"
}
```

You create this when signing:

```js
const token = jwt.sign(
  {
    userId: user._id,
    username: user.username
  },
  process.env.JWT_SECRET
);
```

You can think of this as:

```text
JWT Payload
     ↓
{
   userId: "123",
   username: "rahul"
}
```

---

# 10. Important: Don't put passwords in JWT

Never do:

```js
jwt.sign({
  username: user.username,
  password: user.password
}, ...)
```

Don't put sensitive information into JWT payloads.

A JWT payload should generally contain identifiers and other non-sensitive claims, such as:

```js
{
  userId: user._id,
  username: user.username
}
```

---

# 11. Is JWT encrypted?

This is a very important beginner question.

**Normally, JWT payloads are encoded, not encrypted.**

Therefore, don't assume this is secret:

```js
{
  username: "rahul",
  userId: "123"
}
```

Someone who has the token can decode the payload.

The signature is what helps the server detect whether the token was altered.

So:

```text
JWT ≠ encrypted password container
```

---

# 12. JWT Signature

The signature is used to verify that the token hasn't been tampered with.

The server has a secret:

```env
JWT_SECRET=my-super-secret-key
```

When creating a token:

```js
jwt.sign(payload, JWT_SECRET)
```

The library generates a signature.

Later:

```js
jwt.verify(token, JWT_SECRET)
```

checks the signature.

If someone changes the payload:

```text
userId: 123
```

to:

```text
userId: 999
```

without knowing the secret, verification fails.

---

# 13. What is `JWT_SECRET`?

You usually store the secret in `.env`:

```env
JWT_SECRET=my-super-secret-key
```

Then:

```js
process.env.JWT_SECRET
```

gets that value.

Example:

```js
const token = jwt.sign(
  { userId: user._id },
  process.env.JWT_SECRET
);
```

And later:

```js
jwt.verify(
  token,
  process.env.JWT_SECRET
);
```

The same secret must be used.

---

# 14. Never hardcode your secret

Don't do:

```js
const secret = "hello123";
```

Instead:

```env
JWT_SECRET=some-long-random-secret
```

and:

```js
process.env.JWT_SECRET
```

Also don't commit your `.env` file to Git.

Your `.gitignore` should usually include:

```text
.env
```

---

# 15. Installing JWT

For Node/Express:

```bash
npm install jsonwebtoken
```

Then:

```js
const jwt = require("jsonwebtoken");
```

---

# 16. The two JWT functions you must know

The two most important functions are:

```js
jwt.sign()
```

and:

```js
jwt.verify()
```

Think:

```text
sign   → CREATE
verify → CHECK
```

---

# 17. `jwt.sign()`

Example:

```js
const token = jwt.sign(
  { userId: user._id },
  process.env.JWT_SECRET
);
```

This creates a JWT.

You can think:

```text
Payload
   +
Secret
   ↓
jwt.sign()
   ↓
JWT
```

---

# 18. `jwt.verify()`

Later:

```js
const user = jwt.verify(
  token,
  process.env.JWT_SECRET
);
```

This checks:

- Is the token correctly signed?
    
- Was it modified?
    
- Has it expired?
    
- Is the signature valid?
    

If valid, it returns the payload.

---

# 19. `expiresIn`

You can set token expiration:

```js
const token = jwt.sign(
  {
    userId: user._id
  },
  process.env.JWT_SECRET,
  {
    expiresIn: "1h"
  }
);
```

Now the token expires after one hour.

Examples:

```js
expiresIn: "1h"
expiresIn: "30m"
expiresIn: "7d"
expiresIn: "15s"
```

---

# 20. What happens after token expiration?

Suppose:

```text
Token expires in 1 hour
```

After one hour:

```js
jwt.verify(token, secret)
```

throws an error.

Therefore your middleware should catch it:

```js
try {
  const user = jwt.verify(token, process.env.JWT_SECRET);
} catch (error) {
  return res.status(401).json({
    message: "Invalid or expired token"
  });
}
```

---

# 21. Registration

Now let's build the authentication system.

A user registers:

```http
POST /api/users/register
```

Request:

```json
{
  "username": "rahul",
  "email": "rahul@gmail.com",
  "password": "123456"
}
```

The server should:

```text
Receive data
   ↓
Check whether user exists
   ↓
Hash password
   ↓
Save user
   ↓
Return success
```

---

# 22. Why hash passwords?

Never store:

```text
123456
```

directly in your database.

If your database contains:

```json
{
  "username": "rahul",
  "password": "123456"
}
```

that's dangerous.

Instead use bcrypt.

Install:

```bash
npm install bcryptjs
```

Then:

```js
const bcrypt = require("bcryptjs");
```

---

# 23. Hashing a password

```js
const hashedPassword = await bcrypt.hash(password, 10);
```

If password is:

```text
123456
```

bcrypt produces something like:

```text
$2b$10$xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

You store the hash.

Not the original password.

---

# 24. Why can we compare passwords later?

During login:

```js
const isPasswordValid = await bcrypt.compare(
  password,
  user.password
);
```

bcrypt takes:

```text
User's entered password
        +
Stored hash
        ↓
bcrypt.compare()
        ↓
true / false
```

So:

```js
if (!isPasswordValid) {
  return res.status(401).json({
    message: "Invalid username or password"
  });
}
```

---

# 25. Complete registration example

```js
router.post("/register", async (req, res) => {
  try {
    const { username, email, password } = req.body;

    const existingUser = await User.findOne({
      $or: [
        { username },
        { email }
      ]
    });

    if (existingUser) {
      return res.status(400).json({
        message: "Username or email already exists"
      });
    }

    const hashedPassword = await bcrypt.hash(password, 10);

    const user = new User({
      username,
      email,
      password: hashedPassword
    });

    await user.save();

    return res.status(201).json({
      message: "User registered successfully"
    });

  } catch (error) {
    return res.status(500).json({
      message: "Server error"
    });
  }
});
```

---

# 26. Why do we check existing users?

Suppose Rahul already exists.

If he registers again:

```json
{
  "username": "rahul",
  "email": "rahul@gmail.com"
}
```

we don't want duplicate accounts.

So:

```js
const existingUser = await User.findOne({
  $or: [
    { username },
    { email }
  ]
});
```

means:

> Find a user whose username OR email matches.

---

# 27. Login

Now the user logs in:

```http
POST /api/users/login
```

Body:

```json
{
  "username": "rahul",
  "password": "123456"
}
```

Server:

```text
Find username
    ↓
Check password
    ↓
Correct?
    ↓
jwt.sign()
    ↓
Return token
```

---

# 28. Complete login example

```js
router.post("/login", async (req, res) => {
  try {
    const { username, password } = req.body;

    const user = await User.findOne({ username });

    if (!user) {
      return res.status(401).json({
        message: "Invalid username or password"
      });
    }

    const isPasswordValid = await bcrypt.compare(
      password,
      user.password
    );

    if (!isPasswordValid) {
      return res.status(401).json({
        message: "Invalid username or password"
      });
    }

    const token = jwt.sign(
      {
        userId: user._id,
        username: user.username
      },
      process.env.JWT_SECRET,
      {
        expiresIn: "1h"
      }
    );

    return res.status(200).json({
      message: "Login successful",
      token
    });

  } catch (error) {
    return res.status(500).json({
      message: "Server error"
    });
  }
});
```

---

# 29. What exactly happens during login?

Let's say the database contains:

```json
{
  "_id": "abc123",
  "username": "rahul",
  "email": "rahul@gmail.com",
  "password": "$2b$10$..."
}
```

User sends:

```json
{
  "username": "rahul",
  "password": "123456"
}
```

Server:

```js
const user = await User.findOne({ username });
```

gets the user.

Then:

```js
bcrypt.compare("123456", user.password);
```

returns:

```js
true
```

Then:

```js
jwt.sign(
  {
    userId: user._id,
    username: user.username
  },
  process.env.JWT_SECRET,
  {
    expiresIn: "1h"
  }
);
```

creates:

```text
JWT TOKEN
```

Server returns it.

---

# 30. What does the frontend do with the JWT?

The frontend receives:

```json
{
  "message": "Login successful",
  "token": "eyJhbGciOi..."
}
```

It needs to keep the token somewhere so it can send it with future requests.

Common approaches include:

- HttpOnly secure cookies
    
- in-memory storage
    
- browser storage such as `localStorage` in some architectures
    

For learning JWT, you may see:

```js
localStorage.setItem("token", token);
```

Then:

```js
const token = localStorage.getItem("token");
```

Security-sensitive production applications should carefully consider token storage and cookie/XSS/CSRF tradeoffs rather than blindly copying this pattern.

---

# 31. How does the client send JWT?

The most common header format is:

```http
Authorization: Bearer TOKEN
```

Example:

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

There are two pieces:

```text
Bearer
TOKEN
```

---

# 32. What is `Bearer`?

`Bearer` is the authentication scheme.

Think:

```text
Authorization: Bearer <token>
```

It basically tells the server:

> "I'm presenting this token as my authentication credential."

---

# 33. Express request headers

In Express:

```js
req.headers
```

contains HTTP request headers.

So:

```js
req.headers.authorization
```

could contain:

```text
Bearer eyJhbGciOi...
```

---

# 34. JWT Middleware

Now we need middleware that checks the token.

```js
const jwt = require("jsonwebtoken");

const auth = (req, res, next) => {
  try {
    const authHeader = req.headers.authorization;

    if (!authHeader) {
      return res.status(401).json({
        message: "No token provided"
      });
    }

    const token = authHeader.split(" ")[1];

    const user = jwt.verify(
      token,
      process.env.JWT_SECRET
    );

    req.user = user;

    next();

  } catch (error) {
    return res.status(401).json({
      message: "Invalid or expired token"
    });
  }
};

module.exports = auth;
```

---

# 35. Understand `authHeader`

Suppose Postman sends:

```http
Authorization: Bearer ABC123
```

Then:

```js
const authHeader = req.headers.authorization;
```

gives:

```text
"Bearer ABC123"
```

---

# 36. Understand `.split(" ")`

This:

```js
authHeader.split(" ")
```

takes:

```text
Bearer ABC123
```

and produces:

```js
[
  "Bearer",
  "ABC123"
]
```

Then:

```js
authHeader.split(" ")[1]
```

gives:

```text
ABC123
```

That's the token.

---

# 37. Why was your earlier code failing?

You had:

```js
if (typeof bearerHeader != undefined) {
```

This is incorrect.

Use:

```js
if (!bearerHeader) {
  return res.status(401).json({
    message: "No token provided"
  });
}
```

Otherwise:

```js
bearerHeader.split(" ")
```

can run when `bearerHeader` is undefined.

Then JavaScript complains:

```text
Cannot read properties of undefined
(reading 'split')
```

---

# 38. Why do we write `req.user = user`?

This is one of the most important concepts.

After:

```js
const user = jwt.verify(
  token,
  process.env.JWT_SECRET
);
```

we get something like:

```js
{
  userId: "123",
  username: "rahul",
  iat: 1750000000,
  exp: 1750003600
}
```

We do:

```js
req.user = user;
```

Now the next route can access it.

---

# 39. Why can't the next route access `user` directly?

Because this:

```js
const user = jwt.verify(...);
```

creates a local variable inside the middleware.

The next route doesn't have direct access to that variable.

So:

```js
req.user = user;
```

puts the information onto the request object.

Then:

```js
next();
```

passes the request to the next handler.

---

# 40. Example of `req.user`

Middleware:

```js
const auth = (req, res, next) => {

  const user = jwt.verify(
    token,
    process.env.JWT_SECRET
  );

  req.user = user;

  next();
};
```

Protected route:

```js
router.get("/profile", (req, res) => {

  console.log(req.user);

  res.json({
    user: req.user
  });

});
```

The route might receive:

```js
{
  userId: "123",
  username: "rahul"
}
```

---

# 41. What is `next()`?

`next()` tells Express:

> "My middleware is finished. Continue to the next middleware/route."

Without:

```js
next();
```

the request may never reach the route.

For example:

```js
app.use(auth);

app.get("/profile", (req, res) => {
  res.json({
    message: "Profile"
  });
});
```

Flow:

```text
Request
   ↓
auth
   ↓
JWT valid?
   ↓
next()
   ↓
/profile
```

---

# 42. What happens if token is invalid?

Suppose:

```text
Authorization: Bearer fake-token
```

Then:

```js
jwt.verify(fakeToken, secret)
```

throws an error.

The `catch` runs:

```js
catch (error) {
  return res.status(401).json({
    message: "Invalid or expired token"
  });
}
```

The protected route never executes.

---

# 43. HTTP status codes

For JWT authentication, you'll commonly see:

### `200 OK`

Request succeeded.

```js
res.status(200)
```

### `201 Created`

Resource successfully created.

```js
res.status(201)
```

### `400 Bad Request`

Request data is invalid.

### `401 Unauthorized`

Authentication is missing or invalid.

For example:

```text
No token
Invalid credentials
Invalid JWT
Expired JWT
```

### `403 Forbidden`

Often used when authentication succeeded but the user is not allowed to perform an action.

A common design is:

```text
401 → You're not authenticated.
403 → You're authenticated, but not allowed.
```

Different APIs may make slightly different choices, but consistency matters.

---

# 44. Applying middleware to routes

You can protect a specific route:

```js
router.get("/profile", auth, (req, res) => {
  res.json({
    user: req.user
  });
});
```

Only `/profile` requires authentication.

---

# 45. Protecting an entire router

You can do:

```js
app.use("/api/students", auth, studentsRouter);
```

Now every student route requires authentication.

For example:

```text
GET    /api/students
POST   /api/students
PUT    /api/students/:id
DELETE /api/students/:id
```

all require JWT.

---

# 46. Protecting routes using `app.use(auth)`

You used:

```js
app.use("/api/users", usersRouter);

app.use(auth);

app.use("/api/students", studentsRouter);
```

This means:

```text
/api/users/*
```

comes before `auth`.

So user routes are public.

Then:

```js
app.use(auth);
```

starts authentication.

Everything after it is protected unless you deliberately bypass/restructure it.

---

# 47. Why route order matters

Express processes middleware from top to bottom.

Consider:

```js
app.use(auth);

app.use("/api/users", usersRouter);
```

Now:

```text
POST /api/users/register
```

would hit:

```text
auth
```

first.

But a new user doesn't have a JWT yet!

So registration would fail.

That's why your structure is better:

```js
app.use("/api/users", usersRouter);

app.use(auth);

app.use("/api/students", studentsRouter);
```

---

# 48. Complete architecture

A clean project could look like:

```text
project/
│
├── config/
│   └── db.js
│
├── models/
│   └── users.model.js
│
├── routes/
│   ├── users.route.js
│   └── students.route.js
│
├── middlewares/
│   └── auth.js
│
├── .env
├── .gitignore
├── index.js
└── package.json
```

---

# 49. `.env`

```env
PORT=5000
MONGO_URI=mongodb://localhost:27017/mydb
JWT_SECRET=your-long-random-secret
```

Don't commit this file.

---

# 50. User model

For MongoDB/Mongoose:

```js
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  username: {
    type: String,
    required: true,
    unique: true
  },

  email: {
    type: String,
    required: true,
    unique: true
  },

  password: {
    type: String,
    required: true
  }
});

module.exports = mongoose.model("User", userSchema);
```

---

# 51. Complete users route

```js
const express = require("express");
const router = express.Router();

const User = require("../models/users.model");
const bcrypt = require("bcryptjs");
const jwt = require("jsonwebtoken");

router.post("/register", async (req, res) => {
  try {
    const {
      username,
      email,
      password
    } = req.body;

    const existingUser = await User.findOne({
      $or: [
        { username },
        { email }
      ]
    });

    if (existingUser) {
      return res.status(400).json({
        message: "Username or email already exists"
      });
    }

    const hashedPassword = await bcrypt.hash(
      password,
      10
    );

    const user = new User({
      username,
      email,
      password: hashedPassword
    });

    await user.save();

    return res.status(201).json({
      message: "User registered successfully"
    });

  } catch (error) {
    return res.status(500).json({
      message: "Server error"
    });
  }
});


router.post("/login", async (req, res) => {
  try {

    const {
      username,
      password
    } = req.body;

    const user = await User.findOne({
      username
    });

    if (!user) {
      return res.status(401).json({
        message: "Invalid username or password"
      });
    }

    const isPasswordValid =
      await bcrypt.compare(
        password,
        user.password
      );

    if (!isPasswordValid) {
      return res.status(401).json({
        message: "Invalid username or password"
      });
    }

    const token = jwt.sign(
      {
        userId: user._id,
        username: user.username
      },
      process.env.JWT_SECRET,
      {
        expiresIn: "1h"
      }
    );

    return res.status(200).json({
      message: "Login successful",
      token
    });

  } catch (error) {

    return res.status(500).json({
      message: "Server error"
    });

  }
});


module.exports = router;
```

---

# 52. Complete `auth.js`

```js
const jwt = require("jsonwebtoken");

const auth = (req, res, next) => {

  try {

    const authHeader =
      req.headers.authorization;

    if (!authHeader) {
      return res.status(401).json({
        message: "No token provided"
      });
    }

    const parts =
      authHeader.split(" ");

    if (
      parts.length !== 2 ||
      parts[0] !== "Bearer"
    ) {
      return res.status(401).json({
        message: "Invalid authorization format"
      });
    }

    const token = parts[1];

    const user = jwt.verify(
      token,
      process.env.JWT_SECRET
    );

    req.user = user;

    next();

  } catch (error) {

    return res.status(401).json({
      message: "Invalid or expired token"
    });

  }

};

module.exports = auth;
```

This is a little more defensive than the version from your lecture.

---

# 53. Complete `index.js`

```js
require("dotenv").config();

const express = require("express");
const cors = require("cors");

const app = express();

const usersRouter =
  require("./routes/users.route");

const studentsRouter =
  require("./routes/students.route");

const auth =
  require("./middlewares/auth");

const PORT = process.env.PORT || 5000;

app.use(cors());

app.use(express.json());

app.use(express.urlencoded({
  extended: false
}));


// PUBLIC ROUTES
app.use(
  "/api/users",
  usersRouter
);


// AUTHENTICATION MIDDLEWARE
app.use(auth);


// PROTECTED ROUTES
app.use(
  "/api/students",
  studentsRouter
);


// 404
app.use((req, res) => {
  res.status(404).json({
    message: "Route not found"
  });
});


app.listen(PORT, () => {
  console.log(
    `Server running on port ${PORT}`
  );
});
```

---

# 54. Testing in Postman

This is where you should practice.

## Step 1 — Register

```text
POST
http://localhost:5000/api/users/register
```

Body → raw → JSON:

```json
{
  "username": "rahul",
  "email": "rahul@gmail.com",
  "password": "123456"
}
```

Expected:

```json
{
  "message": "User registered successfully"
}
```

---

# 55. Step 2 — Check MongoDB

Your database should contain something similar to:

```json
{
  "_id": "...",
  "username": "rahul",
  "email": "rahul@gmail.com",
  "password": "$2b$10$..."
}
```

Notice:

```text
123456
```

is NOT stored.

Instead:

```text
$2b$10$...
```

is stored.

That's bcrypt hashing.

---

# 56. Step 3 — Login

```text
POST
http://localhost:5000/api/users/login
```

Body:

```json
{
  "username": "rahul",
  "password": "123456"
}
```

Response:

```json
{
  "message": "Login successful",
  "token": "eyJhbGciOi..."
}
```

Copy the token.

---

# 57. Step 4 — Access protected route

Suppose you have:

```js
router.get("/", (req, res) => {
  res.json({
    message: "Students retrieved"
  });
});
```

Request:

```text
GET
http://localhost:5000/api/students
```

In Postman:

```text
Authorization
   ↓
Type: Bearer Token
   ↓
Token: eyJhbGciOi...
```

Send it.

The request contains:

```http
Authorization: Bearer eyJhbGciOi...
```

---

# 58. What happens internally?

Let's follow it.

Postman:

```text
GET /api/students
Authorization: Bearer TOKEN
```

Express:

```js
app.use(auth);
```

calls:

```js
auth(req, res, next)
```

Then:

```js
req.headers.authorization
```

gets:

```text
Bearer TOKEN
```

Then:

```js
authHeader.split(" ")
```

becomes:

```js
[
  "Bearer",
  "TOKEN"
]
```

Then:

```js
const token = parts[1];
```

gets:

```text
TOKEN
```

Then:

```js
jwt.verify(
  token,
  process.env.JWT_SECRET
);
```

checks it.

If valid:

```js
req.user = user;
```

Then:

```js
next();
```

Then students route executes.

---

# 59. What happens if you don't send the token?

Request:

```text
GET /api/students
```

without:

```http
Authorization: Bearer TOKEN
```

Then:

```js
req.headers.authorization
```

is:

```js
undefined
```

This condition catches it:

```js
if (!authHeader) {
  return res.status(401).json({
    message: "No token provided"
  });
}
```

Response:

```json
{
  "message": "No token provided"
}
```

The students route never runs.

---

# 60. What happens with an invalid token?

Suppose you send:

```text
Authorization: Bearer abc123
```

Then:

```js
jwt.verify("abc123", secret)
```

fails.

The `catch` executes:

```js
return res.status(401).json({
  message: "Invalid or expired token"
});
```

---

# 61. What happens with an expired token?

Suppose:

```js
expiresIn: "1h"
```

One hour later:

```js
jwt.verify(token, secret)
```

fails because the token has expired.

The middleware returns:

```json
{
  "message": "Invalid or expired token"
}
```

The user must authenticate again or use whatever refresh-token architecture your application implements.

---

# 62. Accessing the logged-in user's ID

Suppose your JWT payload is:

```js
{
  userId: "abc123",
  username: "rahul"
}
```

After:

```js
req.user = user;
```

you can do:

```js
console.log(req.user.userId);
```

or:

```js
console.log(req.user.username);
```

This is extremely useful.

---

# 63. Example: "Get my profile"

You can create:

```js
router.get("/me", auth, async (req, res) => {

  const user = await User.findById(
    req.user.userId
  );

  res.json({
    user
  });

});
```

Flow:

```text
GET /api/users/me
        ↓
auth
        ↓
jwt.verify()
        ↓
req.user.userId
        ↓
User.findById()
        ↓
MongoDB
        ↓
Return user
```

This is where `req.user` becomes very useful.

---

# 64. JWT doesn't automatically fetch the user from MongoDB

This is important.

When you do:

```js
const user = jwt.verify(
  token,
  process.env.JWT_SECRET
);
```

JWT only gives you the **payload**.

It does not automatically execute:

```js
User.findById(...)
```

If you need fresh database information:

```js
const dbUser = await User.findById(
  req.user.userId
);
```

---

# 65. JWT vs database

Think about the difference.

JWT:

```js
{
  userId: "123",
  username: "rahul"
}
```

Database:

```js
{
  _id: "123",
  username: "rahul",
  email: "rahul@gmail.com",
  password: "...",
  createdAt: "...",
  ...
}
```

JWT carries the information you chose to put into it.

MongoDB stores the complete user document.

---

# 66. Why include `userId` in JWT?

Suppose:

```js
{
  userId: "123"
}
```

Then when the user asks:

```text
GET /profile
```

you know which database user to retrieve:

```js
const user = await User.findById(
  req.user.userId
);
```

That's one of the most common JWT patterns.

---

# 67. Example: user-specific data

Imagine students belong to users.

Database:

```text
Student A → userId 123
Student B → userId 456
```

Logged-in user has:

```js
req.user.userId === "123"
```

Then:

```js
const students = await Student.find({
  userId: req.user.userId
});
```

returns only that user's students.

So JWT can help identify:

```text
Who is making this request?
```

---

# 68. Authentication vs authorization again

Suppose:

```text
Rahul is logged in.
```

Authentication:

```text
JWT valid? ✅
```

But maybe Rahul is a normal user and cannot delete students.

Authorization:

```text
Is Rahul an admin? ❌
```

Then:

```text
401 → authentication problem
403 → authorization/permission problem
```

---

# 69. Role-based authorization

You could include:

```js
const token = jwt.sign(
  {
    userId: user._id,
    username: user.username,
    role: user.role
  },
  process.env.JWT_SECRET,
  {
    expiresIn: "1h"
  }
);
```

Then middleware:

```js
const adminOnly = (req, res, next) => {

  if (req.user.role !== "admin") {
    return res.status(403).json({
      message: "Admin access required"
    });
  }

  next();
};
```

Then:

```js
router.delete(
  "/students/:id",
  auth,
  adminOnly,
  deleteStudent
);
```

Flow:

```text
Request
  ↓
JWT authentication
  ↓
Is token valid?
  ↓
YES
  ↓
Is role admin?
  ↓
YES
  ↓
Delete student
```

---

# 70. Logout with JWT

This is one of the areas beginners often misunderstand.

With a simple stateless JWT setup, the server generally doesn't have a session to destroy.

If the client has:

```text
JWT = ABC123
```

and sends:

```text
POST /logout
```

the server cannot automatically erase the copy stored in the browser.

The client needs to remove its token from wherever it stored it.

For example, if using localStorage:

```js
localStorage.removeItem("token");
```

Then future requests don't contain the JWT.

---

# 71. Why your `/logout` route doesn't really logout

Your route:

```js
router.post("/logout", async (req, res) => {
  return res.status(200).json({
    message: "Logged out successfully"
  });
});
```

only sends a message.

It doesn't invalidate the JWT.

If the token remains valid for another 50 minutes, technically the token can still authenticate requests.

Real applications can implement stronger logout/revocation mechanisms, such as:

- short-lived access tokens + refresh tokens
    
- token revocation/deny lists
    
- rotating refresh tokens
    
- secure cookie clearing
    

The right approach depends on the architecture.

---

# 72. Access token vs refresh token

For beginner JWT projects, you may initially use:

```text
Access token
```

with:

```js
expiresIn: "1h"
```

More advanced applications commonly use:

```text
Access token → short-lived
Refresh token → longer-lived
```

Example:

```text
Login
 ↓
Access token → 15 minutes
Refresh token → several days
```

When access token expires:

```text
Frontend
   ↓
Refresh token
   ↓
Server
   ↓
New access token
```

This is a more advanced topic, so don't worry about implementing it until basic JWT is clear.

---

# 73. JWT and sessions

There are two common authentication approaches:

### Session-based authentication

```text
Login
 ↓
Server creates session
 ↓
Server stores session
 ↓
Client gets session ID
 ↓
Client sends session ID
 ↓
Server looks up session
```

### JWT-based authentication

```text
Login
 ↓
Server creates JWT
 ↓
Client gets JWT
 ↓
Client sends JWT
 ↓
Server verifies JWT
```

JWT can be useful in APIs and distributed systems because the server can verify the token without needing a central session lookup for every request.

But JWT isn't automatically better than sessions. Each approach has tradeoffs.

---

# 74. Common JWT mistakes

## Mistake 1

```js
if (typeof bearerHeader != undefined)
```

❌ Wrong.

Use:

```js
if (!bearerHeader)
```

---

## Mistake 2

Putting password in JWT:

```js
{
  username,
  password
}
```

❌ Don't do this.

---

## Mistake 3

Hardcoding secret:

```js
jwt.sign(payload, "12345")
```

❌ Avoid.

Use:

```js
process.env.JWT_SECRET
```

---

## Mistake 4

Forgetting `next()`:

```js
req.user = user;
// next() missing
```

The request may hang because the middleware never passes control onward.

---

## Mistake 5

Not handling invalid JWT:

```js
jwt.verify(token, secret);
```

without proper error handling can cause unwanted errors.

Use `try/catch` or appropriate error handling.

---

## Mistake 6

Calling `.split()` before checking the header:

```js
const token =
  req.headers.authorization.split(" ")[1];
```

If no header exists:

```text
undefined.split()
```

💥 Error.

Check first.

---

## Mistake 7

Protecting registration with JWT

Wrong order:

```js
app.use(auth);
app.use("/api/users", usersRouter);
```

New users don't have a token yet.

---

# 75. The most important JWT vocabulary

|Term|Meaning|
|---|---|
|Authentication|Who are you?|
|Authorization|What are you allowed to do?|
|JWT|JSON Web Token|
|Payload|Data/claims inside token|
|Secret|Server-side key used to sign/verify|
|`jwt.sign()`|Creates JWT|
|`jwt.verify()`|Verifies JWT|
|Bearer|Authorization scheme|
|Middleware|Function that runs between request and route|
|`req`|Request object|
|`req.user`|User information attached to request|
|`next()`|Continue to next middleware/route|
|`bcrypt.hash()`|Hash password|
|`bcrypt.compare()`|Compare entered password with stored hash|
|`expiresIn`|JWT expiration time|

---

# 76. The three files you are learning

Your project can be understood as three major pieces.

### `users.route.js`

Responsible for:

```text
REGISTER
LOGIN
```

It creates JWT:

```js
jwt.sign()
```

---

### `auth.js`

Responsible for:

```text
GET TOKEN
VERIFY TOKEN
IDENTIFY USER
```

It uses:

```js
jwt.verify()
```

and:

```js
req.user = user;
```

---

### `index.js`

Responsible for:

```text
CONNECTING EVERYTHING
```

For example:

```js
app.use("/api/users", usersRouter);

app.use(auth);

app.use("/api/students", studentsRouter);
```

---

# 77. Remember this mental model

Imagine a nightclub.

### Register

You tell the club:

```text
My name is Rahul.
```

The club creates your account.

### Login

You show:

```text
Username + password
```

The club verifies you.

### JWT

The club gives you a wristband:

```text
JWT
```

### Protected route

Every time you want to enter a restricted area:

```text
Show wristband
```

The security guard:

```js
jwt.verify()
```

checks it.

If valid:

```js
next();
```

You enter.

The guard also tells the next room:

```js
req.user = user;
```

> "This is Rahul."

---

# 78. The entire system in code

The core idea can be reduced to:

### Login

```js
const token = jwt.sign(
  {
    userId: user._id
  },
  process.env.JWT_SECRET,
  {
    expiresIn: "1h"
  }
);

res.json({
  token
});
```

### Middleware

```js
const auth = (req, res, next) => {

  try {

    const authHeader =
      req.headers.authorization;

    if (!authHeader) {
      return res.status(401).json({
        message: "No token"
      });
    }

    const token =
      authHeader.split(" ")[1];

    const user =
      jwt.verify(
        token,
        process.env.JWT_SECRET
      );

    req.user = user;

    next();

  } catch (error) {

    return res.status(401).json({
      message: "Invalid token"
    });

  }
};
```

### Protected route

```js
router.get(
  "/profile",
  auth,
  (req, res) => {

    res.json({
      message: "Welcome",
      user: req.user
    });

  }
);
```

That's the heart of JWT authentication.

---

# 79. Your complete learning checklist

You should be able to explain each of these without looking at your code:

### Basics

-  What is authentication?
    
-  What is authorization?
    
-  What is JWT?
    
-  Why use JWT?
    
-  What are the three JWT parts?
    
-  What is the payload?
    
-  What is the signature?
    
-  What is the secret?
    

### Node/Express

-  What is middleware?
    
-  What is `req`?
    
-  What is `res`?
    
-  What is `next()`?
    
-  What is `req.headers.authorization`?
    

### JWT library

-  What does `jwt.sign()` do?
    
-  What does `jwt.verify()` do?
    
-  What does `expiresIn` do?
    

### Passwords

-  Why hash passwords?
    
-  What does `bcrypt.hash()` do?
    
-  What does `bcrypt.compare()` do?
    
-  Why shouldn't passwords be stored directly?
    

### Middleware

-  Why check for the Authorization header?
    
-  Why use `.split(" ")`?
    
-  Why use `[1]`?
    
-  Why use `jwt.verify()`?
    
-  Why do `req.user = user`?
    
-  Why call `next()`?
    

### Express architecture

-  Why are `/register` and `/login` public?
    
-  Why are `/students` protected?
    
-  Why does middleware order matter?
    

### Security

-  Don't put passwords in JWT.
    
-  Don't expose JWT secrets.
    
-  Don't commit `.env`.
    
-  Validate tokens before trusting their claims.
    
-  Use HTTPS in production.
    
-  Think carefully about token storage.
    
-  Understand the difference between authentication and authorization.
    

---

# 80. The one diagram I recommend memorizing

If you're learning JWT for the first time, memorize this:

```text
                    REGISTER
                       ↓
                Hash password
                       ↓
                Save in MongoDB
                       ↓
                     LOGIN
                       ↓
             Username + password
                       ↓
               bcrypt.compare()
                       ↓
                    Valid?
                       ↓
                    YES
                       ↓
                  jwt.sign()
                       ↓
                    JWT
                       ↓
              Send JWT to client
                       ↓
        ┌──────────────────────────┐
        │ Client requests protected│
        │         route             │
        └─────────────┬────────────┘
                      ↓
          Authorization: Bearer JWT
                      ↓
               auth middleware
                      ↓
                Get JWT token
                      ↓
                jwt.verify()
                      ↓
                Is JWT valid?
                 /          \
               NO            YES
               ↓              ↓
             401         req.user = user
                              ↓
                            next()
                              ↓
                       Protected route
                              ↓
                        Database query
                              ↓
                           Response
```

And remember the three lines at the center:

```js
const token = jwt.sign(payload, secret);
```

**Create token**

```js
const user = jwt.verify(token, secret);
```

**Verify token**

```js
req.user = user;
```

**Pass verified user information to the next route**

If those three lines are clear to you, you have understood the core of JWT authentication in Express.