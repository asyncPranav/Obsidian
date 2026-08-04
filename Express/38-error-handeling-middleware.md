

---

Perfect. Since you're learning Express systematically, let's build your understanding from **zero**, without skipping steps.

---

# Step 1: What is an Error?

Imagine this route:

```javascript
app.get("/", (req, res) => {
    res.send("Hello");
});
```

Works perfectly.

Now imagine this:

```javascript
app.get("/", (req, res) => {
    const user = undefined;

    console.log(user.name);

    res.send("Hello");
});
```

Output:

```text
TypeError: Cannot read properties of undefined
```

Your server crashes or Express sends a default HTML error page.

We don't want that.

We want to send something like

```json
{
    "message":"Something went wrong"
}
```

That's where **Error Handling Middleware** comes in.

---

# Step 2: What is Middleware?

You already know middleware.

Normal middleware:

```javascript
function logger(req,res,next){

    console.log("Request received");

    next();
}
```

It has

```javascript
(req,res,next)
```

3 parameters.

---

# Step 3: Error Middleware

Error middleware has **4 parameters**.

```javascript
function errorHandler(err,req,res,next){

}
```

Notice

```javascript
err
```

is the extra parameter.

That's how Express recognizes it.

---

# Step 4: Smallest Error Middleware

```javascript
const express = require("express");

const app = express();

app.use((err,req,res,next)=>{

    console.log("Error middleware executed");

});

app.listen(8000);
```

Nothing special happens yet.

Because...

No error is reaching it.

---

# Step 5: Creating our first Error

Let's create a route.

```javascript
app.get("/error",(req,res)=>{

    throw new Error("Something went wrong");

});
```

Now visit

```
localhost:8000/error
```

You'll notice...

Our middleware still doesn't run.

Why?

Because Express **doesn't automatically catch synchronous throw everywhere the way you might expect across all cases**, and in async handlers it definitely won't. The common Express pattern is to **pass the error to `next()`**.

---

# Step 6: next(error)

Instead of

```javascript
throw new Error("Something went wrong");
```

do

```javascript
app.get("/error",(req,res,next)=>{

    const error = new Error("Something went wrong");

    next(error);

});
```

This means

> "Express, I'm giving this error to you."

---

# Step 7: Receive that Error

Now write

```javascript
app.use((err,req,res,next)=>{

    console.log(err);

});
```

Visit

```
/error
```

Console

```
Error: Something went wrong
```

🎉

You just caught your first error.

---

# Step 8: Send Response

Instead of only printing

```javascript
console.log(err);
```

Send response.

```javascript
app.use((err,req,res,next)=>{

    res.status(500).json({

        message:err.message

    });

});
```

Now browser gets

```json
{
    "message":"Something went wrong"
}
```

instead of crashing.

---

# Step 9: Complete Flow

```text
Browser
    │
    ▼
GET /error
    │
    ▼
Route
    │
    ▼
next(error)
    │
    ▼
Express
    │
    ▼
Error Middleware
    │
    ▼
JSON Response
```

---

# Step 10: Let's make another Error

```javascript
app.get("/student",(req,res,next)=>{

    const student = null;

    if(!student){

        return next(new Error("Student not found"));

    }

});
```

Browser

```json
{
   "message":"Student not found"
}
```

Different route

Same middleware.

---

# Step 11: Why is this useful?

Without middleware

```javascript
app.get(...)

try{

}
catch(error){

    res.status(500)...

}
```

Every controller.

Again

Again

Again

Again

20 controllers

↓

20 try catch blocks.

With middleware

```javascript
next(error)
```

Everything goes here

```javascript
app.use(errorHandler);
```

One place.

---

# Step 12: Industry Version

Later you'll write

```javascript
const errorHandler=(err,req,res,next)=>{

    res.status(err.statusCode || 500).json({

        success:false,

        message:err.message

    });

}
```

And every controller simply says

```javascript
next(new Error("Student not found"));
```

No duplicate response logic.

---

# Your Learning Path

Don't jump to the final version immediately. Practice in this order:

1. ✅ Create an error manually with `next(new Error(...))`.
    
