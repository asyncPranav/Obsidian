
---


> **Goal of Step 5:** Set up the infrastructure required for session-based authentication.  
> We are **not authenticating users yet** and **not creating sessions during login yet**.

---

## 1. Why do we need session infrastructure?

After our current login implementation:

```text
POST /api/auth/login
        ↓
Find user
        ↓
bcrypt.compare()
        ↓
Password correct
        ↓
200 Login successful
```

the request ends.

The server does **not remember** that the user successfully logged in.

We need a mechanism that allows the server to remember:

```text
"This client is authenticated as User X."
```

That's what sessions are for.

---

# 2. Session vs Cookie

These are **not the same thing**.

### Session

The session is server-side information.

Conceptually:

```text
Session Store

sessionId: abc123
userId:    64f...
```

It tells the server:

```text
abc123 → User X
```

### Cookie

The cookie is client-side information that helps identify the session.

Conceptually:

```text
Browser

sessionId=abc123
```

So the relationship is:

```text
Cookie
   ↓
Session ID
   ↓
Session Store
   ↓
User ID
   ↓
User
```

### Important

Don't think:

```text
Cookie = Session
```

Instead:

```text
Cookie → identifies the session
Session → maintains server-side session information
```

---

# 3. Packages we installed

```bash
npm install express-session connect-mongo
```

### `express-session`

Provides session management for Express.

It gives requests access to:

```js
req.session
```

### `connect-mongo`

Allows session data to be stored in MongoDB instead of only in the Node.js process memory.

Our architecture:

```text
Express
   ↓
express-session
   ↓
connect-mongo
   ↓
MongoDB
```

---

# 4. Why session configuration belongs in middleware

We already have:

```text
src/
└── middlewares/
```

So we created:

```text
src/middlewares/session.middleware.js
```

The responsibility is:

```text
session.middleware.js
        ↓
Configure session management
```

While `app.js` is responsible for registering middleware in the correct order.

Remember:

> **Middleware implementation → `middlewares/`**

> **Middleware registration/order → `app.js`**

---

# 5. `session.middleware.js`

Our file:

```js
import session from "express-session";
import MongoStore from "connect-mongo";

const sessionMiddleware = session({
  secret: process.env.SESSION_SECRET,

  resave: false,

  saveUninitialized: false,

  store: MongoStore.create({
    mongoUrl: process.env.MONGO_URI,
  }),

  cookie: {
    httpOnly: true,
    secure: false,
    maxAge: 1000 * 60 * 60 * 24,
  },
});

export default sessionMiddleware;
```

Let's understand every option.

---

# 6. `secret`

```js
secret: process.env.SESSION_SECRET
```

We add this to `.env`:

```env
SESSION_SECRET=some-long-random-secret
```

The secret is used by `express-session` to cryptographically sign the session ID cookie.

Conceptually:

```text
SESSION_SECRET
       ↓
express-session
       ↓
protect session cookie integrity
```

### Important

Never hardcode sensitive secrets in source code.

Don't commit `.env` to GitHub.

---

# 7. `resave`

```js
resave: false
```

Means:

> Don't save the session back to the session store on every request if it hasn't changed.

Imagine:

```text
GET /api/tasks
GET /api/tasks
GET /api/users
```

If the session hasn't changed, we don't need to repeatedly rewrite it.

Therefore:

```js
resave: false
```

---

# 8. `saveUninitialized`

```js
saveUninitialized: false
```

Means:

> Don't save a session that hasn't been initialized with meaningful data.

For example, someone simply makes:

```text
GET /api/users
```

before authentication.

We don't want to create pointless sessions like:

```text
Session #1 → nothing
Session #2 → nothing
Session #3 → nothing
```

Later, when login actually initializes a session:

```js
req.session.userId = user._id;
```

the session becomes meaningful and can be stored.

---

# 9. `store`

```js
store: MongoStore.create({
  mongoUrl: process.env.MONGO_URI,
})
```

This tells `express-session`:

> Store session data in MongoDB.

Without a persistent session store, the default session storage is memory-based, which is not appropriate for a real production application.

Our MongoDB structure will conceptually become:

```text
MongoDB
│
├── users
│
├── tasks
│
└── sessions
```

So:

```text
User data       → users
Task data       → tasks
Session data    → sessions
```

---

# 10. `cookie`

```js
cookie: {
  httpOnly: true,
  secure: false,
  maxAge: 1000 * 60 * 60 * 24,
}
```

This controls the session cookie.

