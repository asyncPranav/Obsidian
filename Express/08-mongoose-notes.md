

---
Below is a **much more complete, interview + project-ready Mongoose guide** covering almost everything you’ll actually use in real backend development.

---

# 🧠 MONGOOSE COMPLETE NOTES (DETAILED)

---

# 🚀 1. Introduction to Mongoose

Mongoose is an **ODM (Object Data Modeling)** library for MongoDB and Node.js.

### What it does:

- Converts MongoDB documents → JavaScript objects
    
- Adds **schema structure** to MongoDB
    
- Provides **validation, middleware, relationships, hooks**
    

---

## 🆚 MongoDB vs Mongoose

|Feature|MongoDB (native)|Mongoose|
|---|---|---|
|Structure|Schema-less|Schema-based|
|Validation|Manual|Built-in|
|Code style|Raw queries|JS-friendly models|
|Middleware|No|Yes|
|Relations|Manual|Populate support|

---

# ⚙️ 2. Installation & Setup

```bash
npm install mongoose
```

---

## Import Mongoose

```js
import mongoose from "mongoose";
```

---

## Connect to MongoDB

### Local DB

```js
mongoose.connect("mongodb://127.0.0.1:27017/mydb");
```

---

### MongoDB Atlas

```js
mongoose.connect("mongodb+srv://user:pass@cluster.mongodb.net/mydb");
```

---

## Proper Production Connection

```js
mongoose.connect(process.env.MONGO_URI)
.then(() => console.log("DB Connected"))
.catch(err => console.log(err));
```

---

## Connection Events

```js
mongoose.connection.on("connected", () => {
  console.log("MongoDB connected");
});

mongoose.connection.on("error", (err) => {
  console.log("Connection error", err);
});
```

---

# 🧱 3. Schema (Core Concept)

A schema defines **structure + rules of documents**

---

## Basic Schema

```js
const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  age: Number
});
```

---

## Schema Types

```js
{
  name: String,
  age: Number,
  isActive: Boolean,
  createdAt: Date,
  tags: [String],
  meta: Object
}
```

---

# 🔥 4. Schema Options (VERY IMPORTANT)

```js
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    trim: true,
    minlength: 3,
    maxlength: 50
  }
});
```

---

## Common Validators

|Validator|Use|
|---|---|
|required|field must exist|
|minlength|minimum length|
|maxlength|max length|
|min|number min|
|max|number max|
|unique|no duplicates|
|default|default value|
|enum|allowed values|

---

## Example (Full Validation)

```js
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    trim: true
  },
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true
  },
  age: {
    type: Number,
    min: 18,
    max: 60
  },
  role: {
    type: String,
    enum: ["user", "admin"],
    default: "user"
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
});
```

---

# 📦 5. Model (Brain of Mongoose)

Model = interface to MongoDB collection

```js
const User = mongoose.model("User", userSchema);
```

---

# 🧪 6. CRUD OPERATIONS (CORE)

---

## ➕ CREATE

```js
await User.create({
  name: "Rahul",
  email: "rahul@gmail.com",
  age: 22
});
```

OR

```js
const user = new User(data);
await user.save();
```

---

## 📖 READ ALL

```js
const users = await User.find();
```

---

## 📖 FILTERED READ

```js
User.find({ age: { $gte: 18 } });
```

---

## 🔍 FIND ONE

```js
User.findOne({ email: "test@gmail.com" });
```

---

## 🔍 FIND BY ID

```js
User.findById("id_here");
```

---

## ✏️ UPDATE

### Update by ID

```js
User.findByIdAndUpdate(id, {
  name: "Updated"
}, { new: true });
```

---

### Update One

```js
User.updateOne(
  { name: "Rahul" },
  { $set: { age: 25 } }
);
```

---

## ❌ DELETE

```js
User.findByIdAndDelete(id);
```

OR

```js
User.deleteOne({ name: "Rahul" });
```

---

# 🔍 7. Query Helpers (VERY IMPORTANT)

## Sorting