2. ✅ Write an error-handling middleware that logs the error.
    
3. ✅ Modify it to send a JSON response.
    
4. ✅ Add custom status codes (e.g., 404, 400, 500).
    
5. ✅ Learn `asyncHandler` to eliminate repetitive `try...catch` in async routes.
    

This progression will make the concept much easier to understand than starting with the production pattern.


---

Excellent. Phase 1 was:

- ✅ What is an error?
    
- ✅ `next(error)`
    
- ✅ Error middleware
    
- ✅ `(err, req, res, next)`
    

Now we'll move to **Phase 2**, where we learn **why we attach extra information to errors** instead of always returning `500`.

---

# Phase 2: Custom Errors (Status Codes)

## Problem

Suppose you write:

```javascript
app.get("/student/:id", (req, res, next) => {
  next(new Error("Student not found"));
});
```

Your middleware:

```javascript
app.use((err, req, res, next) => {
  res.status(500).json({
    message: err.message,
  });
});
```

Response:

```json
{
  "message": "Student not found"
}
```

Status Code:

```text
500 Internal Server Error
```

❌ But is this really a server error?

No.

The server is working perfectly.

The problem is that **the requested student doesn't exist**.

The correct status should be:

```text
404 Not Found
```

---

# Step 1: Attach a Status Code

JavaScript `Error` objects are just objects.

You can add your own properties.

```javascript
app.get("/student/:id", (req, res, next) => {
  const error = new Error("Student not found");

  error.status = 404;

  next(error);
});
```

Notice:

```javascript
error.status = 404;
```

We're adding our own property.

---

# Step 2: Read It in Middleware

Instead of:

```javascript
res.status(500)
```

write:

```javascript
app.use((err, req, res, next) => {
  res.status(err.status || 500).json({
    message: err.message,
  });
});
```

Now Express does:

```
If error.status exists
        ↓
Use it

Else
        ↓
Use 500
```

---

# Step 3: Test

### Route 1

```javascript
app.get("/student", (req, res, next) => {
  const error = new Error("Student not found");

  error.status = 404;

  next(error);
});
```

Response:

```http
404 Not Found
```

```json
{
  "message": "Student not found"
}
```

---

### Route 2

```javascript
app.get("/server", (req, res, next) => {
  const error = new Error("Database crashed");

  next(error);
});
```

No status attached.

Response:

```http
500 Internal Server Error
```

```json
{
  "message": "Database crashed"
}
```

---

# Step 4: Another Example

Bad request.

```javascript
app.get("/login", (req, res, next) => {
  const error = new Error("Email is required");

  error.status = 400;

  next(error);
});
```

Response:

```http
400 Bad Request
```

---

Unauthorized.

```javascript
app.get("/admin", (req, res, next) => {
  const error = new Error("Login required");

  error.status = 401;

  next(error);
});
```

Response:

```http
401 Unauthorized
```

---

Forbidden.

```javascript
app.get("/secret", (req, res, next) => {
  const error = new Error("Access denied");

  error.status = 403;

  next(error);
});
```

---

# Phase 2 Summary

```javascript
const error = new Error("Student not found");

error.status = 404;

next(error);
```

↓

```javascript
app.use((err, req, res, next) => {
  res.status(err.status || 500).json({
    message: err.message,
  });
});
```

---

# Why this matters

Imagine a real project with:

- User login
    
- Student CRUD
    
- Products
    
- Orders
    
- Payments
    

Different situations require different HTTP status codes:

- `400` → Invalid input
    
- `401` → Not logged in
    
- `403` → Logged in but not allowed
    
- `404` → Resource not found
    
- `409` → Conflict (e.g., email already exists)
    
- `500` → Unexpected server error
    

Using a custom `status` property lets one centralized error middleware handle all of these consistently.

---

## Next Phase (Phase 3)

We'll make this even cleaner by creating a reusable **`ApiError` class**:

```javascript
throw new ApiError(404, "Student not found");
```

instead of writing:

```javascript
const error = new Error("Student not found");
error.status = 404;
next(error);
```

