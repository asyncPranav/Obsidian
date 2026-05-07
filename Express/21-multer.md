
---

# 🧠 COMPLETE MULTER NOTES (VERY DETAILED)

## File Uploading in Express.js using multer

These notes are beginner-friendly but also contain developer-level understanding.

---

# 📌 1. WHAT IS MULTER?

Multer is a **middleware** for Express.js used to:

✔ upload files  
✔ handle images  
✔ handle PDFs/videos/docs  
✔ process multipart/form-data

---

# 🧠 SIMPLE UNDERSTANDING

Normally Express can read:

```text
text data
JSON data
form fields
```

But Express CANNOT understand files directly.

Example:

```text
photo.png
resume.pdf
video.mp4
```

👉 Multer helps Express understand and process files.

---

# 🔥 REAL LIFE ANALOGY

Imagine:

```text
User uploads image
↓
Browser sends file
↓
Express gets confused
↓
Multer acts like translator
↓
File gets saved
```

---

# 📌 2. WHY MULTER IS NEEDED?

Without multer:

❌ `req.body` cannot process files  
❌ uploaded files inaccessible  
❌ form with image upload fails

---

# 📌 3. INSTALLATION

```bash
npm install multer
```

---

# 📌 4. IMPORTING MULTER

```js
const multer = require("multer");
```

---

# 📌 5. BASIC FILE UPLOAD FLOW (VERY IMPORTANT)

```text
Client selects file
↓
Browser sends multipart/form-data
↓
Multer middleware intercepts request
↓
Multer processes file
↓
File saved
↓
File info available in req.file
```

---

# 📌 6. IMPORTANT CONCEPT → multipart/form-data

When uploading files:

```html
<form enctype="multipart/form-data">
```

MUST be used.

---

# ❌ Without enctype

File upload WILL NOT work.

---

# 🧠 WHY?

Normal form:

```text
application/x-www-form-urlencoded
```

Used for:

- text
    
- numbers
    

BUT files require:

```text
multipart/form-data
```

---

# 📌 7. SIMPLEST MULTER EXAMPLE

---

## STEP 1 → IMPORT

```js
const express = require("express");
const multer = require("multer");

const app = express();
```

---

## STEP 2 → CREATE UPLOAD OBJECT

```js
const upload = multer({ dest: "uploads/" });
```

---

# 🧠 WHAT THIS DOES

```text
dest: "uploads/"
```

means:

👉 save uploaded files inside uploads folder

---

# 📌 8. CREATE FORM

```html
<form action="/upload" method="POST" enctype="multipart/form-data">

  <input type="file" name="image">

  <button type="submit">
    Upload
  </button>

</form>
```

---

# 🧠 IMPORTANT PARTS

---

## method="POST"

File uploads usually use POST request.

---

## enctype="multipart/form-data"

Allows file transfer.

---

## name="image"

VERY IMPORTANT ⚠️

This name must match multer field name.

---

# 📌 9. ROUTE HANDLING

```js
app.post("/upload", upload.single("image"), (req, res) => {

  console.log(req.file);

  res.send("File uploaded");

});
```

---

# 🧠 EXPLANATION

---

## upload.single("image")

means:

👉 upload ONLY ONE file  
👉 field name is `"image"`

---

# 🧠 VERY IMPORTANT

This:

```js
single("image")
```

MUST MATCH:

```html
<input name="image">
```

---

# ❌ WRONG

```html
<input name="photo">
```

BUT:

```js
upload.single("image")
```

❌ file won't work

---

# 📌 10. req.file

After upload:

```js
console.log(req.file);
```

Contains uploaded file info.

---

# 📌 EXAMPLE OUTPUT

```js
{
  fieldname: 'image',
  originalname: 'cat.png',
  encoding: '7bit',
  mimetype: 'image/png',
  destination: 'uploads/',
  filename: 'abc123.png',
  path: 'uploads/abc123.png',
  size: 12345
}
```

---

# 🧠 IMPORTANT FIELDS

|Field|Meaning|
|---|---|
|originalname|original file name|
|mimetype|file type|
|size|file size|
|path|saved location|
|filename|stored filename|

---

# 📌 11. req.body + req.file

If form has both text and file:

```html
<input type="text" name="title">
<input type="file" name="image">
```

Then:

```js
req.body.title
req.file
```

---

# 📌 12. STORAGE ENGINE (VERY IMPORTANT)

Professional apps use:

```js
multer.diskStorage()
```

instead of simple `dest`.

---

# 📌 WHY?

Because:

- custom filenames
    
- custom folders
    
- better control
    

---

# 📌 13. DISK STORAGE SYNTAX

```js
const storage = multer.diskStorage({

  destination: function(req, file, cb) {
    cb(null, "uploads/");
  },

  filename: function(req, file, cb) {
    cb(null, Date.now() + "-" + file.originalname);
  }

});
```

---

# 🧠 EXPLANATION

---

# 🔹 destination()

