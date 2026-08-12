
---

Absolutely. Now we move to **Phase 2 — Step 6: Create the Session During Login**.

This is the point where your API changes from:

> "I verified your password."

to:

> **"I know who you are for subsequent requests."**

---

# Phase 2 — Step 6: Create Session During Login

## 1. Where we are currently

Your login currently does:

```text
POST /api/auth/login
        ↓
Validate email/password
        ↓
Find user
        ↓
bcrypt.compare()
        ↓
Correct?
   /          \
 NO            YES
 ↓              ↓
401          200 Login successful
```

But after the `200` response:

```text
❌ Server doesn't remember the user
```

Now we're going to add:

```text
Correct password
      ↓
Create session
      ↓
Store userId inside session
      ↓
Send session cookie to client
```

---

# 2. The most important line

Inside your `login` controller, after successfully verifying the password:

```js
req.session.userId = user._id;
```

This is the key line of Step 6.

It means:

> "For this session, remember that this user is authenticated."

Conceptually:

```text
req.session
     │
     └── userId
            ↓
         64f8...
            ↓
          User
```

---

# 3. Modify your login controller

Your current login probably looks approximately like:

```js
const login = async (req, res, next) => {
  try {
    const { email, password } = req.body;

    const user = await User.findOne({ email });

    if (!user) {
      return next(new ApiError(401, "Invalid email or password"));
    }

    const isPasswordCorrect = await bcrypt.compare(
      password,
      user.password,
    );

    if (!isPasswordCorrect) {
      return next(new ApiError(401, "Invalid email or password"));
    }

    res.status(200).json({
      status: "success",
      message: "Login successful",
      data: {
        user: {
          id: user._id,
          name: user.name,
          email: user.email,
          role: user.role,
        },
      },
    });
  } catch (error) {
    next(error);
  }
};
```

Add:

```js
req.session.userId = user._id;
```

after password verification.

So:

```js
const login = async (req, res, next) => {
  try {
    const { email, password } = req.body;

    const user = await User.findOne({ email });

    if (!user) {
      return next(new ApiError(401, "Invalid email or password"));
    }

    const isPasswordCorrect = await bcrypt.compare(
      password,
      user.password,
    );

    if (!isPasswordCorrect) {
      return next(new ApiError(401, "Invalid email or password"));
    }

    // Create authenticated session
    req.session.userId = user._id;

    res.status(200).json({
      status: "success",
      message: "Login successful",
      data: {
        user: {
          id: user._id,
          name: user.name,
          email: user.email,
          role: user.role,
        },
      },
    });
  } catch (error) {
    next(error);
  }
};
```

That's it for the basic session creation.

But let's understand **exactly what just happened**.

---

# 4. What happens when login succeeds?

Suppose:

```text
User ID:
68a123...
```

The user sends:

```json
{
  "email": "pranav@example.com",
  "password": "Test@123"
}
```

Your server verifies:

```text
email
 ↓
User.findOne()
 ↓
user found
 ↓
bcrypt.compare()
 ↓
true
```

Then:

```js
req.session.userId = user._id;
```

Now conceptually:

```text
SESSION

┌──────────────────────────┐
│ sessionId: abc123        │
│ userId: 68a123...        │
└──────────────────────────┘
```

The important relationship is:

```text
sessionId
    ↓
userId
    ↓
User document
```

---

# 5. Where is this session stored?

Remember our Step 5 configuration:

```js
store: MongoStore.create({
  mongoUrl: process.env.MONGO_URI,
})
```

Therefore the session is persisted in MongoDB.

Your database should eventually have something conceptually like:

```text
MongoDB
│
├── users
│
├── tasks
│
└── sessions
```

After successful login, you should see a session document inside:

```text
sessions
```

The exact structure is managed by `express-session` / `connect-mongo`, so **don't manually create session documents yourself.**

---

# 6. What happens to the browser/Postman?

The server also sends a cookie back in the response.

Conceptually:

```text
Server
   │
   │ Set-Cookie: connect.sid=....
   ▼
Client
```

The cookie contains the session identifier.

**The cookie does not contain your password.**

And typically you should not think of it as directly containing:

```text
userId = 68a123
```

Instead:

```text
Cookie
   ↓
Session ID
   ↓
MongoDB session
   ↓
userId
```

---

# 7. Why doesn't the client send `userId`?

This is one of the biggest changes we're going to make to your Task API.

Previously you had:

```json
{
  "title": "Learn Sessions",
  "description": "Understand sessions",
  "userId": "68a123..."
}
```

That's insecure for an authenticated API.

A malicious client could simply change:

```json
"userId": "SOME_OTHER_USER"
```

and potentially create a task belonging to someone else.

Instead:

```text
Client
  ↓
"I want to create a task"
  ↓
Server checks session
  ↓
Session says userId = 68a123
  ↓
Server creates task for 68a123
```

Eventually:

```js
const task = await Task.create({
  title,
  description,
  completed,
  userId: req.user._id,
});
```

We'll get there.

---

# 8. Test it in Postman

First, restart your server if necessary.

Then:

```http
POST /api/auth/login
```

Body:

```json
{
  "email": "your-existing-user@example.com",
  "password": "your-password"
}
```

You should get:

```text
200
Login successful
```

But now we need to check something new.

---

# 9. Check MongoDB Compass

Open your database.

Previously:

```text
users
tasks
```

Now look for:

```text
sessions
```

After successful login, you should see a session document.

You don't need to understand every field yet.

The important thing is:

> **A successful login should now result in a stored session.**

---

# 10. Check Postman's cookie

After the login request, inspect Postman's cookies for your localhost domain.

You should see something related to:

```text
connect.sid
```

The exact value will be a long encoded/signed session identifier.

So now we have:

```text
POST /login
       ↓
Password verified
       ↓
req.session.userId = user._id
       ↓
MongoDB stores session
       ↓
Server sends session cookie
       ↓
Postman stores cookie
```

---

# 11. Very important: Don't manually send the cookie yet

For now, don't copy the cookie and manually put it into headers.

Postman can maintain cookies for you.

The idea we're testing is:

```text
Login
 ↓
Set-Cookie
 ↓
Postman stores it
 ↓
Next request
 ↓
Postman sends it automatically
```

We'll verify that behavior in the next step.

---

# 12. What if you login again?

Suppose you login twice.

You might see multiple session records.

That's because you can have multiple active sessions.

For example:

```text
Laptop
   ↓
Session A → User 1

Phone
   ↓
Session B → User 1
```

Both sessions can belong to the same user.

This is normal.

Later, logout will allow us to destroy a session.

---

# 13. One important security improvement — regenerate the session

For a production-quality authentication flow, we should avoid simply attaching authentication data to an existing session after login.

A stronger login pattern is:

```js
req.session.regenerate((err) => {
  if (err) return next(err);

  req.session.userId = user._id;

  req.session.save((err) => {
    if (err) return next(err);

    // send response
  });
});
```

Why?

Because regenerating the session after authentication helps protect against **session fixation**.

However, I don't want you to blindly paste this yet.

### For our learning project, first understand this basic flow:

```js
req.session.userId = user._id;
```

Once you've confirmed that session creation works, we'll improve the implementation with session regeneration and explicitly understand **why** we're doing it.

---

# 14. Current architecture

Your application now looks like:

```text
                    REQUEST
                       │
                       ▼
              express.json()
                       │
                       ▼
             sessionMiddleware
                       │
                       ▼
                   /auth/login
                       │
                       ▼
                  login()
                       │
             ┌─────────┴─────────┐
             │                   │
        Find User          bcrypt.compare()
             │                   │
             └─────────┬─────────┘
                       │
                  Credentials
                    correct
                       │
                       ▼
             req.session.userId
                       │
                       ▼
                connect-mongo
                       │
                       ▼
                   MongoDB
                       │
                       ▼
                 Session stored
                       │
                       ▼
                Session Cookie
                       │
                       ▼
                    Client
```

---

# 15. What we've achieved

Before Step 6:

```text
Register
   ↓
Hash password
   ↓
MongoDB

Login
   ↓
Verify password
   ↓
Response
```

After Step 6:

```text
Register
   ↓
Hash password
   ↓
MongoDB

Login
   ↓
Verify password
   ↓
Create session
   ↓
Store userId in session
   ↓
MongoDB
   ↓
Send session cookie
```

But we're still missing one critical piece:

```text
Session exists
     ↓
How do protected routes know
which user is logged in?
```

That's our **next step**.

---

# Step 6 Checklist

```text
PHASE 2 — STEP 6

[ ] Add req.session.userId = user._id
[ ] Login successfully
[ ] Check MongoDB sessions collection
[ ] Check Postman cookies
[ ] Understand cookie → session → userId relationship
[ ] Understand why client shouldn't provide userId
[ ] Understand why sessions are server-side
[ ] Understand basic session creation

Next:
[ ] Step 7 — Authentication middleware
[ ] Read req.session.userId
[ ] Find the authenticated user
[ ] Attach user to req.user
[ ] Protect routes
```

### The key concept from Step 6

Remember this line:

```js
req.session.userId = user._id;
```

It creates the bridge:

```text
LOGIN
  ↓
Verified User
  ↓
Session
  ↓
userId
  ↓
Future Requests
```

**Implement this, login again, verify the `sessions` collection in MongoDB and the `connect.sid` cookie in Postman. Then we'll build the authentication middleware that actually uses this session.**