This is the pattern used in many production Express applications.

---

Excellent. Now you're entering the **professional way** of handling errors in Express.

# Phase 3 — Custom Error Class (`ApiError`)

This is where many developers first see OOP (Object-Oriented Programming) applied in backend development.

---

# Why do we need a Custom Error Class?

In Phase 2 we wrote:

```javascript
const error = new Error("Student not found");

error.status = 404;

next(error);
```

It works.

But imagine a project with **100+ controllers**.

You'll repeatedly write:

```javascript
const error = new Error("User not found");
error.status = 404;
next(error);
```

Again...

Again...

Again...

Again...

This is repetitive.

Instead, create one class that does everything.

---

# Understanding Classes

Before writing `ApiError`, understand what a class is.

Example:

```javascript
class Student {

    constructor(name, age){

        this.name = name;
        this.age = age;

    }

}
```

Create object

```javascript
const s1 = new Student("Pranav",20);

console.log(s1);
```

Output

```text
Student {

 name:"Pranav",

 age:20

}
```

The constructor automatically creates the object.

---

# Error is also a Class

When you write

```javascript
const error = new Error("Something wrong");
```

You're actually doing

```javascript
new Error(...)
```

which means

> Error itself is a class.

We can extend it.

---

# What does `extends` mean?

Suppose

```javascript
class Animal{

    eat(){

        console.log("Eating");

    }

}
```

Dog is also an animal.

```javascript
class Dog extends Animal{

}
```

Now

```javascript
const d = new Dog();

d.eat();
```

Output

```text
Eating
```

Dog inherited Animal.

---

Same idea.

```javascript
class ApiError extends Error{

}
```

ApiError inherits everything from Error.

---

# First ApiError

```javascript
class ApiError extends Error{

    constructor(message){

        super(message);

    }

}
```

Notice

```javascript
super(message)
```

What is super?

---

# What is super()?

Parent class

```javascript
class Animal{

    constructor(name){

        this.name = name;

    }

}
```

Child

```javascript
class Dog extends Animal{

    constructor(name){

        super(name);

    }

}
```

`super()` calls the parent constructor.

Exactly the same.

Error's constructor accepts

```javascript
new Error(message)
```

So

```javascript
super(message)
```

means

```javascript
Error(message)
```

---

# Add Status Code

Now improve it.

```javascript
class ApiError extends Error{

    constructor(statusCode,message){

        super(message);

        this.statusCode=statusCode;

    }

}
```

Now create one.

```javascript
const error=new ApiError(

404,

"Student not found"

);

console.log(error);
```

Output

```text
ApiError {

 message:"Student not found",

 statusCode:404

}
```

Amazing.

We don't need

```javascript
error.status=404;
```

anymore.

---

# Folder Structure

Industry

```text
src

|

utils

   |

   ApiError.js
```

---

ApiError.js

```javascript
class ApiError extends Error{

    constructor(statusCode,message){

        super(message);

        this.statusCode=statusCode;

    }

}

module.exports=ApiError;
```

---

# Using It

Controller

```javascript
const ApiError=require("../utils/ApiError");
```

Instead of

```javascript
const error=new Error(

"Student not found"

);

error.status=404;

next(error);
```

Write

```javascript
next(

new ApiError(

404,

"Student not found"

)

);
```

Much cleaner.

---

# Error Middleware

Instead of

```javascript
res.status(

err.status ||500

)
```

Now

```javascript
app.use((err,req,res,next)=>{

res.status(

err.statusCode ||500

).json({

success:false,

message:err.message

});

});
```

Done.

---

# Complete Flow

```text
Controller

       |

Student not found

       |

new ApiError(404,"Student not found")

       |

next(error)

       |

Error Middleware

       |

err.statusCode

err.message

       |

JSON Response
```

---

# Real Example

```javascript
app.get("/student/:id",(req,res,next)=>{

const found=false;

if(!found){

return next(

new ApiError(

404,

"Student not found"

)

);

}

});
```

Response

```json
{
    "success":false,
    "message":"Student not found"
}
```

Status

```text
404
```

---

# Why Big Companies Use This

