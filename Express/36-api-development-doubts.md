
---
# Problem-01 :
#### Problem : if student not found and user uploaded a new image then that image will be stored in uploads folder without any reference in database and it will consume storage unnecessarily.

### Solution :
if student not found then delete the uploaded image immediately before sending response to client

```js
if (!student) {
  if (req.file) {
    // delete the uploaded image if student not found
    try {
      await fs.unlink(req.file.path);
    } catch (error) {
      console.log("Uploaded image not found");
    }
  }

  return res.status(404).json({ message: "Student not found" });
}
```


---


# Problem-02 :

When updating a student’s profile image, the system deletes the old image and then updates the database separately.

Since these two actions are not connected, if the database update fails after deleting the old image, the system becomes inconsistent — the database may still point to a file that no longer exists, or an uploaded new file may remain unused on the server.

This happens because file operations and database operations are independent and cannot automatically undo each other.



```js
// update a student
router.put("/:id", upload.single("profile_pic"), async (req, res) => {
  try {
    const student = await Student.findById(req.params.id);
    if (!student) {
      // Problem : if student not found and user uploaded a new image then that image will be stored in uploads folder without any reference in database and it will consume storage unnecessarily.
      // Solution : if student not found then delete the uploaded image immediately before sending response to client
      if (req.file) {
        // delete the uploaded image if student not found
        try {
          await fs.unlink(req.file.path);
        } catch (error) {
          console.log("Uploaded image not found");
        }
      }
      return res.status(404).json({ message: "Student not found" });
    }

    const updatedData = {
      ...req.body,
    };

    // If new image uploaded
    if (req.file) {
      // delete old image
      if (student.profile_pic) {
        try {
          await fs.unlink(student.profile_pic);
        } catch (error) {
          console.log("Old image not found");
        }
      }
      // update with new image path
      updatedData.profile_pic = req.file.path;
    }

    const updatedStudent = await Student.findByIdAndUpdate(
      req.params.id,
      updatedData,
      {
        new: true, // Return the updated document instead of the original one
        runValidators: true, // Apply schema validation rules to the update operation
      },
    );

    res.json(updatedStudent);
  } catch (error) {
    // Handle invalid ObjectId error : when user send invalid mongodb _id in the request params
    if (error.name === "CastError") {
      return res.status(400).json({
        message: "Invalid student ID",
      });
    }

    res.status(500).json({ message: error.message });
  }
});
```


Let's walk through your code step by step.

### Initial state

Database:

```json
{
  "_id": "123",
  "profile_pic": "uploads/old.jpg"
}
```

Filesystem:

```txt
uploads/old.jpg
```

---

### User uploads a new image

Multer runs **before** your route handler and saves the file:

```txt
uploads/old.jpg
uploads/new.jpg
```

Now:

```js
req.file.path === "uploads/new.jpg"
```

---

### Your code deletes the old image

```js
if (student.profile_pic) {
  await fs.unlink(student.profile_pic);
}
```

Filesystem becomes:

```txt
uploads/new.jpg
```

`old.jpg` is gone.

---

### Then database update runs

```js
await Student.findByIdAndUpdate(
  req.params.id,
  updatedData,
  {
    new: true,
    runValidators: true,
  }
);
```

Suppose this throws an error.

Examples:

```js
updatedData.age = -5; // validator fails
```

or

```js
updatedData.email = "invalid-email";
```

or MongoDB connection temporarily fails.

The update never reaches the database successfully.

---

### What is stored now?

Database was **not updated**, so it still contains:

```json
{
  "_id": "123",
  "profile_pic": "uploads/old.jpg"
}
```

But filesystem contains:

```txt
uploads/new.jpg
```

because you already deleted `old.jpg`.

So:

|Item|Status|
|---|---|
|Database reference|`uploads/old.jpg`|
|`old.jpg` file|Deleted|
|`new.jpg` file|Exists|
|Database points to new image|No|

That's why the inconsistency occurs.

---

### The root cause

These two operations are independent:

```js
await fs.unlink(student.profile_pic);      // filesystem
await Student.findByIdAndUpdate(...);      // database
```

Node.js cannot automatically "undo" the file deletion if the database update fails afterward.

There is no transaction connecting:

- MongoDB
    
- Local file system
    

So if step 1 succeeds and step 2 fails, you are left in a partially completed state.

That's why deleting the old image **after** a successful database update is usually safer. If the update fails, at least the database and old file remain consistent.



# Solution: Update Student with Profile Image (Safe File Handling)

## Goal

While updating a student:

- If a new image is uploaded, update the database with the new image path.
    
