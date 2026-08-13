

---

Yes. **Step 6 is done.** Now we move to **Step 7: Authentication Middleware**.

And we'll do it slowly, because this is the point where `req.session.userId` and `req.user` finally connect.

---

# Step 7 — Authentication Middleware

## 1. What are we trying to achieve?

Right now, after login, your server knows:

```js
req.session.userId
```

For example:

```text
req.session.userId
        ↓
     68abc123
```

But your controllers don't yet have an easy way to know:

> "Who is making this request?"

We want every **authenticated request** to eventually have:

```js
req.user
```

containing the actual user.

So the flow becomes:

```text
Cookie
   ↓
Session
   ↓
req.session.userId
   ↓
Find User
   ↓
req.user
```

---

# 2. Why do we need middleware?

Imagine this request:

```http
GET /api/tasks
```

Before the controller runs, we want to check:

```text
Is the user logged in?
```

That's authentication.

We don't want to write this logic inside every controller:

```js
// getAllTasks
// getTask
// createTask
// updateTask
// deleteTask
```

That would be repetitive.

Instead:

```text
Request
   ↓
authenticate middleware
   ↓
Task controller
```

The middleware handles authentication once.

---

# 3. Create the middleware file

You already have:

```text
src/
└── middlewares/
```

Create:

```text
src/middlewares/auth.middleware.js
```

For now, don't worry about authorization.

This middleware is responsible **only for authentication**.

---

# 4. First version — just check the session

Start with this:

```js
const authenticate = (req, res, next) => {
  const userId = req.session.userId;

  if (!userId) {
    return res.status(401).json({
      status: "fail",
      message: "You are not authenticated",
    });
  }

  next();
};

export default authenticate;
```

### Don't rush to copy this.

Understand what this line does:

```js
const userId = req.session.userId;
```

Suppose you logged in.

During login:

```js
req.session.userId = user._id;
```

So on a later request:

```text
req.session
    ↓
{
   userId: "68abc123"
}
```

Therefore:

```js
const userId = req.session.userId;
```

gives:

```text
68abc123
```

---

# 5. Why check `!userId`?

Suppose someone hasn't logged in.

They don't have an authenticated session.

So:

```js
req.session.userId
```

will be:

```text
undefined
```

Then:

```js
if (!userId)
```

is true.

We return:

```text
401 Unauthorized
```

Meaning:

> "You haven't authenticated yourself."

---

# 6. Why `next()`?

This is very important.

Middleware sits **between the request and controller**.

For example:

```js
router.get("/tasks", authenticate, getAllTasks);
```

The flow is:

```text
GET /tasks
    ↓
authenticate
    ↓
next()
    ↓
getAllTasks
```

If authentication fails:

```text
GET /tasks
    ↓
authenticate
    ↓
❌ 401
```

The controller doesn't execute.

---

# 7. But this middleware isn't enough yet

You might notice:

```js
const userId = req.session.userId;
```

We know **which user ID** is logged in.

But we still don't have:

```js
req.user
```

We eventually want:

```text
req.session.userId
        ↓
User.findById()
        ↓
req.user
```

So let's improve the middleware.

---

# 8. Get the actual user

Import your User model:

```js
import User from "../models/user.model.js";
import ApiError from "../utils/ApiError.js";
```

Then:

```js
const authenticate = async (req, res, next) => {
  try {
    const userId = req.session.userId;

    if (!userId) {
      return next(new ApiError(401, "You are not authenticated"));
    }

    const user = await User.findById(userId);

    if (!user) {
      return next(new ApiError(401, "User no longer exists"));
    }

    req.user = user;

    next();
  } catch (error) {
    next(error);
  }
};

export default authenticate;
```

This is the important version.

---

# 9. Understand it line by line

### Line 1

```js
const userId = req.session.userId;
```

We're asking:

> "What user ID is stored in this session?"

Example:

```text
userId = 68abc123
```

---

### Line 2

```js
if (!userId) {
```

We're asking:

> "Does this session actually contain a logged-in user?"

If no:

```text
401 Unauthorized
```

---

### Line 3

```js
const user = await User.findById(userId);
```

We have:

```text
68abc123
```

But that's only an ID.

So we ask MongoDB:

> "Give me the user whose `_id` is 68abc123."

MongoDB returns:

```js
{
  _id: "68abc123",
  name: "Pranav",
  email: "pranav@example.com",
  password: "...",
  role: "user"
}
```

---

### Line 4

```js
if (!user) {
```

What if the session says:

```text
userId = 68abc123
```

but that user was deleted from MongoDB?

Then:

```js
User.findById()
```

returns:

```text
null
```

The session is no longer useful.

So reject the request.

---

### The BIG line

```js
req.user = user;
```

This is where your earlier confusion gets resolved.

We are **creating a property ourselves**.

Express doesn't magically create `req.user`.

We're saying:

```text
"Hey request object,
keep this user information with you."
```

So now:

```text
req
│
├── session
│     └── userId: 68abc123
│
└── user
      ├── _id: 68abc123
      ├── name: Pranav
      ├── email: ...
      └── role: user
```

Then:

```js
next();
```

allows the request to continue.

---

# 10. Why is `req.user` useful?

Suppose we eventually have:

```js
const createTask = async (req, res, next) => {
```

We don't want the client to send:

```json
{
  "title": "Learn sessions",
  "description": "Understand sessions",
  "userId": "68abc123"
}
```

Instead:

```js
const task = await Task.create({
  title,
  description,
  userId: req.user._id,
});
```

Because:

```text
req.user
   ↓
authenticated user
```

The client can't choose the owner.

That's where authentication starts becoming useful for your **Task API**.

---

# 11. Let's test it BEFORE touching tasks

We don't want to immediately modify your entire Task API.

Let's create a small test route:

```http
GET /api/auth/me
```

Why?

Because we want to prove:

```text
Cookie
 ↓
Session
 ↓
userId
 ↓
User
 ↓
req.user
```

works correctly.

---

# 12. Add `/me` controller

In your auth controller:

```js
const getMe = async (req, res, next) => {
  try {
    res.status(200).json({
      status: "success",
      data: {
        user: {
          id: req.user._id,
          name: req.user.name,
          email: req.user.email,
          role: req.user.role,
        },
      },
    });
  } catch (error) {
    next(error);
  }
};
```

Then export it:

```js
export {
  register,
  login,
  getMe,
};
```

Use whatever your existing controller function names are; **don't duplicate your existing register/login code.**

---

# 13. Add the route

In your auth route:

```js
import authenticate from "../middlewares/auth.middleware.js";
```

Then:

```js
router.get("/me", authenticate, getMe);
```

The important part is:

```text
authenticate
     ↓
getMe
```

---

# 14. Test in Postman

### First: login

```http
POST /api/auth/login
```

Use your existing credentials.

You already know this creates the session.

---

### Second: call

```http
GET /api/auth/me
```

**Don't manually send the user ID.**

Postman should send the session cookie from your login.

If everything is working:

```json
{
  "status": "success",
  "data": {
    "user": {
      "id": "68abc123",
      "name": "Pranav",
      "email": "pranav@example.com",
      "role": "user"
    }
  }
}
```

🎉 That proves the complete chain works.

---

# 15. Then test without authentication

Delete/clear the Postman cookie for your API domain.

Then:

```http
GET /api/auth/me
```

You should get:

```json
{
  "status": "fail",
  "statusCode": 401,
  "message": "You are not authenticated"
}
```

That proves our middleware is actually protecting the route.

---

You don't need to change any code for this part.

### In Postman

#### 1. First, make sure `/me` works while logged in

You should have already done:

```http
POST /api/auth/login
```

Then:

```http
GET /api/auth/me
```

and received your user information.

That means Postman currently has your session cookie.

---

#### 2. Remove the session cookie

In Postman:

1. Find your request, for example:
    
    ```http
    GET http://localhost:5000/api/auth/me
    ```
    
2. Look near the top for **Cookies**.
    
3. Click **Cookies**.
    
4. Find the cookie belonging to your API domain, usually something like:
    
    ```text
    localhost
    ```
    
5. Find your session cookie. If you're using the default `express-session` setup, it is commonly:
    
    ```text
    connect.sid
    ```
    
6. Delete that cookie.
    
7. Close the cookie window.
    

**Important:** Don't delete your whole Postman collection—just remove the session cookie.

---

#### 3. Send `/me` again

Now send:

```http
GET /api/auth/me
```

This time there is **no session cookie**.

Your middleware executes:

```js
const userId = req.session.userId;

if (!userId) {
  return next(new ApiError(401, "You are not authenticated"));
}
```

Because there is no valid session:

```text
req.session.userId
        ↓
    undefined
        ↓
    !userId
        ↓
      true
        ↓
     401
```

You should get something like:

```json
{
  "status": "fail",
  "statusCode": 401,
  "message": "You are not authenticated"
}
```

### 4. What this proves

You've now tested both sides:

```text
LOGIN
  ↓
session cookie
  ↓
GET /me
  ↓
req.session.userId
  ↓
req.user
  ↓
200 ✅
```

And:

```text
NO COOKIE
  ↓
GET /me
  ↓
no req.session.userId
  ↓
401 ❌
```

If you get **401 without the cookie**, your authentication middleware is protecting the route correctly.

**One small warning:** If you delete the cookie and then `/me` still returns the user, check Postman's **Headers** tab for a manually added `Cookie` header and remove it too.

---

# The complete Step 7 flow

Keep this in your notes:

```text
POST /login
     ↓
Password verified
     ↓
req.session.userId = user._id
     ↓
Session saved in MongoDB
     ↓
Cookie sent to client
     ↓
────────────────────────────
     ↓
GET /auth/me
     ↓
Cookie automatically sent
     ↓
express-session reads cookie
     ↓
Session found
     ↓
req.session.userId
     ↓
User.findById(userId)
     ↓
req.user = user
     ↓
next()
     ↓
getMe controller
     ↓
req.user
     ↓
Response
```

### Your immediate task

Don't modify the Task controller yet.

Do these **three things only**:

1. Create `auth.middleware.js`.
    
2. Add the protected `/me` route.
    
3. Test `/login → /me` and then `/me` without the session cookie.
    

Once `/me` works, **Step 7 is complete**, and then we'll use the exact same authentication middleware on your existing `/api/tasks` routes.