Imagine these errors

```text
User not found

Email already exists

Token expired

Password incorrect

Product unavailable

Order cancelled

Payment failed
```

Instead of

```javascript
const error=new Error(...);

error.status=...

next(error);
```

Everywhere

they simply do

```javascript
throw new ApiError(404,"User not found");
```

or

```javascript
return next(

new ApiError(

401,

"Login required"

)

);
```

One clean line.

---

# Phase 3 Summary

### Normal Error

```javascript
const error=new Error("User not found");

error.status=404;

next(error);
```

### ApiError

```javascript
next(

new ApiError(

404,

"User not found"

)

);
```

Cleaner, reusable, and easier to maintain.

---

# One important note

This is **not the final stage** of error handling. The next step in professional Express apps is **Phase 4: `asyncHandler`**, where you'll remove repetitive `try...catch` blocks from your async controllers. It's one of the biggest quality-of-life improvements in Express development and naturally builds on `ApiError`.


---

# Phase 4 — asyncHandler (Beginner Version)

## Chapter 1 — The Real Problem

Let's say we have one controller.

```javascript
const getStudents = async (req, res, next) => {
  try {
    const students = await Student.find();

    res.json(students);
  } catch (error) {
    next(error);
  }
};
```

Looks fine.

Now write another controller.

```javascript
const createStudent = async (req, res, next) => {
  try {
    const student = await Student.create(req.body);

    res.json(student);
  } catch (error) {
    next(error);
  }
};
```

Another one.

```javascript
const deleteStudent = async (req, res, next) => {
  try {
    await Student.findByIdAndDelete(req.params.id);

    res.json({
      message: "Deleted",
    });
  } catch (error) {
    next(error);
  }
};
```

Notice something?

---

## What changes?

Only this part changes:

```javascript
await Student.find();
```

or

```javascript
await Student.create();
```

or

```javascript
await Student.findById();
```

Everything else is identical.

```javascript
try {

}
catch(error){

next(error);

}
```

This is called **duplicate code**.

---

# Chapter 2 — What is Duplicate Code?

Imagine this.

You write

```javascript
console.log("Hello");
```

100 times.

Bad idea.

Instead

```javascript
function sayHello() {
    console.log("Hello");
}
```

Now

```javascript
sayHello();
sayHello();
sayHello();
```

Much better.

You moved repeated code into one function.

---

Exactly the same thing happens here.

Instead of writing

```javascript
try {

}
catch(error){

next(error);

}
```

100 times

We move it into one function.

That function is called

```text
asyncHandler
```

---

# Chapter 3 — First asyncHandler

Forget Express.

Forget MongoDB.

Forget errors.

Let's make a normal function.

```javascript
function hello() {
    console.log("Hello");
}
```

Now another function.

```javascript
function runFunction(fn) {

    fn();

}
```

Call it.

```javascript
runFunction(hello);
```

Output

```
Hello
```

Question:

What is

```javascript
fn
```

?

Answer:

It is another function.

So

```javascript
runFunction(hello);
```

means

```
hello becomes fn
```

---

# Chapter 4 — Controllers are also Functions

Look carefully.

```javascript
const getStudents = async (req, res) => {

};
```

Is this a function?

YES.

Which means

We can pass it into another function.

Exactly like

```javascript
hello
```

---

Instead of

```javascript
app.get("/students", getStudents);
```

we can write

```javascript
app.get(
    "/students",
    asyncHandler(getStudents)
);
```

Question:

What is inside

```javascript
asyncHandler(...)
```

?

Answer:

```
getStudents
```

The controller itself.

---

# Chapter 5 — Build asyncHandler

Start with nothing.

```javascript
function asyncHandler(fn) {

}
```

Question:

What is

```javascript
fn
```

?

Answer

```
getStudents
```

or

```
createStudent
```

or

```
deleteStudent
```

Any controller.

---

# Chapter 6 — But Express Wants Something

Express expects this

```javascript
(req, res, next) => {

}
```

because

```javascript
app.get("/", something_here);
```

always receives

```javascript
req
res
next
```

So our asyncHandler must RETURN another function.