Controls:

```text
where file should be stored
```

---

## cb(null, "uploads/")

means:

```text
No error
Store in uploads folder
```

---

# 🔹 filename()

Controls:

```text
how filename should look
```

---

# 📌 Example

```js
171234-photo.png
```

---

# 🧠 WHY CUSTOM FILENAME IMPORTANT?

Without custom naming:

Two users upload:

```text
photo.png
```

Old file may get overwritten.

---

# 📌 14. CREATE MULTER INSTANCE

```js
const upload = multer({
  storage: storage
});
```

---

# 📌 15. COMPLETE PROFESSIONAL SETUP

```js
const multer = require("multer");

const storage = multer.diskStorage({

  destination: function(req, file, cb) {
    cb(null, "uploads/");
  },

  filename: function(req, file, cb) {
    cb(null, Date.now() + "-" + file.originalname);
  }

});

const upload = multer({
  storage: storage
});
```

---

# 📌 16. upload.single()

Used for ONE file.

```js
upload.single("image")
```

---

# 📌 17. upload.array()

Used for MULTIPLE files.

```js
upload.array("images", 5)
```

---

# 🧠 MEANING

```text
field name = images
max files = 5
```

---

# 📌 18. upload.fields()

Used for multiple different fields.

---

## Example

```js
upload.fields([
  { name: "avatar", maxCount: 1 },
  { name: "gallery", maxCount: 5 }
])
```

---

# 📌 19. FILE FILTERING (VERY IMPORTANT)

Used to allow only images.

---

# Example

```js
const upload = multer({

  storage: storage,

  fileFilter: function(req, file, cb) {

    if(file.mimetype.startsWith("image")) {
      cb(null, true);
    } else {
      cb(new Error("Only images allowed"));
    }

  }

});
```

---

# 🧠 WHY IMPORTANT?

Security.

Without filtering:  
❌ anyone uploads dangerous files

---

# 📌 20. LIMIT FILE SIZE

```js
const upload = multer({

  storage: storage,

  limits: {
    fileSize: 1024 * 1024 * 2
  }

});
```

---

# 🧠 MEANING

```text
2MB max size
```

---

# 📌 21. SERVING UPLOADED FILES

If images stored in uploads folder:

```js
app.use("/uploads", express.static("uploads"));
```

---

# 🧠 WHY?

Allows browser access:

```text
localhost:3000/uploads/photo.png
```

---

# 📌 22. SHOW IMAGE IN EJS

```html
<img src="/uploads/<%= user.image %>">
```

---

# 📌 23. ERROR HANDLING WITH MULTER

---

## Example

```js
app.post("/upload", (req, res) => {

  upload.single("image")(req, res, function(err) {

    if(err) {
      return res.send(err.message);
    }

    res.send("Uploaded");

  });

});
```

---

# 📌 24. COMMON BEGINNER MISTAKES

---

## ❌ Forgetting enctype

Most common mistake.

---

## ❌ uploads folder missing

Create folder manually.

---

## ❌ Field names mismatch

```html
name="photo"
```

BUT:

```js
single("image")
```

❌ won't work

---

## ❌ Using GET request

Uploads use POST.

---

## ❌ Not serving static folder

Then images won't display.

---

# 📌 25. REAL PROJECT USES

Multer used in:

✔ profile image upload  
✔ blog image upload  
✔ resume upload  
✔ product image upload  
✔ gallery apps  
✔ social media apps

---

# 📌 26. MULTER + MVC STRUCTURE

---

# Model

Stores filename in DB.

---

# Controller

Handles upload logic.

---

# Route

Uses multer middleware.

---

# View

Contains upload form.

---

# 📌 27. COMPLETE FLOW (VERY IMPORTANT)

```text
User selects file
↓
Browser sends multipart/form-data
↓
Multer middleware runs
↓
File saved in uploads folder
↓
File info added to req.file
↓
Controller stores filename in DB
↓
Image shown in frontend
```

---

# 📌 28. req.file vs req.files

|Property|Used For|
|---|---|
|req.file|single file|
|req.files|multiple files|

---

# 📌 29. IMPORTANT INTERVIEW QUESTIONS

---

## Q1. Why multer needed?

Because Express cannot handle multipart/form-data by default.

---

## Q2. Difference between single() and array()

|single()|array()|
|---|---|
|one file|multiple files|

---

## Q3. Why enctype important?

Allows file transfer in forms.

---

## Q4. Why use diskStorage()?

For custom storage control.

---

# 🧠 FINAL SUMMARY

---

# Multer is:

✔ middleware  
✔ handles file uploads  
✔ processes multipart/form-data  
✔ stores files  
✔ gives file info

---

# MOST IMPORTANT THINGS TO REMEMBER

✔ enctype="multipart/form-data"  
✔ upload.single("fieldname")  
✔ req.file  
✔ diskStorage()  
✔ fileFilter  
✔ fileSize limit  
✔ express.static() for uploads

---

