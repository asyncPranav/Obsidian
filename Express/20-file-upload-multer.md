
---

# File Uploads with Multer — Complete Beginner Notes

---

## 1. The "Why" — What Problem Does Multer Solve?

Before writing a single line of code, let's understand what actually happens when a user uploads a file through a browser form, because this is where most beginners get confused.

When you submit a regular HTML form with text fields, the browser encodes the data as a simple string that looks like `username=pranav&age=21`. This is called `application/x-www-form-urlencoded` encoding, and Express's built-in `express.urlencoded()` middleware handles it perfectly.

But a file is not a string. A file is raw binary data — it could be image pixels, PDF bytes, audio samples, anything. You cannot encode binary data as a simple key-value string. So when a form contains a file input, the browser switches to a completely different encoding format called **`multipart/form-data`**. This format splits the request body into multiple "parts", where each part can be either a text field or a raw file chunk.

Here is the problem: **Express has no built-in ability to parse `multipart/form-data`**. Neither `express.json()` nor `express.urlencoded()` can handle it. If you try to access `req.body` or `req.file` in a route that receives a file upload without any special middleware, you will get `undefined`.

This is exactly the gap that **Multer** fills. Multer is a middleware specifically designed to parse `multipart/form-data` requests. It reads the incoming file data, saves it (either to disk or to memory), and then hands you a clean `req.file` or `req.files` object in your route handler — just like `express.json()` hands you `req.body`.

---

## 2. The Mental Model — What Multer Does Internally

Think of Multer as a post office sorting room. When a request comes in containing a file, Multer intercepts it, opens the package, separates the file parts from the text parts, saves the files to a location you specify, and then neatly labels everything so your route handler can easily find it.

```
Incoming multipart/form-data Request
              ↓
         Multer Middleware
              ↓
   ┌──────────────────────────────┐
   │  Parses the request body     │
   │  Extracts text fields    ────┼──→  req.body
   │  Extracts file(s)        ────┼──→  req.file  (single)
   │  Saves file to disk/memory   │     req.files (multiple)
   └──────────────────────────────┘
              ↓
         Route Handler runs
    (req.file and req.body are now populated)
```

The key insight is that Multer populates **two** things simultaneously: `req.body` for the text fields, and `req.file` or `req.files` for the uploaded file(s). You get both in your route handler.

---

## 3. Installation

```bash
npm install multer
```

---

## 4. The Three Storage Options

The most important decision you make when configuring Multer is: **where should the uploaded file go?** Multer gives you three strategies, and understanding the tradeoff between them is essential.

### 4.1 Disk Storage — Saving Files to Your Server's File System

This is the most common option. The file is saved directly to a folder on your server's disk. You configure it using `multer.diskStorage()`, which accepts an object with two functions that Multer calls for each uploaded file.

```js
const multer = require("multer");
const path = require("path");

const storage = multer.diskStorage({

  // destination: tells Multer which folder to save the file in
  // cb is a callback — always call it as cb(error, folderPath)
  // passing null as the first argument means "no error"
  destination: function (req, file, cb) {
    cb(null, "uploads/"); // save to the "uploads/" folder
  },

  // filename: tells Multer what to name the saved file on disk
  // Without this, Multer saves files WITHOUT any extension, which breaks images
  filename: function (req, file, cb) {
    // file.fieldname    → the name attribute of the <input> (e.g., "avatar")
    // file.originalname → the original name from user's device (e.g., "photo.jpg")
    // path.extname()    → extracts the extension (e.g., ".jpg")
    // Date.now()        → adds a timestamp to ensure every filename is unique
    const ext = path.extname(file.originalname);
    cb(null, file.fieldname + "-" + Date.now() + ext);
    // Result: "avatar-1717000000000.jpg"
  }
});

const upload = multer({ storage: storage });
```

> 💡 **Why add `Date.now()` to the filename?** If two users both upload a file called `profile.jpg`, the second one would silently overwrite the first. Adding a timestamp makes every filename unique. In production, developers often use the `uuid` package for even more robust uniqueness.

### 4.2 Memory Storage — Keeping Files in RAM

With `memoryStorage()`, the file is never written to disk at all. Instead it lives in your server's RAM as a `Buffer` object, available via `req.file.buffer`.

```js
const storage = multer.memoryStorage();
const upload = multer({ storage: storage });
```

