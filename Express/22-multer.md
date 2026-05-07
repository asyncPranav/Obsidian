
---


# Complete Multer Notes (Very Detailed)

# 1. What Is Multer?

Multer is a middleware for handling:

```text
multipart/form-data
```

which is mainly used for:

- file uploads
    
- image uploads
    
- PDF uploads
    
- document uploads
    

It works with:

- Express.js
    
- Node.js
    

---

# 2. Why Multer Is Needed

# Main Question

> “Why can Express not upload files by itself?”

By default:

```js
req.body
```

can only handle:

- text
    
- JSON
    
- URL encoded data
    

BUT files are binary data.

When a form uploads files:

```html
<input type="file">
```

browser sends data in:

```text
multipart/form-data
```

Express alone cannot parse this format.

That’s why Multer exists.

---

# 3. What Is multipart/form-data?

# Question Driven Learning

## Question

> “Why normal forms work without multer but file uploads don’t?”

Because normal forms send:

```text
application/x-www-form-urlencoded
```

OR

```text
application/json
```

But file uploads require:

```text
multipart/form-data
```

---

# What Happens Internally?

Browser splits request into multiple parts:

Example:

```text
Part 1 → username
Part 2 → email
Part 3 → image file
```

Each part contains:

- headers
    
- content
    
- metadata
    

Multer parses these parts.

---

# 4. Installing Multer

```bash
npm install multer
```

---

# 5. Basic Syntax

```js
const multer = require("multer");
```

---

# 6. Simplest Multer Example

```js
const express = require("express");
const multer = require("multer");

const app = express();

const upload = multer({ dest: "uploads/" });

app.post("/upload", upload.single("image"), (req, res) => {
  res.send("File uploaded");
});

app.listen(3000);
```

---

# Question Driven Understanding

## Question

> “What is upload.single("image")?”

It is middleware.

It tells multer:

```text
Accept ONE file from field named "image"
```

---

# Important

Field name MUST match HTML input name.

Example:

```html
<input type="file" name="image">
```

---

# 7. HTML Form For Multer

```html
<form action="/upload" method="POST" enctype="multipart/form-data">

  <input type="file" name="image">

  <button type="submit">
    Upload
  </button>

</form>
```

---

# MOST IMPORTANT ATTRIBUTE

```html
enctype="multipart/form-data"
```

Without this:

- file upload fails
    
- req.file becomes undefined
    

---

# Question

> “Why enctype is required?”

Because browser must know:

- request contains binary file data
    

---

# 8. Understanding req.file

After upload:

```js
console.log(req.file);
```

Example output:

```js
{
  fieldname: 'image',
  originalname: 'photo.png',
  encoding: '7bit',
  mimetype: 'image/png',
  destination: 'uploads/',
  filename: 'abc123.png',
  path: 'uploads/abc123.png',
  size: 34567
}
```

---

# Important Properties

|Property|Meaning|
|---|---|
|fieldname|input field name|
|originalname|original uploaded filename|
|mimetype|file type|
|destination|upload folder|
|filename|stored filename|
|path|full file path|
|size|file size in bytes|

---

# 9. upload.single()

# Syntax

```js
upload.single(fieldname)
```

# Example

```js
upload.single("profile")
```

Accepts:

- only ONE file
    

Stores file in:

```js
req.file
```

---

# 10. upload.array()

# Question

> “What if user uploads multiple files?”

Use:

```js
upload.array(fieldname, maxCount)
```

Example:

```js
upload.array("photos", 5)
```

Accepts:

- multiple files
    

Stores files in:

```js
req.files
```

---

# HTML Example

```html
<input type="file" name="photos" multiple>
```

---

# 11. upload.fields()

Used for multiple DIFFERENT fields.

Example:

```js
upload.fields([
  { name: "avatar", maxCount: 1 },
  { name: "resume", maxCount: 1 }
])
```

Now:

- avatar upload
    
- resume upload
    

both supported.

---

# Accessing Files

```js
req.files.avatar
req.files.resume
```

---

# 12. upload.none()

Accepts:

- text only
    