---

## `httpOnly`

```js
httpOnly: true
```

The browser won't allow normal JavaScript to directly access this cookie.

For example:

```js
document.cookie
```

can't read an `HttpOnly` cookie.

This helps reduce the risk of client-side JavaScript stealing the session cookie.

---

## `secure`

For local development:

```js
secure: false
```

because we're using something like:

```text
http://localhost:5000
```

When deployed behind HTTPS, we'll normally use:

```js
secure: true
```

We'll revisit this when we deal with production deployment.

---

## `maxAge`

```js
maxAge: 1000 * 60 * 60 * 24
```

Breakdown:

```text
1000 milliseconds
× 60 seconds
× 60 minutes
× 24 hours
```

Therefore:

```text
24 hours
```

The browser-side cookie is configured to last for 24 hours.

We'll later discuss session expiration in more detail.

---

# 11. Registering the middleware in `app.js`

Import:

```js
import sessionMiddleware from "./middlewares/session.middleware.js";
```

Then:

```js
app.use(sessionMiddleware);
```

The important part is **where** we place it.

Your pipeline should currently look like:

```text
express.json()
      ↓
express.urlencoded()
      ↓
sessionMiddleware
      ↓
routes
      ↓
notFound
      ↓
errorHandler
```

Example:

```js
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

app.use(sessionMiddleware);

app.use("/api/users", userRouter);
app.use("/api/tasks", taskRouter);
app.use("/api/auth", authRouter);

app.use(notFound);
app.use(errorHandler);
```

---

# 12. Why must session middleware come before routes?

Suppose we have:

```text
GET /api/tasks
       ↓
task controller
```

Eventually the task controller may need:

```js
req.session
```

Therefore `sessionMiddleware` must execute first.

Correct:

```text
Request
   ↓
sessionMiddleware
   ↓
req.session available
   ↓
router
   ↓
controller
```

Incorrect:

```text
Request
   ↓
router
   ↓
controller
   ↓
sessionMiddleware ❌
```

The controller would already have executed before the session middleware.

---

# 13. What does `express-session` give us?

Once this middleware runs:

```js
app.use(sessionMiddleware);
```

requests get access to:

```js
req.session
```

Conceptually:

```text
Request
   │
   ▼
sessionMiddleware
   │
   ▼
req.session
```

Later, during login, we'll do:

```js
req.session.userId = user._id;
```

Conceptually:

```text
req.session
      │
      └── userId
             ↓
          User ID
```

---

# 14. What we have NOT done yet

This step **doesn't mean the user is authenticated**.

We've only installed the infrastructure.

Currently:

```text
Registration
    ↓
bcrypt hash
    ↓
MongoDB
    ↓
✅ Complete


Login
    ↓
bcrypt.compare()
    ↓
credentials verified
    ↓
200 response
    ↓
❌ No authenticated session yet
```

The next step will change that.

---

# 15. What we'll do in Step 6

We'll modify our successful login flow:

```text
POST /api/auth/login
        ↓
Find user
        ↓
bcrypt.compare()
        ↓
Password correct?
        ↓
YES
        ↓
req.session.userId = user._id
        ↓
Session gets stored
        ↓
Session cookie sent to client
```

Then subsequent requests can carry that cookie:

```text
Client
   ↓
Cookie: session ID
   ↓
Server
   ↓
Session store
   ↓
User ID
   ↓
Authenticated user
```

That's when our API actually starts behaving like a **session-authenticated API**.

---

# Step 5 Checklist

```text
PHASE 2 — STEP 5

[✓] Understand why sessions are needed
[✓] Understand session vs cookie
[✓] Install express-session
[✓] Install connect-mongo
[✓] Create session.middleware.js
[✓] Configure SESSION_SECRET
[✓] Understand resave
[✓] Understand saveUninitialized
[✓] Configure MongoDB session store
[✓] Configure cookie
[✓] Understand httpOnly
[✓] Understand secure
[✓] Understand maxAge
[✓] Register middleware in app.js
[✓] Understand middleware ordering

Next:
[ ] Step 6 — Create a session during login
[ ] Send session cookie
[ ] Inspect session in MongoDB
[ ] Test session behavior with Postman
```

### The one sentence you should remember

> **`express-session` gives us `req.session`; `connect-mongo` persists that session in MongoDB; the cookie lets the client identify which session belongs to it.**

**Step 6 is where things get interesting:** we'll take your already-working `login` controller and make a successful login actually create a session.