This is the right choice when you want to immediately process the file before saving it — for example, resizing an image with the `sharp` library, or uploading it directly to a cloud service like AWS S3 or Cloudinary without ever touching your disk. The tradeoff is that large files will eat your server's RAM, so use this carefully.

### 4.3 Default Storage

If you don't pass any storage option, Multer uses memory storage by default. It is always better to be explicit about which strategy you want so there are no surprises.

---

## 5. The `upload` Object — Your Middleware Factory

After configuring storage, calling `multer({ storage })` returns an `upload` object. This object has several methods, each of which returns a **middleware function** you plug directly into your routes. Think of `upload` as a factory that produces the right middleware depending on what kind of file upload you're handling.

---

## 6. Handling a Single File Upload

`upload.single("fieldname")` creates middleware that expects exactly **one file** from the form input whose `name` attribute matches the string you pass in. After it runs, the file information is available at `req.file`.

```js
const express = require("express");
const multer = require("multer");
const path = require("path");

const app = express();

const storage = multer.diskStorage({
  destination: (req, file, cb) => cb(null, "uploads/"),
  filename: (req, file, cb) => {
    const ext = path.extname(file.originalname);
    cb(null, file.fieldname + "-" + Date.now() + ext);
  }
});

const upload = multer({ storage });

// upload.single("avatar") runs as middleware BEFORE the route handler
// The string "avatar" MUST match the name attribute of the file input in HTML
app.post("/upload-profile", upload.single("avatar"), (req, res) => {

  // If no file was sent, req.file will be undefined
  if (!req.file) {
    return res.status(400).json({ message: "Please upload a file" });
  }

  // req.file contains everything about the uploaded file
  console.log(req.file);
  // {
  //   fieldname:    'avatar',
  //   originalname: 'myPhoto.jpg',
  //   encoding:     '7bit',
  //   mimetype:     'image/jpeg',
  //   destination:  'uploads/',
  //   filename:     'avatar-1717000000000.jpg',
  //   path:         'uploads/avatar-1717000000000.jpg',
  //   size:         245678   ← in bytes
  // }

  // req.body contains any text fields that were in the same form
  console.log(req.body); // e.g., { username: "pranav" }

  res.json({
    message: "File uploaded successfully!",
    filename: req.file.filename,
    path: req.file.path,
  });
});

app.listen(3000);
```

The corresponding HTML form **must** have `enctype="multipart/form-data"` — this is what tells the browser to use multipart encoding instead of the default URL-encoded format:

```html
<form action="/upload-profile" method="POST" enctype="multipart/form-data">
  <input type="text" name="username" placeholder="Your name" />
  <!-- "avatar" here must match upload.single("avatar") exactly -->
  <input type="file" name="avatar" />
  <button type="submit">Upload</button>
</form>
```

---

## 7. Handling Multiple Files from the Same Field

`upload.array("fieldname", maxCount)` handles multiple files from a **single file input** that has the `multiple` attribute. The second argument sets the maximum number of files allowed. All uploaded files land in `req.files` as an array.

```js
// Accepts up to 5 files from an input named "photos"
app.post("/upload-gallery", upload.array("photos", 5), (req, res) => {

  if (!req.files || req.files.length === 0) {
    return res.status(400).json({ message: "Please upload at least one file" });
  }

  // req.files is an array — each element has the same structure as req.file
  const uploadedFiles = req.files.map(file => ({
    filename: file.filename,
    size: file.size,
    mimetype: file.mimetype,
  }));

  res.json({ message: `${req.files.length} file(s) uploaded!`, files: uploadedFiles });
});
```

```html
<!-- The "multiple" attribute lets the user select several files at once -->
<form action="/upload-gallery" method="POST" enctype="multipart/form-data">
  <input type="file" name="photos" multiple />
  <button type="submit">Upload All</button>
</form>
```

---

## 8. Handling Multiple Files from Different Fields

`upload.fields()` is for forms that have **multiple separate file inputs**, each with a different `name`. A good real-world example is a job application form with separate inputs for a profile photo and a resume PDF.