- NO files
    

Example:

```js
upload.none()
```

---

# 13. upload.any()

Accepts:

- any files
    
- any field names
    

Example:

```js
upload.any()
```

Usually NOT recommended for security reasons.

---

# 14. Multer Middleware Flow

Very Important Concept.

```text
Request
   ↓
Multer Middleware
   ↓
req.file / req.files created
   ↓
Route Handler
   ↓
Response
```

---

# Question

> “Why multer runs before controller?”

Because controller needs processed file data.

Without multer:

- Express cannot understand uploaded files.
    

---

# 15. Storage Engine

# Question

> “Where does multer store files?”

Controlled by storage engine.

Two main types:

- Disk Storage
    
- Memory Storage
    

---

# 16. Disk Storage

Most commonly used.

# Syntax

```js
const storage = multer.diskStorage({
  destination: function(req, file, cb) {

  },

  filename: function(req, file, cb) {

  }
});
```

---

# Full Example

```js
const storage = multer.diskStorage({

  destination: function(req, file, cb) {
    cb(null, "uploads/");
  },

  filename: function(req, file, cb) {

    const uniqueName =
      Date.now() + "-" + file.originalname;

    cb(null, uniqueName);
  }

});
```

---

# Understanding cb()

# Question

> “What is cb?”

Callback function.

Syntax:

```js
cb(error, value)
```

Example:

```js
cb(null, "uploads/")
```

Means:

- no error
    
- use uploads folder
    

---

# destination()

Controls:

- upload folder location
    

Example:

```js
destination: function(req, file, cb) {
   cb(null, "uploads/");
}
```

---

# filename()

Controls:

- saved filename
    

Example:

```js
filename: function(req, file, cb) {
   cb(null, Date.now() + ".png");
}
```

---

# Question

> “Why customize filename?”

Because duplicate names can overwrite files.

---

# 17. Using Storage

```js
const upload = multer({
  storage: storage
});
```

---

# 18. Memory Storage

Stores file in RAM.

```js
const storage = multer.memoryStorage();
```

File available as:

```js
req.file.buffer
```

Used when:

- uploading directly to cloud
    
- processing image before saving
    

---

# WARNING

Large files can consume huge memory.

---

# 19. File Validation Using fileFilter

VERY IMPORTANT.

# Question

> “How to allow only images?”

Use:

```js
fileFilter
```

---

# Full Example

```js
const upload = multer({

  storage: storage,

  fileFilter: function(req, file, cb) {

    if (
      file.mimetype === "image/png" ||
      file.mimetype === "image/jpeg"
    ) {
      cb(null, true);
    }
    else {
      cb(new Error("Only images allowed"));
    }

  }

});
```

---

# Understanding fileFilter Parameters

|Parameter|Meaning|
|---|---|
|req|request object|
|file|uploaded file|
|cb|callback|

---

# Question

> “How multer knows file type?”

Using:

```js
file.mimetype
```

---

# Common MIME Types

|Type|MIME|
|---|---|
|PNG|image/png|
|JPG|image/jpeg|
|PDF|application/pdf|

---

# 20. File Size Limit

# Question

> “How to stop huge uploads?”

Use:

```js
limits
```

---

# Example

```js
const upload = multer({

  storage: storage,

  limits: {
    fileSize: 1024 * 1024 * 2
  }

});
```

Meaning:

- max 2MB
    

---

# Why 1024 * 1024?

Because:

- 1KB = 1024 bytes
    
- 1MB = 1024 × 1024 bytes
    

---

# 21. Complete Multer Configuration

```js
const multer = require("multer");

const storage = multer.diskStorage({

  destination: function(req, file, cb) {
    cb(null, "uploads/");
  },

  filename: function(req, file, cb) {

    const uniqueName =
      Date.now() + "-" + file.originalname;

    cb(null, uniqueName);
  }

});

const upload = multer({

  storage: storage,

  limits: {
    fileSize: 1024 * 1024 * 2
  },

  fileFilter: function(req, file, cb) {

    if (
      file.mimetype === "image/png" ||
      file.mimetype === "image/jpeg"
    ) {
      cb(null, true);
    }
    else {
      cb(new Error("Only PNG and JPG allowed"));
    }

  }

});
```