```javascript
function asyncHandler(fn) {

    return function(req, res, next) {

    };

}
```

Now Express is happy.

---

# Chapter 7 — Call the Original Controller

Inside

```javascript
return function(req,res,next){

}
```

call

```javascript
fn(req,res,next);
```

Complete

```javascript
function asyncHandler(fn){

    return function(req,res,next){

        fn(req,res,next);

    };

}
```

Question:

What is

```javascript
fn
```

?

Suppose

```javascript
app.get(
    "/students",
    asyncHandler(getStudents)
);
```

Then

```javascript
fn
```

becomes

```javascript
getStudents
```

So

```javascript
fn(req,res,next);
```

actually means

```javascript
getStudents(req,res,next);
```

Now the controller executes.

---

# Chapter 8 — Why Doesn't This Work?

Suppose

```javascript
const getStudents = async () => {

    throw new Error("Database Error");

};
```

Remember

Async functions return

```
Promise
```

Not a normal value.

When an async function throws an error

It doesn't throw immediately.

It creates a

```
Rejected Promise
```

Imagine

```
Promise

↓

Success

OR

↓

Failure
```

When it fails

Nobody catches it.

---

# Chapter 9 — Promise.catch()

Example

```javascript
Student.find()
```

returns

```
Promise
```

We can write

```javascript
Student.find()

.catch(error => {

    console.log(error);

});
```

Whenever an error happens

`.catch()` receives it.

---

# Chapter 10 — Instead of console.log()

Instead of

```javascript
.catch(error => {

    console.log(error);

});
```

We already have

```javascript
next(error);
```

So simply write

```javascript
.catch(next);
```

Because

This

```javascript
.catch(error => {

    next(error);

});
```

and this

```javascript
.catch(next);
```

are exactly the same.

---

# Chapter 11 — Final asyncHandler

Now replace

```javascript
fn(req,res,next);
```

with

```javascript
Promise

.resolve(

fn(req,res,next)

)

.catch(next);
```

Complete

```javascript
function asyncHandler(fn){

    return function(req,res,next){

        Promise

        .resolve(

            fn(req,res,next)

        )

        .catch(next);

    };

}
```

Don't memorize it.

Understand it.

It says

```
Run the controller

↓

If controller succeeds

↓

Nothing happens

↓

If controller throws error

↓

Call next(error)

↓

Error Middleware
```

---

# Chapter 12 — Picture

Without asyncHandler

```
Controller

↓

try

↓

Database

↓

catch

↓

next(error)
```

Every controller.

---

With asyncHandler

```
Controller

↓

Database

↓

throw error

↓

asyncHandler catches

↓

next(error)

↓

Error Middleware
```

Controllers become tiny.

---

# Before

```javascript
const getStudents = async(req,res,next)=>{

try{

const students=await Student.find();

res.json(students);

}

catch(error){

next(error);

}

}
```

---

# After

```javascript
const getStudents = asyncHandler(

async(req,res)=>{

const students=await Student.find();

res.json(students);

}

);
```

Notice

No

```
try
```

No

```
catch
```

No

```
next(error)
```

Cleaner.

---

# One Sentence to Remember

> **`asyncHandler` is just a wrapper function that executes your async controller and automatically forwards any errors to Express's error-handling middleware.**

---

## I recommend one more step before moving on

Don't try to memorize the final `asyncHandler` implementation yet.

Instead, build it yourself in **4 small versions**:

1. Version 1: A function that accepts another function.
    
2. Version 2: A function that returns another function.
    
3. Version 3: A wrapper that calls the passed function.
    
4. Version 4: Add `Promise.resolve(...).catch(next)`.
    

Building it incrementally makes the implementation much easier to understand than starting with the finished code.


---

#DOUBT - `Is anything left after phase 4`

Yes. In fact, what you've learned so far is only the **foundation**. Professional Express applications build several more layers on top of it.

Here's the roadmap I recommend after Phase 4.

---

# ✅ Phase 1 — Basic Error Middleware (Done)

Learned:

- What is an error
    
- `next(error)`
    
