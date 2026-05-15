
----

# Folder Structure 

```
readnest/
│
├── app.js
├── package.json
├── .env
│
├── config/
│   └── db.js
│
├── controllers/
│   ├── authController.js
│   ├── bookController.js
│   ├── bookmarkController.js
│   └── reviewController.js
│
├── middlewares/
│   ├── authMiddleware.js
│   ├── guestMiddleware.js
│   ├── errorMiddleware.js
│   ├── uploadMiddleware.js
│   └── validateMiddleware.js
│
├── models/
│   ├── User.model.js
│   ├── Book.model.js
│   └── Review.model.js
│
├── routes/
│   ├── authRoutes.js
│   ├── bookRoutes.js
│   ├── bookmarkRoutes.js
│   └── reviewRoutes.js
│
├── views/
│   │
│   ├── partials/
│   │   ├── header.ejs
│   │   ├── navbar.ejs
│   │   ├── footer.ejs
│   │   └── messages.ejs
│   │
│   ├── auth/
│   │   ├── register.ejs
│   │   └── login.ejs
│   │
│   ├── books/
│   │   ├── index.ejs
│   │   ├── single.ejs
│   │   ├── create.ejs
│   │   ├── edit.ejs
│   │   └── search.ejs
│   │
│   ├── dashboard/
│   │   ├── index.ejs
│   │   ├── bookmarks.ejs
│   │   └── mybooks.ejs
│   │
│   ├── errors/
│   │   ├── 404.ejs
│   │   └── error.ejs
│   │
│   └── home.ejs
│
├── public/
│   │
│   ├── css/
│   │   └── style.css
│   │
│   ├── js/
│   │   └── main.js
│   │
│   ├── images/
│   │
│   ├── uploads/
│   │   ├── covers/
│   │   └── pdfs/
│   │
│   └── icons/
│
├── validators/
│   ├── authValidator.js
│   ├── bookValidator.js
│   └── reviewValidator.js
│
├── utils/
│   ├── pagination.js
│   ├── deleteFile.js
│   └── calculateRating.js
│
└── node_modules/
```



# User.model.js

```js
import mongoose from "mongoose";

const userSchema = new mongoose.Schema(
  {
    username: {
      type: String,
      required: [true, "Username is required"],
      trim: true,
      minlength: 3,
      maxlength: 30,
    },

    email: {
      type: String,
      required: [true, "Email is required"],
      unique: true,
      lowercase: true,
      trim: true,
    },

    password: {
      type: String,
      required: [true, "Password is required"],
      minlength: 6,
    },

    role: {
      type: String,
      enum: ["user", "admin"],
      default: "user",
    },

    bookmarks: [
      {
        type: mongoose.Schema.Types.ObjectId,
        ref: "Book",
      },
    ],
  },
  {
    timestamps: true,
  }
);

const User = mongoose.model("User", userSchema);

export default User;
```


This is a **Mongoose Schema** for a `User` collection in MongoDB.

---

# 1. Importing mongoose

```js
import mongoose from "mongoose";
```

`mongoose` helps Node.js talk to MongoDB.

It gives:

- schemas
    
- models
    
- validation
    
- relationships
    

Think of it like a layer between your code and MongoDB.

---

# 2. Creating the Schema

```js
const userSchema = new mongoose.Schema(
```

A **schema** defines:

> “What fields a User document should have.”

Like a blueprint/template.

---

# 3. Username Field

```js
username: {
  type: String,
  required: [true, "Username is required"],
  trim: true,
  minlength: 3,
  maxlength: 30,
},
```

This means:

|Option|Meaning|
|---|---|
|`type: String`|username must be text|
|`required`|field cannot be empty|
|`"Username is required"`|custom error message|
|`trim: true`|removes spaces before/after|
|`minlength: 3`|minimum 3 characters|
|`maxlength: 30`|maximum 30 characters|

---

### Example

Input:

```js
"   aman   "
```

Stored in DB:

```js
"aman"
```

because of:

```js
trim: true
```

---

# 4. Email Field

```js
email: {
  type: String,
  required: [true, "Email is required"],
  unique: true,
  lowercase: true,
  trim: true,
},
```

### New things here:

---

## `unique: true`

```js
unique: true
```

Means:

> No two users can have the same email.

So this is NOT allowed:

```js
user1@gmail.com
user1@gmail.com
```

MongoDB creates a unique index internally.

---

