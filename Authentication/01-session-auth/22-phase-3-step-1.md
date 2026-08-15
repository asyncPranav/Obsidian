
---

Excellent. **Step 9 is DONE.** ✅

Now we move to the most important authorization concept in the project.

# Step 10 — Ownership Authorization

Until now, we've established:

```text
Authentication
     ↓
"Who is this?"
     ↓
req.user
```

Now we need:

```text
Authorization
     ↓
"Is this user allowed to access THIS task?"
```

---

## 1. The problem we currently have

Suppose MongoDB contains:

```text
User A → _id: AAA

Task 1 → userId: AAA
Task 2 → userId: BBB
```

User A logs in.

Because of Step 8:

```text
User A
  ↓
session
  ↓
authenticate
  ↓
req.user = User A
```

Now User A knows Task 2's ID and sends:

```http
DELETE /api/tasks/TASK_2_ID
```

Your current controller does:

```js
Task.findByIdAndDelete(req.params.id)
```

It doesn't care who owns the task.

So **User A could delete User B's task.** ❌

That's an authorization vulnerability.

---

# 2. What should happen?

The server needs to check:

```text
Logged-in user
      ↓
req.user._id
      │
      │ compare
      ▼
Task.userId
```

If:

```text
req.user._id === task.userId
```

allow.

Otherwise:

```text
403 Forbidden
```

The important distinction:

```text
401 → You are NOT authenticated.

403 → You ARE authenticated, but you're NOT allowed.
```

---

# 3. Where should we put this logic?

You already have:

```text
src/middlewares/
```

We're going to create:

```text
ownership.middleware.js
```

But there's an important design decision.

We don't want one middleware that only works for tasks.

We want to understand a reusable pattern.

For now, however, **keep it simple and task-specific** because you're learning the concept.

Create:

```text
src/middlewares/taskOwnership.middleware.js
```

---

# 4. What will this middleware do?

The request:

```http
PATCH /api/tasks/:id
```

contains:

```js
req.params.id
```

That's the task ID.

Our middleware will:

```text
req.params.id
       ↓
Find Task
       ↓
Does task exist?
       ↓
Does task.userId equal req.user._id?
       ↓
YES → next()
NO  → 403
```

---

# 5. Write the middleware

Create:

```js
import Task from "../models/task.model.js";
import ApiError from "../utils/ApiError.js";

const checkTaskOwnership = async (req, res, next) => {
  try {
    const task = await Task.findById(req.params.id);

    if (!task) {
      return next(new ApiError(404, "Task not found"));
    }

    if (task.userId.toString() !== req.user._id.toString()) {
      return next(
        new ApiError(403, "You are not allowed to access this task"),
      );
    }

    req.task = task;

    next();
  } catch (error) {
    next(error);
  }
};

export default checkTaskOwnership;
```

Don't just copy it—understand the important parts.

---

# 6. Why do we need `.toString()`?

This is important with MongoDB.

`task.userId` is an:

```text
ObjectId
```

and:

```js
req.user._id
```

is also an ObjectId.

You shouldn't rely on:

```js
task.userId === req.user._id
```

because these are objects.

Instead:

```js
task.userId.toString() === req.user._id.toString()
```

gives us:

```text
"68abc123" === "68abc123"
```

which can be compared reliably.

---

# 7. Why `req.task = task`?

We already queried:

```js
const task = await Task.findById(req.params.id);
```

Imagine we don't save it.

Then the controller might later do:

```js
const task = await Task.findById(req.params.id);
```

That's another database query.

Instead:

```js
req.task = task;
```

means:

```text
Request
├── session
├── user
└── task
```

Now the controller can reuse:

```js
req.task
```

This is another useful middleware pattern:

> Middleware can attach information to `req` for downstream middleware/controllers.

Just like we previously did:

```js
req.user = user;
```

Now we're doing:

```js
req.task = task;
```

---

# 8. Now protect individual task routes

Import the middleware:

```js
import checkTaskOwnership from "../middlewares/taskOwnership.middleware.js";
```

Your task routes should become:

```js
router
  .route("/:id")
  .get(
    authenticate,
    validateObjectId,
    checkTaskOwnership,
    getTask,
  )
  .patch(
    authenticate,
    validateObjectId,
    checkTaskOwnership,
    updateTaskValidator,
    validate,
    updateTask,
  )
  .delete(
    authenticate,
    validateObjectId,
    checkTaskOwnership,
    deleteTask,
  );
```

