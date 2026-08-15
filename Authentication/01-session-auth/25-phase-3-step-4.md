
---

Yes — now that I can see your **actual `auth.controller.js`**, Step 13 can be much more precise.

And importantly: **your current `register()` is already protected against the privilege-escalation issue I described.** You are already doing the correct thing:

```js
const { name, email, password } = req.body;
```

and:

```js
const user = await User.create({
  name,
  email,
  password: hashedPassword,
});
```

You never accept `role` from the client. So we won't unnecessarily change that code.

# Phase 2 — Step 13

# Prevent Privilege Escalation & Understand Trusted Authorization Data

## Where we are

Your current authentication system is now:

```text
REGISTER
   ↓
Validate input
   ↓
Check email
   ↓
bcrypt.hash()
   ↓
Create User
   ↓
role defaults to "user"


LOGIN
   ↓
Find User
   ↓
bcrypt.compare()
   ↓
req.session.userId = user._id
   ↓
Session stored in MongoDB


PROTECTED REQUEST
   ↓
Session
   ↓
authenticate middleware
   ↓
req.user
   ↓
Authorization
   ├── role
   └── ownership


LOGOUT
   ↓
req.session.destroy()
   ↓
Clear cookie
```

Now we're going to understand an important security rule:

> **The client must never be trusted to decide authorization-related information.**

---

# 1. What is privilege escalation?

Privilege escalation means:

> A user manages to give themselves more permissions than they should have.

Our system has:

```js
role: {
  type: String,
  enum: ["user", "admin"],
  default: "user",
}
```

So normally:

```text
New user
   ↓
role = "user"
```

An admin has:

```text
role = "admin"
```

And our authorization middleware probably does something like:

```text
req.user.role
      ↓
Is role "admin"?
      ↓
YES → allow
NO  → 403
```

Therefore, **protecting the role value is extremely important.**

---

# 2. Let's look at YOUR register controller

You currently have:

```js
const register = async (req, res, next) => {
  try {
    const { name, email, password } = req.body;
```

Notice something important.

You're extracting:

```text
name
email
password
```

but NOT:

```text
role
```

That's good.

Then you create the user:

```js
const user = await User.create({
  name,
  email,
  password: hashedPassword,
});
```

Again:

```text
role ❌
```

isn't provided.

Therefore Mongoose uses your model's default:

```js
default: "user"
```

So:

```text
POST /register
       ↓
User.create()
       ↓
role wasn't supplied
       ↓
Mongoose default
       ↓
role = "user"
```

### Your current implementation is already correct.

**Do not change `register()` for this step.**

---

# 3. Why is this important?

Imagine a careless implementation:

```js
const { name, email, password, role } = req.body;
```

and:

```js
const user = await User.create({
  name,
  email,
  password: hashedPassword,
  role,
});
```

Now an attacker could send:

```json
{
  "name": "Hacker",
  "email": "hacker@example.com",
  "password": "Password@123",
  "role": "admin"
}
```

The server would create:

```text
Hacker
email: hacker@example.com
role: admin
```

The attacker just became an administrator.

That's a serious security vulnerability.

---

# 4. Test this against YOUR API

Let's prove that your implementation prevents this.

Send:

```http
POST /api/auth/register
```

with:

```json
{
  "name": "Privilege Test",
  "email": "privilege@example.com",
  "password": "Password@123",
  "role": "admin"
}
```

Notice that we're deliberately sending:

```json
"role": "admin"
```

---

## What should happen?

Your registration should succeed:

```text
201 Created
```

But the response should contain:

```json
{
  "role": "user"
}
```

And MongoDB should contain:

```text
role: "user"
```

NOT:

```text
role: "admin"
```

Why?

Because your controller ignores the incoming `role`.

---

# 5. Why doesn't Express automatically reject `role`?

This is an important distinction.

You might think:

> "If our validator doesn't contain role, shouldn't validation reject it?"

No.

Your validator controls whether specific fields satisfy specific validation rules.

It doesn't automatically mean:

```text
Unknown field → reject request
```

So the request can contain:

```json
{
  "name": "...",
  "email": "...",
  "password": "...",
  "role": "admin"
}
```

But your controller only extracts:

```js
const { name, email, password } = req.body;
```

Therefore:

```text
req.body.role
       ↓
ignored
```

This is the important part.

---

# 6. Your User model provides another layer

Your model has:

```js
role: {
  type: String,
  enum: ["user", "admin"],
  default: "user",
}
```

This gives us two useful protections.

### Protection 1 — Default

If we don't provide a role:

```text
role → "user"
```

### Protection 2 — Enum

If server-side code tries:

```js
role: "super-admin"
```

Mongoose rejects it because:

```text
"user"       ✅
"admin"      ✅
"super-admin" ❌
```

But remember:

> **The enum is not an authorization system.**

It only controls valid values.

Your authorization middleware decides whether someone is allowed to perform an admin action.

---

# 7. Where does the trusted role come from?

After login:

```js
req.session.userId = user._id;
```

The session stores the identity:

```text
session
   ↓
userId
```

Then authentication middleware finds the user:

```text
req.session.userId
       ↓
User.findById()
       ↓
user
       ↓
req.user
```

So now:

```js
req.user.role
```

comes from your **database user record**.

That's trusted server-side information.

---

# 8. This is why our authorization middleware works

Suppose:

```text
Database

User A
role = user
```

Request:

```http
GET /api/users
```

Pipeline:

```text
Request
   ↓
Session
   ↓
authenticate
   ↓
req.user
   ↓
req.user.role
   ↓
"user"
   ↓
authorize("admin")
   ↓
403 Forbidden
```

The client cannot simply send:

```json
{
  "role": "admin"
}
```

and change:

```js
req.user.role
```

because authorization isn't reading the role from the request body.

It's reading:

```js
req.user.role
```

from the authenticated user.

---

# 9. Very important distinction

There are three different kinds of data here.

### A. User-controlled input

Example:

```js
req.body.title
req.body.description
req.body.email
```

We validate these.

---

### B. Authentication-derived data

Example:

```js
req.session.userId
```

This comes from the authenticated session.

---

### C. Authorization data

Example:

```js
req.user.role
req.user._id
task.userId
```

These determine what the user is allowed to do.

We should **not allow the client to directly control them.**

---

# 10. You already applied this principle to Tasks

Remember Step 9.

Originally:

```js
const { title, description, completed, userId } = req.body;
```

We changed it.

Now:

```js
const { title, description, completed } = req.body;
```

and:

```js
userId: req.user._id
```

Why?

Because the client shouldn't be able to say:

```json
{
  "title": "My Task",
  "userId": "SOME_OTHER_USER"
}
```

Instead:

```text
Session
   ↓
req.user._id
   ↓
Task.userId
```

That's exactly the same security principle we're learning now.

---

# 11. What about updating users?

Your current `updateUser` controller from Phase 1 accepts:

```js
const { name, email, password } = req.body;
```

Notice:

```text
role ❌
```

That's good.

We should **keep it that way**.

Don't add:

```js
const { name, email, password, role } = req.body;
```

and don't add:

```js
updatedData.role = role;
```

Otherwise a user could potentially change their own role.

---

# 12. Your `updateUserValidator` is also correct

Your validator handles:

```text
name
email
password
```

and doesn't contain:

```text
role
```

Keep it that way.

---

# 13. What if an admin needs to change someone's role?

That's a different operation.

For example, eventually we could create:

```http
PATCH /api/users/:id/role
```

and protect it:

```text
authenticate
      ↓
authorize("admin")
      ↓
changeUserRole
```

Then:

```text
Student
   ↓
❌ cannot change role

Admin
   ↓
✅ can change another user's role
```

That's the correct architecture.

**We are not building this endpoint in Step 13.**

---

# 14. One subtle issue in your current code

I noticed this in your `register()`:

```js
return next(new ApiError("Email already exists", 400));
```

