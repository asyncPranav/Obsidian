
---
## Doubt regarding order of middleware in route

The order is important because **middleware runs from top to bottom in the exact order you write it in the route**.

Express does not automatically understand that "validation should happen after file parsing". You decide the flow.

---

Suppose your route was like this (your wrong order):

```js
router.post(
  "/",
  createStudentValidator,
  upload.single("profile"),
  createStudent
);
```

The request flow becomes:

```
Request
   |
   ↓
createStudentValidator
   |
   ↓
upload.single("profile")
   |
   ↓
createStudent controller
```

Now the problem:

Your request is:

```
Content-Type: multipart/form-data
```

because you are sending:

```
name     Aryan
rollNo   101
profile  image.png
```

But Express itself does not parse `multipart/form-data`.

Only Multer does that.

Before Multer runs:

```js
req.body
```

is:

```js
{}
```

and:

```js
req.file
```

is:

```js
undefined
```

So your validator checks:

```js
body("name")
.notEmpty()
```

against:

```js
req.body.name
```

which is:

```js
undefined
```

Therefore it says:

```json
{
  "msg": "name is required"
}
```

---

Now the correct order:

```js
router.post(
  "/",
  upload.single("profile"),
  createStudentValidator,
  createStudent
);
```

Flow:

```
Request
   |
   ↓
Multer
   |
   ↓
Parse multipart/form-data
   |
   ↓
req.body created
req.file created
   |
   ↓
Validator
   |
   ↓
Controller
```

After Multer:

```js
req.body
```

becomes:

```js
{
  name: "Aryan",
  rollNo: "101"
}
```

and:

```js
req.file
```

becomes:

```js
{
  fieldname: "profile",
  path: "uploads/image.png"
}
```

Now validator can correctly check:

```js
name exists ✅
rollNo exists ✅
```

---

Think of it like a pipeline:

Wrong:

```
Check passport
     |
     ↓
Create passport
```

You cannot validate something before it exists.

Correct:

```
Create passport
     |
     ↓
Check passport
```

---

For APIs, a common order is:

```js
router.post(
  "/",
  upload.single("profile"),  // Parse files + body
  createStudentValidator,   // Validate parsed data
  createStudent             // Business logic
);
```

Because:

1. **Multer** → extracts data from the request
    
2. **Validator** → checks whether extracted data is correct
    
3. **Controller** → saves/processes the data
    

Your order was wrong because you were trying to validate data **before the middleware responsible for creating that data had executed**.

---

# ✅ Doubt Regarding Updating Profile Image in Controller

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

Nahi. **`existingStudent.profile` new profile ko point nahi karega.** Ye Mongoose ki bahut important behavior hai.

Jab tum ye likhte ho:

```js
const existingStudent = await Student.findById(req.params.id);
```

to `existingStudent` us waqt database ka **snapshot** hai.

Example:

Database:

```js
{
  _id: "123",
  name: "Rahul",
  profile: "uploads/old.jpg"
}
```

Ab:

```js
const existingStudent = await Student.findById(req.params.id);
```

Memory me:

```js
existingStudent.profile
// "uploads/old.jpg"
```

Ab tum update karte ho:

```js
const updatedStudent = await Student.findByIdAndUpdate(
  req.params.id,
  {
    profile: "uploads/new.jpg"
  },
  { new: true }
);
```

Ab kya hoga?

```js
existingStudent.profile
// "uploads/old.jpg"

updatedStudent.profile
// "uploads/new.jpg"
```

`findByIdAndUpdate()` **database me document update karta hai**, lekin `existingStudent` object ko mutate/update nahi karta.

Isliye ye bilkul safe hai:

```js
await Student.findByIdAndUpdate(...);

// existingStudent ke paas abhi bhi old path hai
await deleteFile(existingStudent.profile);
```

### Visualization

```text
Database
──────────────
profile = old.jpg
      │
      ▼
findById()
      │
      ▼
existingStudent.profile = old.jpg   (memory copy)

      │
      ▼
findByIdAndUpdate(profile = new.jpg)

Database
──────────────
profile = new.jpg

Memory
──────────────
existingStudent.profile = old.jpg   ✅ unchanged

updatedStudent.profile = new.jpg
```

Agar tum manually ye karte:

```js
existingStudent.profile = req.file.path;
await existingStudent.save();
```

tab:

```js
existingStudent.profile
// "uploads/new.jpg"
```

kyunki tumne **usi object** ko modify kiya hai.

Lekin `findByIdAndUpdate()` ek **alag query** chalata hai. Ye pehle se loaded `existingStudent` object ko update nahi karta.

**Isliye production me commonly ye pattern use hota hai:**

```js
const existingStudent = await Student.findById(id);

const updatedStudent = await Student.findByIdAndUpdate(
  id,
  updatedData,
  { new: true }
);

// existingStudent.profile is still the OLD image path
if (req.file && existingStudent.profile) {
  await deleteFile(existingStudent.profile);
}
```

To tumhara doubt valid tha, lekin is case me `existingStudent.profile` **old image path hi rahega**, isi wajah se uska use cleanup ke liye kiya jata hai.


---

# Detail Answer

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