- Delete the old image only after the database update succeeds.
    
- If the database update fails, delete the newly uploaded image.
    
- If student is not found, delete the uploaded image immediately.
    

This prevents:

- Orphan files
    
- Broken image references
    
- Database/File system inconsistency
    

---

```js
// update a student
router.put("/:id", upload.single("profile_pic"), async (req, res) => {
  try {
    const student = await Student.findById(req.params.id);
	
    if (!student) {
      // Problem :
      // Multer uploads image before this route handler executes.
      // If student does not exist, uploaded image will remain in uploads folder
      // without any database reference.
	
      // Solution :
      // Delete uploaded image immediately before returning response.
	
      if (req.file) {
        try {
          await fs.unlink(req.file.path);
        } catch (error) {
          console.log("Uploaded image not found");
        }
      }
	
      return res.status(404).json({
        message: "Student not found",
      });
    }

    const updatedData = {
      ...req.body,
    };

    // Store old image path
    // We will delete it only after successful database update.
    const oldProfilePic = student.profile_pic;

    // If user uploaded a new image
    if (req.file) {
      // Add new image path to update object
      updatedData.profile_pic = req.file.path;
    }

    let updatedStudent;

    try {
      updatedStudent = await Student.findByIdAndUpdate(
        req.params.id,
        updatedData,
        {
          new: true, // Return updated document
          runValidators: true, // Apply schema validation rules
        },
      );
    } catch (error) {

      // Problem :
      // New image was uploaded successfully
      // but database update failed.

      // Example :
      // - Validation error
      // - MongoDB connection issue
      // - Server error

      // Result :
      // Uploaded image has no database reference.

      // Solution :
      // Delete newly uploaded image.

      if (req.file) {
        try {
          await fs.unlink(req.file.path);
        } catch (err) {
          console.log("Uploaded image not found");
        }
      }

      throw error;
    }

    // Database update succeeded

    // Problem :
    // Old image is no longer needed.

    // Solution :
    // Delete old image after successful database update.

    if (req.file && oldProfilePic) {
      try {
        await fs.unlink(oldProfilePic);
      } catch (error) {
        console.log("Old image not found");
      }
    }

    res.json(updatedStudent);

  } catch (error) {

    // Handle invalid MongoDB ObjectId

    if (error.name === "CastError") {
      return res.status(400).json({
        message: "Invalid student ID",
      });
    }

    res.status(500).json({
      message: error.message,
    });
  }
});
```

---

# Why This Order Is Important

## Unsafe Approach

```js
await fs.unlink(oldProfilePic);

await Student.findByIdAndUpdate(...);
```

### Problem

If update fails:

Database:

```json
{
  "profile_pic": "old.jpg"
}
```

Filesystem:

```txt
new.jpg
```

Issues:

- old.jpg deleted
    
- database still points to old.jpg
    
- new.jpg exists without database reference
    

System becomes inconsistent.

---

# Safe Approach

```js
1. Upload new image
2. Update database
3. Delete old image
```

If update succeeds:

Database:

```json
{
  "profile_pic": "new.jpg"
}
```

Filesystem:

```txt
new.jpg
```

Everything is correct.

---

# If Database Update Fails

```js
1. Upload new image
2. Update database fails
3. Delete uploaded image
```

Database:

```json
{
  "profile_pic": "old.jpg"
}
```

Filesystem:

```txt
old.jpg
```

Everything remains correct.

---

# Golden Rule

Whenever replacing a file:

❌ Never delete the old file before the database update succeeds.

✅ Always:

1. Upload new file.
    
2. Update database.
    
3. If update succeeds → delete old file.
    
4. If update fails → delete new file.
    

This keeps the database and filesystem synchronized.

---


Here are **complete, structured, and detailed notes** for your entire `update student` API. You can save this as a reference for interviews, revision, or backend understanding.

---

# 📘 UPDATE STUDENT API — COMPLETE NOTES (Multer + MongoDB + File Handling)

---

# 🎯 Goal of this API

This API updates a student record and optionally updates their profile image.

It must ensure:

- ✔ Database stays correct
    
- ✔ Filesystem stays clean
    
- ✔ No orphan images exist
    
- ✔ No broken image references exist
    

---

# ⚙️ TECHNOLOGIES INVOLVED

- Express.js → API handling
    
- MongoDB (Mongoose) → data storage
    
- Multer → file upload handling
    
- fs/promises → file deletion
    
- Node.js filesystem → image storage
    

---

# ⚠️ CORE PROBLEM IN THIS SYSTEM

There are **3 independent systems**:

|System|Role|
|---|---|
|Multer|uploads file first|
|MongoDB|stores image path|
|File system|stores actual image|

👉 They do NOT share transactions

---

# 💥 MAIN PROBLEMS

## ❌ 1. Orphan File Problem

When:

- Student does NOT exist
    
- But image is already uploaded
    

👉 File exists but database has no record

### Result:

- Wasted storage
    
- Untracked files
    

---

## ❌ 2. Partial Failure Problem (Most Important)

When:

1. New image uploaded ✔
    
2. Old image deleted ❌
    
3. Database update fails ❌
    

### Result:

- Old image is gone
    
- DB still points to old image
    
- New image is unused
    

👉 SYSTEM INCONSISTENCY

---

## ❌ 3. Order Dependency Problem

If we delete files BEFORE DB update:

- Any DB failure breaks system integrity
    

---

# 🧠 MAIN SOLUTION STRATEGY

We follow this rule:

> “Database is the source of truth. File operations depend on DB success.”

---

# 🚀 STEP-BY-STEP FLOW

---

# 🟢 STEP 1: Find student

```js
const student = await Student.findById(req.params.id);
```

### Purpose:

Check if student exists.

---

## ❌ If student NOT found

### Problem:

- Multer already uploaded file
    
- No DB record exists
    

### Solution:

Delete uploaded file immediately

```js
await fs.unlink(req.file.path);
```

### Then stop execution:

```js
return res.status(404)
```

---

# 🟡 STEP 2: Prepare update data

```js
const updatedData = { ...req.body };
```

### Meaning:

Collect normal fields like:

- name
    
- age
    
- email
    

---

# 🟡 STEP 3: Store old image

```js
const oldProfilePic = student.profile_pic;
```

### Why?

We may need to delete it later.

BUT ONLY AFTER SUCCESSFUL UPDATE.

---

# 🟡 STEP 4: Attach new image (if exists)

```js
if (req.file) {
  updatedData.profile_pic = req.file.path;
}
```

### Meaning:

Replace old image with new one in update request.

---

# 🔵 STEP 5: Update database (MOST IMPORTANT)

```js
updatedStudent = await Student.findByIdAndUpdate(...)
```

### This is the CORE operation

---

# ❌ CASE A: DATABASE UPDATE FAILS

### Possible reasons:

- validation error
    
- invalid data
    
- MongoDB error
    
- server crash
    

---

### Problem:

- New image already uploaded
    
- But DB did not save it
    

👉 Image becomes useless

---

### Solution:

Delete newly uploaded image

```js
await fs.unlink(req.file.path);
```

### Then:

```js
throw error;
```

👉 Pass error to outer handler

---

# 🟢 CASE B: DATABASE UPDATE SUCCESS

Now DB is correct.

---

### Problem:

Old image is no longer needed.

---

### Solution:

Delete old image safely

```js
await fs.unlink(oldProfilePic);
```

---

# 🔴 STEP 6: Send response

```js
res.json(updatedStudent);
```

---

# ⚠️ OUTER CATCH BLOCK

Handles:

- invalid ObjectId
    
- unexpected server errors
    

---

### Example:

```js
if (error.name === "CastError")
```

👉 invalid MongoDB ID

---

# 🧠 WHY "RETHROW ERROR" IS USED

Inside inner catch:

```js
throw error;
```

### Purpose:

- inner catch → cleanup only
    
- outer catch → send response
    

---

### Without rethrow:

- outer catch never runs ❌
    
- request may hang ❌
    

---

# 🔥 FINAL EXECUTION FLOW

## CASE 1: Student not found

```
upload file
→ delete file
→ return 404
```

---

## CASE 2: DB update fails

```
upload file
→ try DB update
→ fail
→ delete new file
→ throw error
→ outer catch sends response
```

---

## CASE 3: DB update success

```
upload file
→ update DB
→ delete old file
→ send response
```

---

# 📌 GOLDEN RULES

## 🧠 Rule 1

Never trust filesystem before DB success

---

## 🧠 Rule 2

Database is the source of truth

---

## 🧠 Rule 3

Always clean up:

- orphan uploads
    
- unused images
    

---

## 🧠 Rule 4

Delete files ONLY when you are sure DB update succeeded

---

# 🧾 FINAL SUMMARY (INTERVIEW READY)

This API ensures consistency between MongoDB and filesystem by carefully controlling the lifecycle of uploaded images. It prevents orphan files and broken references by deleting uploaded images when database operations fail and removing old images only after successful updates.

---


# Problem-03 :

Even after applying this  below code image get uploaded to server folder on put request for an invalid id 