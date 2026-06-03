
---

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