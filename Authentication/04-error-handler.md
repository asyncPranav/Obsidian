
---

Absolutely. Think of this as **a custom way to handle errors in an Express.js API**.

You have **two separate pieces**:

1. `ApiError` → creates a custom error with a status code.
    
2. `errorHandler` → catches that error and sends a proper response to the client.
    

### 1. `ApiError`

```js
class ApiError extends Error {
  constructor(statusCode, message) {
    super(message);
    this.statusCode = statusCode;
  }
}

export default ApiError;
```

Let's break it down.

#### `class ApiError extends Error`

JavaScript already gives you an `Error` class:

```js
const error = new Error("Something went wrong");
```

Your code creates a **custom version** of that error:

```js
class ApiError extends Error
```

So `ApiError` gets all the normal behavior of JavaScript's `Error`, but you add something extra: `statusCode`.

---

#### The constructor

```js
constructor(statusCode, message) {
```

You're saying that whenever you create an `ApiError`, you expect two things:

```js
statusCode
message
```

For example:

```js
new ApiError(404, "User not found");
```

Here:

```text
statusCode → 404
message    → "User not found"
```

---

#### `super(message)`

This is important.

Because you wrote:

```js
class ApiError extends Error
```

your class inherits from `Error`.

So you need to call the parent `Error` constructor:

```js
super(message);
```

This basically says:

> "Hey JavaScript, initialize this as a normal Error with this message."

So:

```js
new ApiError(404, "User not found")
```

will have:

```js
err.message
// "User not found"
```

---

#### `this.statusCode = statusCode`

This adds your custom property:

```js
this.statusCode = statusCode;
```

So:

```js
const error = new ApiError(404, "User not found");
```

roughly gives you:

```js
error.message
// "User not found"

error.statusCode
// 404
```

That's the main reason for creating `ApiError`.

Normally an `Error` has a message, but your API also needs to know **what HTTP status code to return**.

---

# 2. `errorHandler`

Now you have:

```js
const errorHandler = (err, req, res, next) => {
  const statusCode = err.statusCode || 500;
  const message = err.message || "Internal Server Error";

  res.status(statusCode).json({
    status: "fail",
    statusCode,
    message,
  });
};
```

This is **Express error-handling middleware**.

The important thing is this:

```js
(err, req, res, next)
```

Normal Express middleware looks like:

```js
(req, res, next)
```

But error-handling middleware has **four arguments**:

```js
(err, req, res, next)
```

The first one is the error.

---

## Imagine a request

Suppose your API has:

```js
app.get("/users/:id", (req, res, next) => {
  const user = null;

  if (!user) {
    return next(new ApiError(404, "User not found"));
  }

  res.json(user);
});
```

The user requests:

```text
GET /users/123
```

But user `123` doesn't exist.

So you do:

```js
next(new ApiError(404, "User not found"));
```

Now Express says:

> "An error was passed to `next()`. I need to send it to the error-handling middleware."

So your `errorHandler` receives:

```js
err = ApiError {
  statusCode: 404,
  message: "User not found"
}
```

---

## Then this runs

```js
const statusCode = err.statusCode || 500;
```

Since:

```js
err.statusCode
```

is:

```js
404
```

you get:

```js
statusCode = 404;
```

The `|| 500` means:

> "If `err.statusCode` doesn't exist, use 500."

For example:

```js
err.statusCode = 404
```

→ `404`

But:

```js
err.statusCode = undefined
```

→ `500`

So it's basically a fallback.

---

Then:

```js
const message = err.message || "Internal Server Error";
```

Same idea.

If:

```js
err.message
```

is:

```text
User not found
```

then:

```js
message = "User not found";
```

If there is no message:

```js
message = "Internal Server Error";
```

---

# Finally, the response

```js
res.status(statusCode).json({
  status: "fail",
  statusCode,
  message,
});
```

Since we had:

```js
statusCode = 404
message = "User not found"
```

the client receives:

```json
{
  "status": "fail",
  "statusCode": 404,
  "message": "User not found"
}
```

And the HTTP response status itself is:

```text
404 Not Found
```

---

# The whole flow

This is the most important part to understand:

```text
Client
  │
  │ GET /users/123
  ▼
Express Route
  │
  │ User doesn't exist
  ▼
next(new ApiError(404, "User not found"))
  │
  ▼
errorHandler
  │
  ├── err.statusCode → 404
  │
  ├── err.message → "User not found"
  │
  ▼
res.status(404).json(...)
  │
  ▼
Client gets:
{
  "status": "fail",
  "statusCode": 404,
  "message": "User not found"
}
```

---

## Why not just do this?

You might wonder why we don't simply do:

```js
res.status(404).json({
  message: "User not found"
});
```

You **could**.

But imagine a large application with hundreds of routes.

You'd have errors like:

```js
res.status(400).json(...)
res.status(401).json(...)
res.status(403).json(...)
res.status(404).json(...)
res.status(500).json(...)
```

all over your application.

Instead, you can do:

```js
throw new ApiError(404, "User not found");
```

or:

```js
next(new ApiError(404, "User not found"));
```

And let **one central error handler** decide how errors should be returned.

That gives you a consistent API.

---

# One more example

Suppose you have:

```js
app.get("/profile", (req, res, next) => {
  const loggedIn = false;

  if (!loggedIn) {
    return next(new ApiError(401, "You are not logged in"));
  }

  res.json({
    name: "John"
  });
});
```

This:

```js
new ApiError(401, "You are not logged in")
```

creates:

```js
{
  message: "You are not logged in",
  statusCode: 401
}
```

Then:

```js
next(...)
```

passes it to:

```js
errorHandler
```

which does:

```js
res.status(401).json({
  status: "fail",
  statusCode: 401,
  message: "You are not logged in"
});
```

The client gets:

```json
{
  "status": "fail",
  "statusCode": 401,
  "message": "You are not logged in"
}
```

### In one sentence

**`ApiError` lets you create errors that contain both a message and HTTP status code, while `errorHandler` catches those errors and turns them into a consistent JSON response for the client.**


