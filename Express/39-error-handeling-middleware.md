

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