```js
app.post("/apply", upload.fields([
  { name: "profilePic", maxCount: 1 },   // one photo
  { name: "resume", maxCount: 1 },       // one PDF
  { name: "certificates", maxCount: 5 }, // up to 5 certificates
]), (req, res) => {

  // req.files is now an OBJECT keyed by field name
  // Each value is an ARRAY (even if maxCount is 1), so grab index [0] for single files
  const profilePic = req.files["profilePic"]?.[0];
  const resume = req.files["resume"]?.[0];
  const certificates = req.files["certificates"]; // array of up to 5

  res.json({
    profilePic: profilePic?.filename,
    resume: resume?.filename,
    certificates: certificates?.map(f => f.filename),
    applicantName: req.body.name, // text field from the same form
  });
});
```

> 🔑 **The `req.file` vs `req.files` confusion:** With `upload.single()`, the result is `req.file` — a single object. With `upload.array()`, it's `req.files` — a flat array. With `upload.fields()`, it's `req.files` — an **object of arrays**, keyed by field name. This distinction trips up many beginners, so keep it front of mind.

---

## 9. Handling Forms with No Files

`upload.none()` is used when your form has `enctype="multipart/form-data"` but contains **no file inputs** — only text fields. Without Multer processing it, `req.body` would be empty.

```js
app.post("/text-only-form", upload.none(), (req, res) => {
  // req.body is now correctly populated even from a multipart form
  console.log(req.body);
  res.json({ received: req.body });
});
```

---

## 10. File Filtering — Only Allowing Certain File Types

Right now, if someone uploaded a `.exe` or `.sh` script to your server, Multer would happily save it. That's a real security risk. The `fileFilter` option lets you inspect each file before it's saved and decide whether to accept or reject it.

The filter function receives the request, the file object, and a callback `cb`. You call `cb(null, true)` to accept or `cb(null, false)` to silently reject. Passing an `Error` as the first argument of `cb` rejects the file and also triggers your error handler with that message.

```js
// A file filter that only allows image files
const imageFilter = (req, file, cb) => {

  // file.mimetype tells you the type of the file (e.g., "image/jpeg", "application/pdf")
  if (file.mimetype.startsWith("image/")) {
    cb(null, true); // ✅ accept the file
  } else {
    // ❌ reject the file with a meaningful error message
    cb(new Error("Only image files are allowed!"), false);
  }
};

const upload = multer({
  storage: storage,
  fileFilter: imageFilter,
});
```

For more precise control, you can check an allowlist of specific MIME types:

```js
const documentFilter = (req, file, cb) => {
  const allowedTypes = [
    "image/jpeg",
    "image/png",
    "image/webp",
    "application/pdf",
    "application/vnd.openxmlformats-officedocument.wordprocessingml.document", // .docx
  ];

  if (allowedTypes.includes(file.mimetype)) {
    cb(null, true);
  } else {
    cb(new Error("Only images and PDF/DOCX documents are allowed."), false);
  }
};
```

> ⚠️ **Security Note:** Never rely solely on MIME type checking. The MIME type is reported by the browser, and a malicious user can forge it. In production, also use a library like `file-type` which reads the actual file bytes (the "magic bytes") to confirm the real file type. For learning purposes, MIME type checking is a solid first layer.

---

## 11. Limiting File Size

The `limits` option prevents users from uploading files so large they could crash your server or fill your disk.

```js
const upload = multer({
  storage: storage,
  fileFilter: imageFilter,
  limits: {
    fileSize: 5 * 1024 * 1024, // 5 MB in bytes (5 × 1024 × 1024)
    files: 10,                  // maximum number of files per request
    fields: 10,                 // maximum number of non-file text fields
  }
});
```

The `fileSize` is always in bytes, which is why you see the `5 * 1024 * 1024` calculation — it's just a readable way of writing 5,242,880. If an uploaded file exceeds the limit, Multer throws a `MulterError` with the code `"LIMIT_FILE_SIZE"`.

---

## 12. Error Handling — The Critical Part

Error handling with Multer has a specific nuance you need to know. Multer throws **two distinct kinds of errors**:

`MulterError` is thrown by Multer itself for things like exceeding the file size limit or sending too many files. These errors have a `.code` property (e.g., `"LIMIT_FILE_SIZE"`) that you can use to give the user a precise message.

A regular `Error` is what you throw yourself inside the `fileFilter` function (e.g., "Only images allowed"). Multer catches this and forwards it to Express's error handling pipeline.

Both end up in your standard 4-parameter error-handling middleware, but you handle them differently:

```js
app.post("/upload", upload.single("avatar"), (req, res) => {
  res.json({ message: "Upload successful!", file: req.file });
});

// This error handler MUST come after all routes
app.use((err, req, res, next) => {

  if (err instanceof multer.MulterError) {
    // Multer-specific errors — each has a machine-readable .code property
    if (err.code === "LIMIT_FILE_SIZE") {
      return res.status(400).json({ error: "File is too large. Max size is 5MB." });
    }
    if (err.code === "LIMIT_FILE_COUNT") {
      return res.status(400).json({ error: "Too many files uploaded." });
    }
    if (err.code === "LIMIT_UNEXPECTED_FILE") {
      return res.status(400).json({ error: "Unexpected field name in upload." });
    }
    return res.status(400).json({ error: err.message });

  } else if (err) {
    // Errors thrown by our own fileFilter (wrong type, etc.)
    return res.status(400).json({ error: err.message });
  }

  next();
});
```

---

## 13. A Complete, Production-Style Setup

Now let's bring everything together into one clean, well-organized example — this is the structure you'd actually use in a real project:

```js
const express = require("express");
const multer = require("multer");
const path = require("path");
const fs = require("fs");

const app = express();
app.use(express.json());

// ─── Ensure Upload Directory Exists ─────────────────────────────────────────
// If the "uploads/" folder doesn't exist when the server starts, create it
// This prevents Multer from throwing an error on the first upload
const uploadDir = "uploads/";
if (!fs.existsSync(uploadDir)) {
  fs.mkdirSync(uploadDir, { recursive: true });
}

// ─── Storage Configuration ───────────────────────────────────────────────────
const storage = multer.diskStorage({
  destination: (req, file, cb) => cb(null, uploadDir),

  filename: (req, file, cb) => {
    const ext = path.extname(file.originalname).toLowerCase();
    // Clean up the base name: replace spaces with hyphens, remove special chars
    const baseName = path.basename(file.originalname, ext)
      .replace(/\s+/g, "-")
      .replace(/[^a-zA-Z0-9-]/g, "");
    cb(null, `${baseName}-${Date.now()}${ext}`);
  }
});

// ─── File Filter ─────────────────────────────────────────────────────────────
const imageFilter = (req, file, cb) => {
  const allowed = ["image/jpeg", "image/png", "image/webp", "image/gif"];
  if (allowed.includes(file.mimetype)) {
    cb(null, true);
  } else {
    cb(new Error("Invalid file type. Only JPEG, PNG, WebP, and GIF are allowed."), false);
  }
};

// ─── Multer Instance ─────────────────────────────────────────────────────────
const upload = multer({
  storage,
  fileFilter: imageFilter,
  limits: { fileSize: 5 * 1024 * 1024 }, // 5 MB
});

// ─── Routes ──────────────────────────────────────────────────────────────────

// Single file upload (e.g., a profile picture)
app.post("/upload/profile", upload.single("avatar"), (req, res) => {
  if (!req.file) {
    return res.status(400).json({ error: "No file provided." });
  }
  res.json({
    message: "Profile picture uploaded!",
    filename: req.file.filename,
    size: `${(req.file.size / 1024).toFixed(2)} KB`,
    url: `/uploads/${req.file.filename}`, // URL to access the file in the browser
  });
});

// Multiple files from one input (e.g., a photo gallery)
app.post("/upload/gallery", upload.array("images", 6), (req, res) => {
  if (!req.files || req.files.length === 0) {
    return res.status(400).json({ error: "No files provided." });
  }
  const files = req.files.map(f => ({
    name: f.filename,
    size: `${(f.size / 1024).toFixed(2)} KB`,
    url: `/uploads/${f.filename}`,
  }));
  res.json({ message: `${files.length} image(s) uploaded!`, files });
});

// Multiple fields (e.g., a job application form)
app.post("/upload/application",
  upload.fields([
    { name: "photo", maxCount: 1 },
    { name: "resume", maxCount: 1 },
  ]),
  (req, res) => {
    const photo = req.files["photo"]?.[0];
    const resume = req.files["resume"]?.[0];

    res.json({
      message: "Application received!",
      applicantName: req.body.name,
      photo: photo?.filename || "Not provided",
      resume: resume?.filename || "Not provided",
    });
  }
);

// ─── Serving Uploaded Files ──────────────────────────────────────────────────
// This makes files in "uploads/" accessible via GET /uploads/<filename>
// e.g., GET /uploads/avatar-1717000000000.jpg
app.use("/uploads", express.static("uploads"));

// ─── Error Handler (must always be last) ─────────────────────────────────────
app.use((err, req, res, next) => {
  if (err instanceof multer.MulterError) {
    const messages = {
      LIMIT_FILE_SIZE:      "File is too large. Maximum size is 5MB.",
      LIMIT_FILE_COUNT:     "Too many files. Maximum is 6.",
      LIMIT_UNEXPECTED_FILE:"Unexpected field name in the upload.",
    };
    return res.status(400).json({ error: messages[err.code] || err.message });
  }
  if (err) {
    return res.status(400).json({ error: err.message });
  }
  next();
});

app.listen(3000, () => console.log("Server running on port 3000"));
```