But your `ApiError` constructor from Phase 1 is:

```js
class ApiError extends Error {
  constructor(statusCode, message, errors = []) {
    super(message);
    this.statusCode = statusCode;
    this.errors = errors;
  }
}
```

So the correct call should be:

```js
return next(new ApiError(400, "Email already exists"));
```

You have the arguments reversed in this particular line.

Your duplicate-key catch has the correct order:

```js
return next(new ApiError("Email already exists", 400));
```

Actually, looking at that too, **that one is also reversed**.

So both should be:

```js
return next(new ApiError(400, "Email already exists"));
```

This is **not the main purpose of Step 13**, but fix it because otherwise your error response can behave incorrectly.

---

# 15. Corrected register controller

Your controller should therefore look like:

```js
const register = async (req, res, next) => {
  try {
    const { name, email, password } = req.body;

    // 1. Check if the email already exists
    const existingUser = await User.findOne({ email });

    if (existingUser) {
      return next(new ApiError(400, "Email already exists"));
    }

    // 2. Hash the password
    const hashedPassword = await bcrypt.hash(password, 10);

    // 3. Create user
    // role is NOT taken from req.body.
    // Mongoose will use the default role: "user".
    const user = await User.create({
      name,
      email,
      password: hashedPassword,
    });

    // 4. Never send password back
    res.status(201).json({
      status: "success",
      message: "User registered successfully",
      data: {
        user: {
          _id: user._id,
          name: user.name,
          email: user.email,
          role: user.role,
          createdAt: user.createdAt,
          updatedAt: user.updatedAt,
        },
      },
    });
  } catch (error) {
    // Handle duplicate email race condition
    if (error.code === 11000) {
      return next(new ApiError(400, "Email already exists"));
    }

    next(error);
  }
};
```

The important security line is:

```js
const { name, email, password } = req.body;
```

and:

```js
const user = await User.create({
  name,
  email,
  password: hashedPassword,
});
```

---

# 16. Step 13 experiment

Now do this in Postman.

### Request

```http
POST /api/auth/register
```

```json
{
  "name": "Security Test",
  "email": "securitytest@example.com",
  "password": "Password@123",
  "role": "admin"
}
```

### Expected response

```json
{
  "status": "success",
  "data": {
    "user": {
      "role": "user"
    }
  }
}
```

Then check MongoDB.

You should see:

```text
role: "user"
```

---

# 17. Now test the opposite

Try creating a normal user:

```json
{
  "name": "Normal User",
  "email": "normal@example.com",
  "password": "Password@123"
}
```

Expected:

```text
role = user
```

because of:

```js
default: "user"
```

---

# 18. The complete security principle

You should now be able to explain this yourself:

```text
Client
  │
  │ sends untrusted data
  ▼
Express
  │
  ├── Validate input
  │
  ├── Authenticate
  │      ↓
  │   req.session
  │      ↓
  │   req.user
  │
  ├── Authorize
  │      ↓
  │   req.user.role
  │
  └── Perform operation
```

And **never**:

```text
Client
   ↓
"role": "admin"
   ↓
Trust it ❌
```

or:

```text
Client
   ↓
"userId": "someone-else"
   ↓
Trust it ❌
```

---

# Step 13 — What you actually need to do

Your implementation is **already mostly correct**.

So your work is:

```text
STEP 13
────────────────────────────

☑ Understand privilege escalation

☑ Verify register does NOT accept role

☑ Keep:
   const { name, email, password } = req.body

☑ Keep role out of User update

☑ Test registration with:
   "role": "admin"

☑ Verify MongoDB stores:
   role: "user"

☑ Understand why req.user.role is trusted
   while req.body.role is not

☑ Fix ApiError argument order
```

### The key lesson from Step 13

> **The client can request an action, but the server must decide the user's identity, ownership, and permissions.**

You've already applied this principle to `Task.userId`. Now you're applying it to `User.role`.

Once you've done the test and fixed the `ApiError` argument order, **Step 13 is complete.**