
---

Let's walk through the error handling middleware in your code.

You have:

```js
app.use((err, req, res, next) => {
  if (err instanceof multer.MulterError) {
    return res.status(400).send(err.message);
  }
  next(err);
});
```

### Why is this different from normal middleware?

A normal middleware looks like:

```js
app.use((req, res, next) => {
  next();
});
```

An **error-handling middleware** has **4 parameters**:

```js
(err, req, res, next)
```

Express recognizes it as an error handler because of the `err` parameter.

---

### When does it run?

Suppose a user uploads a file larger than 5 MB.

Your Multer configuration:

```js
limits: {
  fileSize: 1024 * 1024 * 5
}
```

When a 10 MB file is uploaded:

```js
upload.single("profile")
```

Multer throws an error:

```js
MulterError: File too large
```

Instead of reaching your route handler:

```js
(req, res) => {
  res.send(req.file);
}
```

Express skips directly to:

```js
app.use((err, req, res, next) => {
  ...
});
```

and `err` contains the Multer error.

---

### What does this line do?

```js
if (err instanceof multer.MulterError)
```

It checks:

> "Is this error created by Multer?"

Examples of Multer errors:

- File too large
    
- Too many files
    
- Unexpected field name
    

If yes:

```js
return res.status(400).send(err.message);
```

Response:

```txt
File too large
```

---

### What happens for other errors?

Suppose you write:

```js
app.post("/single-upload", upload.single("profile"), (req, res) => {
  throw new Error("Database failed");
});
```

Now:

```js
err instanceof multer.MulterError
```

is `false`.

So Express executes:

```js
next(err);
```

which passes the error to the next error middleware.

If there is no next error middleware, Express sends a default error page.

---

### Better version

Usually we handle both Multer and non-Multer errors:

```js
app.use((err, req, res, next) => {
  if (err instanceof multer.MulterError) {
    return res.status(400).send(err.message);
  }

  return res.status(500).send(err.message);
});
```

Now:

|Error Type|Response|
|---|---|
|File too large|400|
|Unexpected field|400|
|Database error|500|
|Any other server error|500|

---

### Flow Diagram

```text
User uploads file
        |
        v
upload.single("profile")
        |
        +---- No Error ----> Route Handler
        |
        +---- Error -----> Error Middleware
                              |
                              v
                    err instanceof MulterError ?
                         /           \
                       Yes            No
                        |              |
                    400 Error      500 Error
```

The key idea is that `upload.single("profile")` can generate errors before your route handler runs, and Express forwards those errors to the error-handling middleware automatically.


---

#DOUBT - **Didn't understand next(err)**

`next(err)` means:

> "I am not handling this error here. Pass it to the next error-handling middleware."

Consider this code:

```js
app.use((err, req, res, next) => {
  if (err instanceof multer.MulterError) {
    return res.status(400).send(err.message);
  }

  next(err);
});
```

Suppose the error is:

```js
new Error("Database connection failed");
```

Then:

```js
err instanceof multer.MulterError
```

is `false`.

So Express executes:

```js
next(err);
```

which means:

```text
Current Error Middleware
          |
          v
      next(err)
          |
          v
Next Error Middleware
```

Example:

```js
app.use((err, req, res, next) => {
  if (err instanceof multer.MulterError) {
    return res.status(400).send(err.message);
  }

  next(err);
});

app.use((err, req, res, next) => {
  res.status(500).send("Something went wrong");
});
```

Flow:

```text
Database Error
      |
      v
First Error Middleware
      |
      | not a MulterError
      v
next(err)
      |
      v
Second Error Middleware
      |
      v
"Something went wrong"
```

---

Why not just do nothing?

```js
app.use((err, req, res, next) => {
  if (err instanceof multer.MulterError) {
    return res.status(400).send(err.message);
  }
});
```

Now if the error is not from Multer:

```js
new Error("Database failed")
```

nothing sends a response and the request may hang or Express may use its default error handler.

That's why we either:

### Pass it on

```js
next(err);
```

or

### Handle it ourselves

```js
app.use((err, req, res, next) => {
  if (err instanceof multer.MulterError) {
    return res.status(400).send(err.message);
  }

  res.status(500).send(err.message);
});
```

In this version, `next(err)` is not needed because you're handling all remaining errors yourself.

A simple way to remember:

- `next()` → go to the next normal middleware.
    
- `next(err)` → go to the next error middleware.
    
- `res.send()` / `res.json()` → end the request-response cycle. No need to call `next()`.


---

### What is `fileFilter` in Multer?

`fileFilter` is a function that lets you **accept or reject files before they are uploaded**.

Without `fileFilter`, Multer accepts any file type:

```txt
photo.jpg  ✅
resume.pdf ✅
video.mp4  ✅
virus.exe  ✅
```

With `fileFilter`, you can allow only specific types.

---

## Syntax

```js
const upload = multer({
  storage,
  fileFilter: (req, file, cb) => {
    // validation logic
  }
});
```

Parameters:

```js
(req, file, cb)
```

- `req` → Express request object
    
- `file` → Information about the uploaded file
    
- `cb` → Callback function
    

---

## Understanding `file`

Suppose a user uploads:

```txt
myphoto.jpg
```

The `file` object looks similar to:

```js
{
  fieldname: 'profile',
  originalname: 'myphoto.jpg',
  encoding: '7bit',
  mimetype: 'image/jpeg'
}
```

The most useful property is:

```js
file.mimetype
```

Examples:

|File|MIME Type|
|---|---|
|jpg|image/jpeg|
|png|image/png|
|webp|image/webp|
|pdf|application/pdf|
|mp4|video/mp4|

---

## Accept All Files

```js
fileFilter: (req, file, cb) => {
  cb(null, true);
}
```

Meaning:

```txt
No error
Accept file
```

---

## Reject All Files

```js
fileFilter: (req, file, cb) => {
  cb(null, false);
}
```

Meaning:

```txt
No error
Do not save file
```

Then:

```js
req.file
```

will be `undefined`.

---

## Allow Only Images

```js
const upload = multer({
  storage,
  fileFilter: (req, file, cb) => {
    const allowedTypes = [
      "image/jpeg",
      "image/png",
      "image/webp"
    ];

    if (allowedTypes.includes(file.mimetype)) {
      cb(null, true);
    } else {
      cb(new Error("Only image files are allowed"));
    }
  }
});
```

### Uploading JPG

```txt
photo.jpg
```

MIME type:

```txt
image/jpeg
```

Result:

```txt
Accepted
```

---

### Uploading PDF

```txt
resume.pdf
```

MIME type:

```txt
application/pdf
```

Result:

```txt
Error: Only image files are allowed
```

---

## Using Regular Expressions

A common approach:

```js
fileFilter: (req, file, cb) => {
  const allowed = /jpeg|jpg|png|webp/;

  const ext = allowed.test(
    path.extname(file.originalname).toLowerCase()
  );

  const mime = allowed.test(file.mimetype);

  if (ext && mime) {
    cb(null, true);
  } else {
    cb(new Error("Images only"));
  }
}
```

This checks:

1. File extension
    
2. MIME type
    

Both must match.

---

## How Errors Reach Your Error Middleware

If you do:

```js
cb(new Error("Images only"));
```

Multer passes the error to Express.

```js
app.use((err, req, res, next) => {
  res.status(400).send(err.message);
});
```

Output:

```txt
Images only
```

---

## Complete Example

```js
const upload = multer({
  storage,
  limits: {
    fileSize: 1024 * 1024 * 5
  },

  fileFilter: (req, file, cb) => {
    const allowedTypes = [
      "image/jpeg",
      "image/png",
      "image/webp"
    ];

    if (allowedTypes.includes(file.mimetype)) {
      cb(null, true);
    } else {
      cb(new Error("Only JPG, PNG and WEBP files are allowed"));
    }
  }
});
```

Route:

```js
app.post(
  "/single-upload",
  upload.single("profile"),
  (req, res) => {
    res.send(req.file);
  }
);
```

Error handler:

```js
app.use((err, req, res, next) => {
  res.status(400).send(err.message);
});
```

### Flow

```text
User uploads file
        |
        v
    fileFilter
        |
   +----+----+
   |         |
 Valid    Invalid
   |         |
   v         v
 Save     Error
 File      |
   |        |
   +--------+
        |
        v
 Error Middleware
```

The callback `cb` is the key:

```js
cb(null, true);   // accept file
cb(null, false);  // reject silently
cb(new Error());  // reject with error
```

Those are the three outcomes you'll use most often in `fileFilter`.


---

