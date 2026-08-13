

---

Perfect. **Step 7 is DONE.** ✅

Now we're going to connect the authentication system to the **actual Task API**. This is where the project starts becoming what we originally wanted.

# Step 8 — Protect the Task API

So far we proved:

```text
/login
   ↓
Session created
   ↓
/me
   ↓
Session identified user
   ↓
req.user
```

Now we apply the same middleware to your existing Task routes.

---

## 1. What we're trying to achieve

Currently, your task routes are basically:

```text
GET    /api/tasks
POST   /api/tasks
GET    /api/tasks/:id
PATCH  /api/tasks/:id
DELETE /api/tasks/:id
```

And right now, someone can call them **without logging in**.

We're going to change that:

```text
                 /api/tasks
                     ↓
             authenticate middleware
                     ↓
                req.user
                     ↓
              Task controller
```

So:

```text
Not logged in
     ↓
GET /api/tasks
     ↓
❌ 401 Unauthorized
```

while:

```text
Logged in
     ↓
GET /api/tasks
     ↓
authenticate
     ↓
req.user
     ↓
✅ controller
```

---

# 2. Your existing Task router

You currently have something similar to:

```js
router
  .route("/")
  .get(validate, getAllTasks)
  .post(createTaskValidator, validate, createTask);

router
  .route("/:id")
  .get(validateObjectId, getTask)
  .patch(validateObjectId, updateTaskValidator, validate, updateTask)
  .delete(validateObjectId, deleteTask);
```

We are **not changing controllers yet**.

First, just protect the routes.

---

# 3. Import authentication middleware

At the top:

```js
import authenticate from "../middlewares/auth.middleware.js";
```

Then:

```js
router
  .route("/")
  .get(authenticate, validate, getAllTasks)
  .post(
    authenticate,
    createTaskValidator,
    validate,
    createTask
  );
```

And:

```js
router
  .route("/:id")
  .get(authenticate, validateObjectId, getTask)
  .patch(
    authenticate,
    validateObjectId,
    updateTaskValidator,
    validate,
    updateTask
  )
  .delete(
    authenticate,
    validateObjectId,
    deleteTask
  );
```

---

# 4. Why put `authenticate` first?

Look at:

```js
.get(authenticate, validate, getAllTasks)
```

Express executes middleware **left to right**:

```text
GET /api/tasks
      ↓
authenticate
      ↓
validate
      ↓
getAllTasks
```

If the user isn't logged in:

```text
authenticate
      ↓
❌ 401
```

So:

```text
validate
getAllTasks
```

never run.

That's exactly what we want.

---

# 5. Test this in Postman

## Test A — Without login

Clear your Postman session cookie.

Then:

```http
GET /api/tasks
```

Expected:

```text
401 Unauthorized
```

Because:

```text
No cookie
   ↓
No session
   ↓
No req.session.userId
   ↓
authenticate fails
```

---

## Test B — Login first

Do:

```http
POST /api/auth/login
```

Your session gets created.

Then immediately:

```http
GET /api/tasks
```

Now:

```text
Cookie
  ↓
Session
  ↓
userId
  ↓
authenticate
  ↓
req.user
  ↓
getAllTasks
```

You should get your existing task response.

### At this point, stop.

Don't change `getAllTasks` yet.

We're first proving that **authentication successfully protects the Task API.**

---

# 6. But we have a problem 😈

Suppose you're logged in as:

```text
User A
_id = AAA
```

You call:

```http
GET /api/tasks
```

Your current controller does:

```js
const tasks = await Task.find();
```

What does that return?

**Every task in the database.**

Imagine:

```text
Task 1 → User A
Task 2 → User A
Task 3 → User B
Task 4 → User C
```

User A requests:

```http
GET /api/tasks
```

and receives:

```text
Task 1
Task 2
Task 3  ← ❌ shouldn't see this
Task 4  ← ❌ shouldn't see this
```

So authentication is working, but **authorization isn't.**

---

# 7. This teaches us an important distinction

We just reached a very important concept.

### Authentication

```text
"Who are you?"
```

Our middleware answers:

```text
You are User A.
```

through:

```js
req.user
```

### Authorization

```text
"What are you allowed to access?"
```

We haven't implemented this yet.

So:

```text
Authentication ≠ Authorization
```

Being logged in doesn't automatically mean you're allowed to access everything.

---

# 8. Our next change

We want:

```text
User A logs in
      ↓
req.user = User A
      ↓
GET /api/tasks
      ↓
Only return tasks where:
task.userId === req.user._id
```

So eventually:

```js
const tasks = await Task.find({
  userId: req.user._id,
});
```

Now:

```text
Database:

Task 1 → User A
Task 2 → User A
Task 3 → User B
Task 4 → User C

              ↓

User A requests /tasks

              ↓

Task.find({
  userId: User A
})

              ↓

Task 1
Task 2
```

That's the beginning of **resource ownership authorization**.

---

# 9. But before we change `getAllTasks`...

There's an even bigger issue in your current API.

Your create-task validator currently requires:

```js
body("userId")
```

because Phase 1 was designed like this:

```json
{
  "title": "Learn Auth",
  "description": "Learn sessions",
  "userId": "USER_ID"
}
```

But now that authentication exists, **the client should NOT tell us who owns the task.**

We already know:

```text
Session
   ↓
req.user
   ↓
req.user._id
```

Therefore we'll change:

```text
❌ Client provides userId

to

✅ Server determines userId
```

That is our next major step.

---

# Step 8 checklist

Before proceeding:

```text
PHASE 2 — STEP 8

✅ Import authenticate middleware
✅ Protect GET /tasks
✅ Protect POST /tasks
✅ Protect GET /tasks/:id
✅ Protect PATCH /tasks/:id
✅ Protect DELETE /tasks/:id

✅ Test without login → 401
✅ Login
✅ Test again → route accessible

Next:
🔜 Stop accepting userId from client
🔜 Create tasks using req.user._id
🔜 Get only authenticated user's tasks
🔜 Ownership authorization
```

### The big picture

We have now moved from:

```text
Phase 1:

Client → Task API → MongoDB
```

to:

```text
Phase 2:

Client
  ↓
Session Cookie
  ↓
Authentication
  ↓
req.user
  ↓
Task API
  ↓
Authorization
  ↓
MongoDB
```

**Your next step is to make `userId` server-controlled.** That's where we'll start modifying your validators and `createTask` controller.