---

# 22. Serving Uploaded Files

# Question

> “How browser displays uploaded image?”

Need static middleware.

```js
app.use("/uploads", express.static("uploads"));
```

---

# Access Image

```text
/uploads/filename.png
```

---

# 23. Error Handling In Multer

# Common Errors

|Error|Reason|
|---|---|
|LIMIT_FILE_SIZE|file too large|
|invalid mimetype|wrong file type|
|req.file undefined|wrong field name|

---

# Central Error Handling

```js
app.use((err, req, res, next) => {

  res.status(500).send(err.message);

});
```

---

# Question

> “Why central error handling?”

Because:

- cleaner code
    
- reusable logic
    

---

# 24. Common Beginner Mistakes

# Mistake 1

Wrong field name.

```html
<input name="photo">
```

BUT:

```js
upload.single("image")
```

Result:

- req.file undefined
    

---

# Mistake 2

Forgot:

```html
enctype="multipart/form-data"
```

---

# Mistake 3

Uploads folder missing.

---

# Mistake 4

Using req.body for file.

Files come in:

```js
req.file
```

NOT:

```js
req.body
```

---

# 25. Multer + Validation

Example:

```js
app.post(
  "/upload",

  upload.single("image"),

  [
    body("username")
      .notEmpty()
  ],

  controller
);
```

---

# Middleware Order Matters

Question:

> “Why upload middleware before validation?”

Because multer must parse multipart/form-data first.

---

# 26. Real Request Lifecycle

```text
Browser Form
      ↓
multipart/form-data request
      ↓
Multer Middleware
      ↓
File parsed
      ↓
req.file created
      ↓
Validation Middleware
      ↓
Route Handler
      ↓
Database/File Saving
      ↓
Response
```

---

# 27. Question-Driven Learning For Multer

BEST SECTION.

---

# Beginner Questions

- Why Express cannot upload files alone?
    
- What is multipart/form-data?
    
- Why enctype needed?
    
- Why req.file exists separately?
    
- What is middleware role here?
    

---

# Intermediate Questions

- Why diskStorage needed?
    
- Why unique filenames important?
    
- How fileFilter works?
    
- How limits work?
    
- Why middleware order matters?
    

---

# Advanced Questions

- How multer parses streams internally?
    
- What is buffering?
    
- Why memoryStorage dangerous for large files?
    
- How cloud uploads work?
    
- How AWS S3 uploads work?
    

---

# 28. Mini Practice Tasks

# Task 1

Single profile upload.

---

# Task 2

Multiple image upload.

---

# Task 3

PDF upload system.

---

# Task 4

File type restriction.

---

# Task 5

Image gallery project.

---

# Task 6

Assignment submission system.

---

# 29. Real-World Uses Of Multer

|Application|Use|
|---|---|
|Instagram|image uploads|
|LinkedIn|resume upload|
|WhatsApp Web|media uploads|
|Google Drive|file storage|
|LMS portals|assignment uploads|

---

# 30. Final Mental Model

Think:

```text
Browser sends file
        ↓
Express cannot understand
        ↓
Multer intercepts request
        ↓
Parses multipart data
        ↓
Creates req.file / req.files
        ↓
Stores file
        ↓
Passes control using next()
```

That is Multer internally at beginner level.

---

# MOST IMPORTANT THINGS TO REMEMBER

## 1.

```html
enctype="multipart/form-data"
```

mandatory.

---

## 2.

Field names must match.

---

## 3.

Multer is middleware.

---

## 4.

Files come in:

- req.file
    
- req.files
    

---

## 5.

Use:

- fileFilter
    
- limits
    

for security.

---

## 6.

Middleware order matters.

---

# Final Learning Advice

Don’t just read multer.

Experiment aggressively:

- upload wrong files
    
- remove next()
    
- change field names
    
- exceed limits
    
- inspect req.file
    
- log everything
    

Breaking things teaches backend faster than watching tutorials.