
---


## Doubt Regarding Updating Profile Image

Suppose I have a simple **Student API** with the following schema:

- `name`
- `rollNo`
- `profile`

When a user sends a **PATCH** request to update the student, I first fetch the existing student by ID:

```js
const existingStudent = await Student.findById(req.params.id);
```

Then, I prepare the update object and update the student document:

```js
const updatedData = {};

// Update only the fields provided in the request
if (name !== undefined) updatedData.name = name;
if (newRollNo !== undefined) updatedData.rollNo = newRollNo;
if (req.file) updatedData.profile = req.file.path;

const updatedStudent = await Student.findByIdAndUpdate(
  req.params.id,
  updatedData,
  {
    new: true,
    runValidators: true,
  }
);
```

After the database update is successful, I delete the old profile image for cleanup:

```js
if (req.file && existingStudent.profile) {
  await deleteFile(existingStudent.profile);
}
```

## My Doubt

Since the student document has already been updated, won't `existingStudent.profile` now point to the **new profile image** instead of the old one?

Or does `existingStudent` still hold the old document data that was fetched before `findByIdAndUpdate()` was executed?