Notice the order:

```text
authenticate
      ↓
validateObjectId
      ↓
checkTaskOwnership
      ↓
controller
```

---

# 9. Why this order?

### First: Authentication

```text
authenticate
```

We need:

```js
req.user
```

because ownership checking needs to know:

> Who is making the request?

So authentication must happen first.

---

### Second: ObjectId validation

```text
validateObjectId
```

We don't want:

```text
abc123
```

going into:

```js
Task.findById()
```

Your existing middleware handles that.

---

### Third: Ownership

Now we have:

```text
req.user
+
valid task ID
```

So we can safely ask:

```text
Does this task belong to this user?
```

---

# 10. What about `getTask`?

Your current controller:

```js
const getTask = async (req, res, next) => {
  const task = await Task.findById(req.params.id);

  ...
};
```

That's okay for now.

The ownership middleware already fetched the task and placed it in:

```js
req.task
```

So we can later clean this up.

**Don't change the controller yet.**

First understand and test the middleware.

---

# 11. Test with TWO users

This test is extremely important.

Create two users:

```text
User A
email: a@example.com

User B
email: b@example.com
```

Login as User A.

Create a task:

```json
{
  "title": "User A Task",
  "description": "This belongs to User A"
}
```

Because of Step 9:

```text
task.userId = User A's ID
```

---

## Test 1 — Owner

While logged in as User A:

```http
GET /api/tasks/TASK_A_ID
```

Expected:

```text
200 OK
```

Because:

```text
req.user._id
      =
task.userId
```

---

## Test 2 — Non-owner

Now login as User B.

Then use the **same Task A ID**:

```http
GET /api/tasks/TASK_A_ID
```

Expected:

```text
403 Forbidden
```

because:

```text
User B ID
   ≠
Task A userId
```

🔥 **This is the moment you have actually implemented authorization.**

---

# 12. Test all three protected operations

As User B, try:

```http
GET /api/tasks/TASK_A_ID
```

```http
PATCH /api/tasks/TASK_A_ID
```

```http
DELETE /api/tasks/TASK_A_ID
```

All should return:

```text
403 Forbidden
```

User A should be able to perform them.

---

# 13. One important thing about `GET /api/tasks`

There's a slight difference.

For:

```http
GET /api/tasks
```

there is no specific task ID.

So ownership middleware can't check one task.

Instead, **the query itself should be scoped to the authenticated user.**

Your current code:

```js
const tasks = await Task.find();
```

needs to eventually become:

```js
const tasks = await Task.find({
  userId: req.user._id,
});
```

Then:

```text
GET /api/tasks
```

means:

> "Give me MY tasks."

Not:

> "Give me every task in the database."

We'll make that change as part of this authorization stage.

---

# Step 10 mental model

Remember this:

```text
                 REQUEST
                    │
                    ▼
             authenticate
                    │
              "Who are you?"
                    │
                    ▼
                req.user
                    │
                    ▼
          validateObjectId
                    │
                    ▼
          checkTaskOwnership
                    │
          "Is this YOUR task?"
                    │
             ┌──────┴──────┐
             │             │
            YES            NO
             │             │
             ▼             ▼
           next()         403
             │
             ▼
        Controller
```

And the entire security model we're building is now:

```text
Authentication
      ↓
Who are you?
      ↓
req.user
      ↓
Authorization
      ↓
What are you allowed to access?
      ↓
Task ownership
      ↓
Controller
```

### Your Step 10 checklist

```text
☐ Create taskOwnership.middleware.js
☐ Find task using req.params.id
☐ Return 404 if task doesn't exist
☐ Compare task.userId with req.user._id
☐ Return 403 for non-owner
☐ Attach task to req.task
☐ Add ownership middleware to :id routes
☐ Test owner → 200
☐ Test another user → 403
☐ Test GET/PATCH/DELETE
☐ Change GET /api/tasks to return only req.user's tasks
```

**Do these and test with two users.** Once you've confirmed User A cannot touch User B's task, we'll move to the next part: **cleaning up the controllers using `req.task` and then introducing role-based authorization (`user` vs `admin`).**