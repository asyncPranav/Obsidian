
---

### Old `task.controller.js`

```js
import Task from "../models/task.model.js";
import User from "../models/user.model.js";
import ApiError from "../utils/ApiError.js";

const getAllTasks = async (req, res, next) => {
  try {
    const tasks = await Task.find();

    res.status(200).json({
      status: "success",
      results: tasks.length,
      data: { tasks },
    });
  } catch (error) {
    next(error);
  }
};

const getTask = async (req, res, next) => {
  try {
    const task = await Task.findById(req.params.id);

    if (!task) {
      return next(new ApiError(404, "Task not found"));
    }

    res.status(200).json({
      status: "success",
      data: { task },
    });
  } catch (error) {
    next(error);
  }
};

const createTask = async (req, res, next) => {
  try {
    const { title, description, completed, userId } = req.body;

    const user = await User.findById(userId);

    if (!user) {
      return next(new ApiError(404, "User not found"));
    }

    const task = await Task.create({
      title,
      description,
      completed,
      userId,
    });

    res.status(201).json({
      status: "success",
      message: "task created successfully",
      data: { task },
    });
  } catch (error) {
    next(error);
  }
};

const updateTask = async (req, res, next) => {
  try {
    const { title, description, completed, userId } = req.body;

    const updatedData = {};

    if (title !== undefined) {
      updatedData.title = title;
    }

    if (description !== undefined) {
      updatedData.description = description;
    }

    if (completed !== undefined) {
      updatedData.completed = completed;
    }

    if (userId !== undefined) {
      const user = await User.findById(userId);

      if (!user) {
        return next(new ApiError(404, "User not found"));
      }

      updatedData.userId = userId;
    }

    if (Object.keys(updatedData).length === 0) {
      return next(new ApiError(400, "No valid fields provided for update"));
    }

    const updatedTask = await Task.findByIdAndUpdate(
      req.params.id,
      updatedData,
      {
        new: true,
        runValidators: true,
      },
    );

    if (!updatedTask) {
      return next(new ApiError(404, "Task not found"));
    }

    res.status(200).json({
      status: "success",
      message: "task updated successfully",
      data: { task: updatedTask },
    });
  } catch (error) {
    next(error);
  }
};

const deleteTask = async (req, res, next) => {
  try {
    const deletedTask = await Task.findByIdAndDelete(req.params.id);

    if (!deletedTask) {
      return next(new ApiError(404, "Task not found"));
    }

    res.status(200).json({
      status: "success",
      message: "task deleted successfully",
      data: { deletedTask },
    });
  } catch (error) {
    next(error);
  }
};

export {
  getAllTasks,
  getTask,
  createTask,
  updateTask,
  deleteTask,
};
```



Great. **Step 8 is DONE.** ✅

Now we reach one of the most important changes in this project.

# Step 9 — Stop trusting `userId` from the client

Remember your Phase 1 `createTask` request:

```json
{
  "title": "Learn Sessions",
  "description": "Understand session authentication",
  "userId": "68abc123"
}
```

There is a security problem here.

A logged-in user could send:

```json
{
  "title": "Steal someone's task ownership",
  "description": "...",
  "userId": "SOME_OTHER_USER_ID"
}
```

Why should the client be allowed to decide who owns the task?

**It shouldn't.**

We already know who the logged-in user is through:

```js
req.user
```

So the server should decide ownership.

---

# 1. New flow

Instead of:

```text
Client
  ↓
userId
  ↓
Task
```

we want:

```text
Client
  ↓
Session Cookie
  ↓
authenticate
  ↓
req.user
  ↓
req.user._id
  ↓
Task.userId
```

The client only provides:

```json
{
  "title": "Learn Sessions",
  "description": "Understand session authentication"
}
```

The server automatically adds:

```js
userId: req.user._id
```

---

# 2. First change — `createTaskValidator`

Your current validator contains:

```js
body("userId")
  .notEmpty()
  .withMessage("User ID is required")
  .isMongoId()
  .withMessage("Invalid User ID format"),
```

**Remove this entire `userId` validation.**

Your validator should now only validate data the client is actually allowed to provide:

```js
import { body } from "express-validator";

const createTaskValidator = [
  body("title")
    .trim()
    .notEmpty()
    .withMessage("Title is required")
    .isLength({ min: 3, max: 100 })
    .withMessage("Title must be between 3 and 100 characters"),

  body("description")
    .trim()
    .notEmpty()
    .withMessage("Description is required")
    .isLength({ max: 500 })
    .withMessage("Description must be less than 500 characters"),

  body("completed")
    .optional()
    .isBoolean()
    .withMessage("Completed must be a boolean value"),
];

export default createTaskValidator;
```

### Why?

