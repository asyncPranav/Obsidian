
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

Great. You already understand basic schema syntax, so now let’s understand this like a real backend developer.

This schema is basically defining:

> “How a User document should look inside MongoDB.”

Think of schema as a **blueprint/template/rules** for user data.

---

# First Big Picture

When a user signs up, data may look like this in database:

```js
{
  _id: "6845abc...",
  username: "darkcoder",
  email: "dark@example.com",
  password: "hashedpassword123",
  role: "user",
  bookmarks: ["bookId1", "bookId2"],
  createdAt: "2026-05-15",
  updatedAt: "2026-05-15"
}
```

This schema controls:

- what fields exist
    
- what type they should be
    
- validations
    
- defaults
    
- relations with other collections
    

---

# Step-by-Step Full Explanation

---

# 1. Import mongoose

```js
import mongoose from "mongoose";
```

Here you import Mongoose.

Mongoose helps Node.js talk with MongoDB easily.

Without mongoose:

- you manually handle MongoDB queries
    

With mongoose:

- schemas
    
- validation
    
- models
    
- easier queries
    
- middleware
    
- population
    
- hooks
    

become easy.

---

# 2. Creating Schema

```js
const userSchema = new mongoose.Schema(
```

This creates a schema named `userSchema`.

Think:

```text
Schema = Rules for document structure
```

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

This means username field should follow these rules.

---

## type: String

```js
type: String
```

Username must be text/string.

Valid:

```js
username: "dark"
```

Invalid:

```js
username: 123
```

---

# required

```js
required: [true, "Username is required"]
```

Means:

- username is compulsory
    
- if missing, mongoose throws error
    

Example:

```js
await User.create({
  email: "a@gmail.com"
})
```

Error:

```text
Username is required
```

---

# Why Array?

```js
[true, "Username is required"]
```

Because:

- first value = validation condition
    
- second value = custom error message
    

Equivalent:

```js
required: true
```

but custom message is better.

---

# trim: true

```js
trim: true
```

Removes extra spaces from start/end.

Example:

```js
"   dark   "
```

becomes:

```js
"dark"
```

Very useful.

---

# minlength

```js
minlength: 3
```

Minimum 3 characters required.

Invalid:

```js
"ab"
```

---

# maxlength

```js
maxlength: 30
```

Maximum 30 characters allowed.

Prevents very long usernames.

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

---

# unique: true

VERY IMPORTANT.

```js
unique: true
```

Means:

- duplicate emails not allowed
    

Two users cannot have:

```text
test@gmail.com
```

---

# Internally What Happens?

Mongoose creates a unique index in MongoDB.

MongoDB checks duplicates.

---

# lowercase: true

```js
lowercase: true
```

Automatically converts:

```js
"DARK@GMAIL.COM"
```

to:

```js
"dark@gmail.com"
```

Useful because:

- emails should be case-insensitive
    

---

# 5. Password Field

```js
password: {
  type: String,
  required: [true, "Password is required"],
  minlength: 6,
},
```

Simple validation:

- password required
    
- minimum 6 chars
    

---

# IMPORTANT REAL WORLD NOTE

You NEVER store plain password.

Wrong:

```js
password: "123456"
```

Correct:

- hash using bcrypt
    

Stored value:

```js
"$2b$10$askjdhaskjdh..."
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

This is for authorization.

Example:

- normal user
    
- admin user
    

---

# enum

```js
enum: ["user", "admin"]
```

Means ONLY these values allowed.

Valid:

```js
role: "admin"
```

Invalid:

```js
role: "manager"
```

Mongoose throws validation error.

---

# default

```js
default: "user"
```

If role not provided:

```js
await User.create({...})
```

then automatically:

```js
role = "user"
```

---

# 7. Bookmarks Field (MOST IMPORTANT PART)

This is probably where confusion started.

```js
bookmarks: [
  {
    type: mongoose.Schema.Types.ObjectId,
    ref: "Book",
  },
],
```

This creates RELATION between collections.

---

# First Understand MongoDB Relations

MongoDB has collections.

Example:

## Users Collection

```js
{
  _id: "u1",
  username: "dark",
  bookmarks: ["b1", "b2"]
}
```

---

## Books Collection

```js
{
  _id: "b1",
  title: "Atomic Habits"
}
```

---

# ObjectId

```js
type: mongoose.Schema.Types.ObjectId
```

Means bookmarks will store MongoDB IDs.

Example:

```js
bookmarks: [
   "68456asd65a6sd5",
   "68456asd65a6sd6"
]
```

These IDs belong to books.

---

# Why Array?

```js
bookmarks: [ ... ]
```

Because one user can bookmark MANY books.

Without array:

- only one bookmark possible
    

With array:

- multiple bookmarks possible
    

---

# ref: "Book"

```js
ref: "Book"
```

Means:

> "These ObjectIds belong to Book model"

This enables:

```js
.populate()
```

VERY IMPORTANT FEATURE.

---

# Example Without populate()

```js
const user = await User.findById(id)

console.log(user.bookmarks)
```

Output:

```js
[
  "68456asd65a6sd5",
  "68456asd65a6sd6"
]
```

Only IDs.

---

# Example With populate()

```js
const user = await User.findById(id).populate("bookmarks")
```

Now mongoose fetches full book documents automatically.

Output:

```js
{
  username: "dark",
  bookmarks: [
    {
      title: "Atomic Habits"
    },
    {
      title: "Deep Work"
    }
  ]
}
```

THIS IS RELATIONSHIP IN MONGODB.

---

# 8. timestamps: true

```js
{
  timestamps: true,
}
```

Automatically adds:

```js
createdAt
updatedAt
```

Example:

```js
{
  createdAt: "2026-05-15T10:00",
  updatedAt: "2026-05-15T10:30"
}
```

---

# 9. Creating Model

```js
const User = mongoose.model("User", userSchema);
```

MOST IMPORTANT LINE.

---

# Difference Between Schema and Model

## Schema

Blueprint/rules.

## Model

Actual tool used to interact with DB.

---

# Example

```js
User.create()
User.find()
User.findById()
User.deleteOne()
```

All possible because of model.

---

# First Parameter

```js
"User"
```

Model name.

MongoDB collection becomes:

```text
users
```

Mongoose pluralizes automatically.

---

# 10. Export

```js
export default User;
```

Allows using User model in other files.

Example:

```js
import User from "../models/user.model.js";
```

---

# Overall Flow of This Schema

```text
Frontend sends signup data
        ↓
Express route receives it
        ↓
Controller validates logic
        ↓
User model stores data
        ↓
Mongoose validates schema
        ↓
MongoDB stores document
```

---

# Real Professional Understanding

This schema handles:

- validation
    
- sanitization
    
- relations
    
- defaults
    
- structure
    
- database consistency
    

all at one place.

That’s why schemas are extremely important in backend.

---

#DOUBT - **Bookmark field in userSchema**

