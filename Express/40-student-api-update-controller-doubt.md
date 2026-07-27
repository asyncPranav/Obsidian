
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


----

# Answer

Short answer: **No.** `existingStudent.profile` will **not** point to the new profile image. It will still contain the **old profile path** that was fetched by `findById()`.

---

## Why?

When you execute:

```js
const existingStudent = await Student.findById(req.params.id);
```

Mongoose performs a database query and returns a **Document instance** containing the data that existed **at that moment**.

For example, suppose your database contains:

```json
{
  "_id": "123",
  "name": "Rahul",
  "rollNo": 101,
  "profile": "uploads/old-profile.jpg"
}
```

After executing:

```js
const existingStudent = await Student.findById(req.params.id);
```

The `existingStudent` object stored in memory looks like:

```js
existingStudent = {
  _id: "123",
  name: "Rahul",
  rollNo: 101,
  profile: "uploads/old-profile.jpg"
}
```

At this point, `existingStudent` is simply an object in your application's memory.

---

## What happens when `findByIdAndUpdate()` runs?

```js
const updatedStudent = await Student.findByIdAndUpdate(
  req.params.id,
  updatedData,
  {
    new: true,
    runValidators: true,
  }
);
```

This query directly updates the document **inside MongoDB**.

Suppose the new uploaded image is:

```
uploads/new-profile.jpg
```

Now the database becomes:

```json
{
  "_id": "123",
  "name": "Rahul",
  "rollNo": 101,
  "profile": "uploads/new-profile.jpg"
}
```

Since you used:

```js
{ new: true }
```

`updatedStudent` contains the updated document:

```js
updatedStudent.profile
// "uploads/new-profile.jpg"
```

However,

```js
existingStudent.profile
```

still contains:

```js
"uploads/old-profile.jpg"
```

because **`findByIdAndUpdate()` does not mutate previously fetched documents.**

---

## Memory vs Database

Think of it like taking a photograph.

```
Database
-------------------------
profile = old-profile.jpg
        │
        │ findById()
        ▼
existingStudent (Snapshot)
profile = old-profile.jpg
```

Later:

```
findByIdAndUpdate()
        │
        ▼
Database
-------------------------
profile = new-profile.jpg
```

But the snapshot you already have in memory remains unchanged:

```
existingStudent.profile
↓
old-profile.jpg
```

A database update does **not** automatically update every JavaScript object that was previously created from that data.

---

## Why is this useful?

Because after a successful database update, you still know which image was previously being used.

```js
if (req.file && existingStudent.profile) {
    await deleteFile(existingStudent.profile);
}
```

Here:

- Database now points to the **new image**
- `existingStudent.profile` still points to the **old image**

So you can safely delete the old file.

---

## What if `existingStudent.profile` also changed automatically?

Imagine this happened:

```
Database updated
        │
        ▼
existingStudent.profile
        │
        ▼
new-profile.jpg
```

Then this code:

```js
await deleteFile(existingStudent.profile);
```

would delete the **newly uploaded image**, leaving the database pointing to a file that no longer exists.

That would break your application.

Fortunately, Mongoose does **not** work like this.

---

## When would `existingStudent.profile` actually change?

Only if **you modify the same document instance yourself.**

Example:

```js
const existingStudent = await Student.findById(id);

existingStudent.profile = req.file.path;

await existingStudent.save();
```

Now:

```js
existingStudent.profile
// uploads/new-profile.jpg
```

because you directly modified the document instance.

---

## Difference between the two approaches

### 1. Using `findByIdAndUpdate()`

```js
const existingStudent = await Student.findById(id);

await Student.findByIdAndUpdate(id, {
    profile: "new.jpg"
});

console.log(existingStudent.profile);
```

Output:

```text
old.jpg
```

`existingStudent` remains unchanged.

---

### 2. Using `.save()`

```js
const existingStudent = await Student.findById(id);

existingStudent.profile = "new.jpg";

await existingStudent.save();

console.log(existingStudent.profile);
```

Output:

```text
new.jpg
```

Because you modified the same document instance.

---

# Conclusion

`findByIdAndUpdate()` updates the document **only in MongoDB** and returns a **new updated document** (when `{ new: true }` is used). It **does not modify** the `existingStudent` object that was fetched earlier.

Therefore:

- `existingStudent.profile` → **Old image path**
- `updatedStudent.profile` → **New image path**

This is exactly why the following cleanup pattern is safe and commonly used in production:

```js
const existingStudent = await Student.findById(id);

const updatedStudent = await Student.findByIdAndUpdate(id, updatedData, {
    new: true,
    runValidators: true,
});

if (req.file && existingStudent.profile) {
    await deleteFile(existingStudent.profile);
}
```

The database now references the **new image**, while `existingStudent.profile` still references the **old image**, allowing you to safely delete the old file without affecting the newly uploaded one.