Because `userId` is **not user input anymore**.

It's authentication-derived data.

---

# 3. Change `createTask`

Your current controller has:

```js
const { title, description, completed, userId } = req.body;
```

Change it to:

```js
const { title, description, completed } = req.body;
```

And your task creation currently does:

```js
const user = await User.findById(userId);

if (!user) {
  return next(new ApiError(404, "User not found"));
}

const task = await Task.create({
  title,
  description,
  completed,
  userId,
});
```

We don't need that anymore.

Why?

Because:

```text
authenticate middleware
        ↓
req.user
```

already proved that the user exists.

So:

```js
const task = await Task.create({
  title,
  description,
  completed,
  userId: req.user._id,
});
```

That's the important change.

---

# 4. Your new `createTask`

Conceptually:

```js
const createTask = async (req, res, next) => {
  try {
    const { title, description, completed } = req.body;

    const task = await Task.create({
      title,
      description,
      completed,
      userId: req.user._id,
    });

    res.status(201).json({
      status: "success",
      message: "task created successfully",
      data: { task },
    });
  } catch (error) {
    next(error);
  }
};
```

Notice:

```js
userId: req.user._id
```

This is the **core lesson of Step 9**.

---

# 5. Why don't we need `User.findById()` anymore?

Previously:

```text
req.body.userId
       ↓
User.findById()
       ↓
Does user exist?
```

Now:

```text
Session
   ↓
req.session.userId
   ↓
authenticate middleware
   ↓
User.findById()
   ↓
req.user
```

So the check already happened **before the controller**.

Our pipeline is:

```text
POST /api/tasks
       ↓
authenticate
       ↓
User exists?
       ↓
req.user
       ↓
createTask
       ↓
Task.userId = req.user._id
```

This is exactly why middleware is useful.

---

# 6. Test it in Postman

### First login

```http
POST /api/auth/login
```

Make sure your session cookie exists.

### Then create task

```http
POST /api/tasks
```

Send:

```json
{
  "title": "Learn Authorization",
  "description": "Understand ownership authorization"
}
```

**Do NOT send `userId`.**

Expected:

```text
201 Created
```

And MongoDB should show:

```text
Task
├── title
├── description
├── completed
├── userId  ← automatically assigned
├── createdAt
└── updatedAt
```

The `userId` should be the ID of the currently logged-in user.

---

# 7. Important experiment

Now try this:

```json
{
  "title": "Test",
  "description": "Testing",
  "userId": "another-user-id"
}
```

What happens?

Your validator won't validate `userId`, and your controller won't use:

```js
req.body.userId
```

So even if the client sends a fake/other ID, **the server still assigns:**

```js
req.user._id
```

That's the security property we wanted.

---

# 8. One more thing: `updateTask`

Your current `updateTaskValidator` also contains:

```js
body("userId")
  .optional()
  .notEmpty()
  .isMongoId()
```

And your controller allows:

```js
if (userId !== undefined) {
   ...
   updatedData.userId = userId;
}
```

🚨 **We will remove this too.**

A user should not be able to change:

```text
Task A
owner = User A
```

into:

```text
Task A
owner = User B
```

That's an ownership/security issue.

So remove `userId` from:

```text
updateTaskValidator
```

and from:

```text
updateTask
```

But **don't modify the rest of `updateTask` yet.**

There's a reason.

---

# 9. The bigger problem we're about to solve

Suppose:

```text
User A
  ↓
Task A
```

and:

```text
User B
  ↓
Task B
```

User A is logged in.

They know Task B's ID.

They send:

```http
DELETE /api/tasks/TASK_B_ID
```

Our Step 8 authentication middleware says:

```text
"Yep, User A is logged in." ✅
```

Then your current controller does:

```js
Task.findByIdAndDelete(req.params.id)
```

and deletes Task B.

😬

That's why **authentication alone isn't enough.**

We now need:

# Step 10 — Ownership Authorization

We'll make the server ask:

```text
Who is logged in?
       ↓
req.user._id

Who owns this task?
       ↓
task.userId

Are they the same?
       ↓
YES → allow
NO  → 403 Forbidden
```

That's the next major concept.

---

## Step 9 checklist

Before you tell me you're done:

```text
PHASE 2 — STEP 9

✅ Remove userId from createTaskValidator
✅ Remove userId from createTask request body
✅ createTask uses req.user._id
✅ Test creating task without userId
✅ Verify MongoDB assigns correct userId
✅ Understand why User.findById() is no longer needed
✅ Remove userId from updateTaskValidator
✅ Remove userId ownership-changing logic from updateTask
```

**Do these changes and test them.**

Then tell me **Step 9 done**, and we'll build **Step 10: Ownership Authorization** — where User A will be prevented from accessing User B's tasks.