- Error middleware
    
- `(err, req, res, next)`
    

Example:

```js
next(new Error("Something went wrong"));
```

---

# ✅ Phase 2 — Custom Status Codes (Done)

Learned:

```js
const error = new Error("Student not found");
error.status = 404;

next(error);
```

---

# ✅ Phase 3 — ApiError Class (Done)

Learned:

```js
throw new ApiError(404, "Student not found");
```

No need to manually set `error.status`.

---

# ✅ Phase 4 — asyncHandler (Done)

Learned:

Instead of

```js
try {
  ...
} catch (error) {
  next(error);
}
```

everywhere,

use

```js
asyncHandler(async (req, res) => {
  ...
});
```

---

# 🚀 Phase 5 — Production Error Handler (Industry Standard)

This is where almost every professional backend differs from beginner projects.

Instead of sending:

```json
{
  "message": "Something went wrong"
}
```

we send a **consistent error format**.

Example:

```json
{
  "success": false,
  "statusCode": 404,
  "message": "Student not found"
}
```

or

```json
{
  "success": false,
  "statusCode": 400,
  "message": "Email is required",
  "errors": [
    "Email is required"
  ]
}
```

Every API returns errors in the same structure.

---

# 🚀 Phase 6 — Handling Mongoose Errors

MongoDB produces different kinds of errors.

Examples:

```text
ValidationError
CastError
Duplicate Key Error
```

Example:

```js
POST /students
```

```json
{
  "email": "alreadyexists@gmail.com"
}
```

Instead of exposing the raw MongoDB error, convert it to:

```json
{
  "message": "Email already exists"
}
```

---

# 🚀 Phase 7 — Custom Error Types

Create specialized error classes.

Example:

```js
class NotFoundError extends ApiError {}

class ValidationError extends ApiError {}

class UnauthorizedError extends ApiError {}
```

Then simply write:

```js
throw new NotFoundError("Student not found");
```

or

```js
throw new UnauthorizedError("Login required");
```

---

# 🚀 Phase 8 — Global 404 Handler

Instead of Express's default response:

```text
Cannot GET /abcd
```

you return:

```json
{
  "success": false,
  "message": "Route not found"
}
```

---

# 🚀 Phase 9 — Logging Errors

Instead of only:

```js
console.log(error);
```

store them in logs.

Examples:

- Winston
    
- Pino
    
- Morgan (HTTP logging)
    

Useful for debugging production issues.

---

# 🚀 Phase 10 — Environment-Based Errors

Development:

```json
{
  "message": "Student not found",
  "stack": "...full stack trace..."
}
```

Production:

```json
{
  "message": "Something went wrong"
}
```

Never expose stack traces to users in production.

---

# 🚀 Phase 11 — Error Monitoring

Large companies don't just log errors—they monitor them.

Tools include:

- Sentry
    
- Bugsnag
    
- Rollbar
    

When your server crashes, they notify you with details.

---

# Error Handling Roadmap

```text
Phase 1 ✅
Basic Error Middleware

        ↓

Phase 2 ✅
Status Codes

        ↓

Phase 3 ✅
ApiError

        ↓

Phase 4 ✅
asyncHandler

        ↓

Phase 5 ⭐
Professional Error Middleware

        ↓

Phase 6 ⭐
Handle Mongoose Errors

        ↓

Phase 7 ⭐
Custom Error Classes

        ↓

Phase 8 ⭐
404 Route Handler

        ↓

Phase 9 ⭐
Logging

        ↓

Phase 10 ⭐
Development vs Production

        ↓

Phase 11 ⭐
Monitoring (Sentry, etc.)
```

## For the 80/20 rule

If your goal is to become productive with Express and MongoDB quickly, I'd prioritize:

- ✅ Phase 1–4 (essential foundation)
    
- ⭐ Phase 5 (consistent error responses)
    
- ⭐ Phase 6 (handle Mongoose errors)
    
- ⭐ Phase 8 (404 handler)
    

These cover the vast majority of error handling you'll implement in everyday Express APIs. Phases 9–11 become more important as your applications move toward production deployment and larger teams.