---

## 14. Serving Uploaded Files to the Browser

Once a file is saved to disk, users need a way to view it. The `express.static()` middleware serves any file in a given directory as a static asset via a URL:

```js
app.use("/uploads", express.static("uploads"));
```

After this line, a file saved as `uploads/avatar-1717000000000.jpg` becomes accessible at `http://localhost:3000/uploads/avatar-1717000000000.jpg`. You typically return this URL in your API response so the frontend can display the image with a simple `<img src="..." />` tag.

---

## 15. Deleting Files After Processing

Sometimes you need to delete a file after you're done with it — for example, after uploading it to cloud storage. Node's built-in `fs` module handles this cleanly:

```js
const fs = require("fs");

app.post("/upload-and-process", upload.single("file"), async (req, res, next) => {
  try {
    const filePath = req.file.path; // e.g., "uploads/file-1717000000000.jpg"

    // ... process the file (resize, upload to S3, etc.) ...

    // Delete the local copy once we're done with it
    fs.unlink(filePath, (err) => {
      if (err) console.error("Could not delete temp file:", err);
    });

    res.json({ message: "File processed and cleaned up!" });
  } catch (error) {
    next(error);
  }
});
```

---

## 16. The `req.file` Object — Complete Reference

When using `upload.single()`, this is the full shape of `req.file`. Knowing what's available helps you decide what to save to your database:

```js
req.file = {
  fieldname:    "avatar",                       // the HTML input's name attribute
  originalname: "myPhoto.jpg",                  // original filename from the user's device
  encoding:     "7bit",                         // file transfer encoding
  mimetype:     "image/jpeg",                   // MIME type — useful for validation
  destination:  "uploads/",                     // the folder where it was saved (disk only)
  filename:     "avatar-1717000000000.jpg",      // the name Multer assigned on disk (disk only)
  path:         "uploads/avatar-1717000000000.jpg", // full relative path (disk only)
  size:         245678,                         // size in bytes

  // For memoryStorage, you get this instead of the above disk-specific fields:
  // buffer: <Buffer ff d8 ff e0 ...>           // raw file bytes (memory only)
}
```

In your database, you typically save just the `filename` or the constructed URL — not the entire object.

---

## 17. Summary Table

|Method|Use case|Where to find files|
|---|---|---|
|`upload.single("field")`|One file from one input|`req.file` — a single object|
|`upload.array("field", n)`|Multiple files from one multi-input|`req.files` — a flat array|
|`upload.fields([...])`|Files from multiple different inputs|`req.files` — object keyed by field name|
|`upload.none()`|Multipart form with only text fields|`req.body` only|

---

## 18. Quick Mental Checklist Before You Test

When setting up file uploads, run through this checklist to avoid the most common bugs:

Does my HTML form have `enctype="multipart/form-data"`? Without it, the browser sends the file as a URL-encoded string and Multer receives nothing. Does the string I pass to `upload.single("name")` exactly match the `name` attribute on the file input in my HTML? A single character difference means `req.file` will be `undefined`. Have I created the destination folder before starting the server? Multer will throw an error if it doesn't exist. Have I added a `fileFilter` to block dangerous file types? Have I set `limits.fileSize` to prevent oversized uploads? And finally, is my Multer error handler registered after all routes?

---

> ✅ **You now have a complete, deep understanding of file uploads with Multer. The natural next topics to explore are connecting file uploads to a database (saving the filename to a MongoDB document alongside other user data) and cloud storage integration — using `memoryStorage()` to upload files directly to Cloudinary or AWS S3 without ever saving them to your server's disk. Both patterns are extremely common in real-world Express applications.**