
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



# 👤 User.model.js

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

Good. This is the MOST important concept in MongoDB + Mongoose — relationships between collections.

You understood basic schema fields because they store normal data directly.

But `bookmarks` is different because it stores **references/links** to another collection.

Let’s understand this VERY slowly.

---

# First Imagine Real App

Suppose you are building:

```text
Book Reading App
```

Users can:

- signup
    
- login
    
- bookmark books
    

---

# You Have TWO Collections

## 1. Users Collection

Stores users.

Example:

```js
{
   username: "dark",
   email: "dark@gmail.com"
}
```

---

## 2. Books Collection

Stores books.

Example:

```js
{
   title: "Atomic Habits",
   author: "James Clear"
}
```

---

# Now Problem

Suppose user bookmarked 3 books.

Question:

Where should we store bookmarks?

Answer:  
Inside user document.

---

# How Beginner Thinks

Beginner may think:

```js
bookmarks: [
   "Atomic Habits",
   "Deep Work"
]
```

BAD APPROACH ❌

Why?

Because:

- repeated data
    
- inefficient
    
- difficult updates
    
- no relation
    

---

# Professional Approach

Instead of storing full book data:

Store ONLY IDs of books.

Example:

```js
bookmarks: [
   "68456asd65a6sd5",
   "68456asd65a6sd6"
]
```

These IDs belong to books collection.

---

# Real Visualization

---

# Books Collection

```js
{
   _id: "b1",
   title: "Atomic Habits"
}

{
   _id: "b2",
   title: "Deep Work"
}
```

---

# Users Collection

```js
{
   username: "dark",
   bookmarks: ["b1", "b2"]
}
```

Meaning:

```text
This user bookmarked books having IDs:
b1 and b2
```

---

# NOW UNDERSTAND THIS CODE

```js
bookmarks: [
  {
    type: mongoose.Schema.Types.ObjectId,
    ref: "Book",
  },
],
```

Let’s break every word.

---

# bookmarks: [ ]

```js
bookmarks: [ ]
```

Means:

> bookmarks is an ARRAY

Why array?

Because user can bookmark MANY books.

Example:

```js
bookmarks: ["b1", "b2", "b3"]
```

---

# type: mongoose.Schema.Types.ObjectId

```js
type: mongoose.Schema.Types.ObjectId
```

Means:

> Each item inside array must be a MongoDB ObjectId

MongoDB automatically creates unique IDs.

Example:

```text
68456asd65a6sd5
```

This ID points to a book document.

---

# VERY IMPORTANT

This is NOT normal string.

This is special MongoDB ID type.

---

# ref: "Book"

```js
ref: "Book"
```

THIS IS THE MAGIC PART.

Means:

> "These ObjectIds belong to Book model"

This creates relationship.

---

# Simple Human Language

This line means:

```text
User has many bookmarked books.
Bookmarks stores IDs of Book documents.
```

---

# Real Data Example

Suppose database has:

---

# Book Collection

```js
{
   _id: ObjectId("111"),
   title: "Atomic Habits"
}

{
   _id: ObjectId("222"),
   title: "Deep Work"
}
```

---

# User Collection

```js
{
   username: "dark",
   bookmarks: [
      ObjectId("111"),
      ObjectId("222")
   ]
}
```

---

# What Happens Internally

User document DOES NOT contain full book.

It only stores references.

Like this:

```text
dark user → bookmarked → book 111
dark user → bookmarked → book 222
```

---

# Why This Is Powerful

Because:

- less repeated data
    
- efficient
    
- scalable
    
- easy updates
    

---

# Example Problem Without References

Suppose 10,000 users bookmarked:

```text
Atomic Habits
```

If title changes:

- you'd update 10,000 documents ❌
    

With references:

- update only ONE book document ✅
    

---

# Now Most Important Thing: populate()

This is why `ref` exists.

---

# Without populate()

```js
const user = await User.findById(id);

console.log(user.bookmarks);
```

Output:

```js
[
   ObjectId("111"),
   ObjectId("222")
]
```

Only IDs.

Not useful for frontend.

---

# With populate()

```js
const user = await User.findById(id)
   .populate("bookmarks");
```

NOW mongoose does magic.

It sees:

```js
ref: "Book"
```

Then automatically fetches books.

---

# Final Result

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

THIS IS WHY REFERENCES EXIST.

---

# Visual Flow

```text
User Document
     ↓
bookmarks array
     ↓
contains Book IDs
     ↓
populate()
     ↓
Mongoose fetches actual books
```

---

# Real Backend Flow

When user clicks:

```text
Bookmark Book
```

Backend may do:

```js
await User.findByIdAndUpdate(userId, {
   $push: { bookmarks: bookId }
});
```

Meaning:

```text
Add this book's ID into bookmarks array
```

---

# Example

If book ID is:

```text
111
```

Then:

Before:

```js
bookmarks: []
```

After:

```js
bookmarks: ["111"]
```

---

# Remove Bookmark

```js
await User.findByIdAndUpdate(userId, {
   $pull: { bookmarks: bookId }
});
```

Removes book ID.

---

# Think of It Like YouTube

When you:

- like video
    
- save playlist
    
- add favorites
    

Platforms usually store:

- references/IDs
    
- not full duplicated objects
    

Same concept here.

---

# One Final Important Concept

This relationship is:

```text
One User → Many Books
```

because:

- one user can bookmark many books
    

This is called:

```text
One-to-Many Relationship
```

---

# SUPER SIMPLE FINAL SUMMARY

```js
bookmarks: [
  {
    type: mongoose.Schema.Types.ObjectId,
    ref: "Book",
  },
],
```

Means:

```text
bookmarks = array

Each item = MongoDB ObjectId

These ObjectIds belong to Book model

Used to connect User and Book collections
```

And `.populate()` converts IDs into actual book data.


---

# 📚 Book.model.js

```js
import mongoose from "mongoose";

const bookSchema = new mongoose.Schema(
  {
    title: {
      type: String,
      required: [true, "Title is required"],
      trim: true,
      minlength: 2,
      maxlength: 100,
    },

    author: {
      type: String,
      required: [true, "Author is required"],
      trim: true,
      maxlength: 100,
    },

    description: {
      type: String,
      required: [true, "Description is required"],
      minlength: 10,
    },

    category: {
      type: String,
      trim: true,
      default: "General",
    },

    coverImage: {
      type: String, // file path or URL
      default: "",
    },

    pdfFile: {
      type: String, // file path
      default: "",
    },

    uploadedBy: {
      type: mongoose.Schema.Types.ObjectId,
      ref: "User",
      required: true,
    },

    averageRating: {
      type: Number,
      default: 0,
    },

    totalRatings: {
      type: Number,
      default: 0,
    },
  },
  {
    timestamps: true,
  }
);

const Book = mongoose.model("Book", bookSchema);

export default Book;
```