## `lowercase: true`

```js
lowercase: true
```

Automatically converts:

```js
AMAN@GMAIL.COM
```

to:

```js
aman@gmail.com
```

Very useful for login systems.

---

# 5. Password Field

```js
password: {
  type: String,
  required: [true, "Password is required"],
  minlength: 6,
},
```

Simple:

- must exist
    
- minimum 6 characters
    

---

### Important Real-World Note

Usually passwords are NOT stored directly.

Before saving:

```js
"mypassword"
```

you hash it using bcrypt:

```js
"$2b$10$sdjkhf..."
```

---

# 6. Role Field

```js
role: {
  type: String,
  enum: ["user", "admin"],
  default: "user",
},
```

This is important.

---

## `enum`

```js
enum: ["user", "admin"]
```

Means only these values are allowed:

✅ allowed:

```js
"user"
"admin"
```

❌ not allowed:

```js
"manager"
"owner"
"hello"
```

---

## `default`

```js
default: "user"
```

If role is not provided:

```js
new User({...})
```

then MongoDB automatically stores:

```js
role: "user"
```

---

# 7. Bookmarks Field (Most Important Part)

```js
bookmarks: [
  {
    type: mongoose.Schema.Types.ObjectId,
    ref: "Book",
  },
],
```

This part confuses most beginners.

Let’s simplify it.

---

# What is bookmarks?

It is an ARRAY.

```js
bookmarks: []
```

A user can bookmark many books.

---

# What is ObjectId?

MongoDB gives every document a unique ID.

Example:

```js
_id: "6825d9a0f4a2..."
```

That ID type is called:

```js
ObjectId
```

---

# So this means:

```js
bookmarks: [
  ObjectId,
  ObjectId,
  ObjectId
]
```

Example:

```js
bookmarks: [
  "652a11...",
  "652a22...",
]
```

These IDs belong to books.

---

# What does `ref: "Book"` mean?

```js
ref: "Book"
```

This creates a relationship.

It tells Mongoose:

> “These ObjectIds belong to the Book model.”

Like SQL foreign keys.

---

# Example

Suppose Book collection has:

```js
{
  _id: "111",
  title: "Atomic Habits"
}
```

User:

```js
{
  username: "aman",
  bookmarks: ["111"]
}
```

The user bookmarked that book.

---

# Why use `ref`?

Because later you can use:

```js
.populate()
```

Example:

```js
User.findById(id).populate("bookmarks")
```

Without populate:

```js
bookmarks: ["111"]
```

With populate:

```js
bookmarks: [
  {
    _id: "111",
    title: "Atomic Habits"
  }
]
```

Very powerful feature.

---

# 8. timestamps

```js
{
  timestamps: true,
}
```

Mongoose automatically adds:

```js
createdAt
updatedAt
```

Example:

```js
{
  createdAt: "2026-05-15",
  updatedAt: "2026-05-15"
}
```

No need to manually create them.

---

# 9. Creating the Model

```js
const User = mongoose.model("User", userSchema);
```

This creates a model called `User`.

Think:

|Thing|Meaning|
|---|---|
|Schema|blueprint|
|Model|actual tool to interact with DB|

---

# Now you can do:

```js
User.create()
User.find()
User.findById()
User.deleteOne()
```

etc.

---

# 10. Exporting

```js
export default User;
```

Allows using this model in other files.

Example:

```js
import User from "./models/User.js";
```

---

# Final Big Picture

Your schema says:

> A User must have:

- username
    
- email
    
- password
    

Optional:

- role
    
- bookmarks
    

And:

- email must be unique
    
- role can only be user/admin
    
- bookmarks store references to Book documents
    
- timestamps are automatic
    

---

# Visual Structure

```js
User
│
├── username
├── email
├── password
├── role
├── bookmarks → references Book documents
├── createdAt
└── updatedAt
```

---

# Real MongoDB Document Example

```js
{
  _id: "abc123",

  username: "aman",

  email: "aman@gmail.com",

  password: "$2b$10$abc...", // hashed

  role: "user",

  bookmarks: [
    "bookid1",
    "bookid2"
  ],

  createdAt: "2026-05-15T10:00:00Z",

  updatedAt: "2026-05-15T10:00:00Z"
}
```

---

The hardest concept here is usually:

- `ObjectId`
    
- `ref`
    
- `populate`
    

Once you understand those, Mongoose becomes much easier.