```js
User.find().sort({ age: 1 }); // ascending
User.find().sort({ age: -1 }); // descending
```

---

## Limiting

```js
User.find().limit(5);
```

---

## Skipping

```js
User.find().skip(10);
```

---

## Selecting Fields

```js
User.find().select("name email");
```

---

# 🧠 8. Middleware (Hooks)

Middleware runs **before or after actions**

---

## Pre-save hook

```js
userSchema.pre("save", function(next) {
  console.log("Before saving user");
  next();
});
```

---

## Post-save hook

```js
userSchema.post("save", function(doc) {
  console.log("User saved:", doc);
});
```

---

## Use cases:

- Password hashing
    
- Logging
    
- Validation
    
- Data modification
    

---

# 🔐 9. Virtuals (Computed Fields)

Virtual fields are NOT stored in DB.

```js
userSchema.virtual("fullName").get(function() {
  return this.firstName + " " + this.lastName;
});
```

---

# 🔗 10. Populate (Relationships)

MongoDB doesn’t join → Mongoose solves this using populate.

---

## Example: User & Posts

```js
const postSchema = new mongoose.Schema({
  title: String,
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User"
  }
});
```

---

## Fetch with populate

```js
Post.find().populate("user");
```

---

## Nested Populate

```js
Post.find().populate({
  path: "user",
  select: "name email"
});
```

---

# 🧩 11. Indexing (Performance Boost)

```js
userSchema.index({ email: 1 });
```

### Why?

- Faster search
    
- Optimized queries
    

---

# 🧠 12. Aggregation (Advanced)

Used for analytics-like queries

```js
User.aggregate([
  { $match: { age: { $gte: 18 } } },
  { $group: { _id: "$age", count: { $sum: 1 } } }
]);
```

---

# 🧪 13. Transactions (Advanced MongoDB feature)

```js
const session = await mongoose.startSession();
session.startTransaction();

try {
  await User.create([{ name: "A" }], { session });
  await session.commitTransaction();
} catch (err) {
  await session.abortTransaction();
}
```

---

# 🧬 14. Schema Types Advanced

## Arrays

```js
tags: [String]
```

---

## Nested Objects

```js
address: {
  city: String,
  pin: Number
}
```

---

## Mixed type

```js
data: mongoose.Schema.Types.Mixed
```

---

# 🧾 15. Timestamps

Automatically adds createdAt & updatedAt

```js
const schema = new mongoose.Schema({}, {
  timestamps: true
});
```

---

# 🧰 16. Instance Methods

```js
userSchema.methods.sayHello = function() {
  return `Hello ${this.name}`;
};
```

Usage:

```js
user.sayHello();
```

---

# 🧠 17. Static Methods

```js
userSchema.statics.findByEmail = function(email) {
  return this.findOne({ email });
};
```

---

# 🔥 18. Lean Queries (Performance)

```js
User.find().lean();
```

Returns plain JS objects instead of Mongoose documents.

---

# ⚡ 19. Error Handling

## Duplicate key error

```js
try {
  await User.create(data);
} catch (err) {
  console.log(err.code); // 11000
}
```

---

# 📁 20. Best Project Structure

```
project/
│
├── models/
├── controllers/
├── routes/
├── middleware/
├── config/
├── utils/
└── server.js
```

---

# 🚀 21. Real-world Flow

1. Connect DB
    
2. Create schema
    
3. Create model
    
4. Build API routes
    
5. Add controllers
    
6. Add middleware
    
7. Connect frontend
    

---

# 💡 22. Best Practices

✔ Always use `.env` for DB URI  
✔ Use `try/catch` everywhere  
✔ Use validation in schema  
✔ Use indexing for large apps  
✔ Use populate carefully (performance cost)  
✔ Prefer lean() for read-heavy APIs

---

# 🔥 FINAL SUMMARY

Mongoose gives you:

- Structured data (Schema)
    
- Easy DB operations (Model)
    
- Validation system
    
- Middleware system
    
- Relationship handling
    
- Query optimization tools
    

---

