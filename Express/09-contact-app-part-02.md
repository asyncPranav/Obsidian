
---


# 🧠 FILE 1: index.js (DATABASE CONNECTION PART)

```js
const mongoose = require("mongoose")
const Contact = require("./models/contacts.models")
```

---

# 🔹 1. Importing Mongoose

```js
const mongoose = require("mongoose")
```

## 🧠 What is this?

- This imports the **Mongoose library**
    
- Mongoose is used to connect Node.js with MongoDB
    

---

## 📌 Why needed?

Without mongoose:

- You cannot connect to MongoDB easily
    
- You cannot use schemas/models
    
- You must write raw MongoDB commands
    

With mongoose:  
✔ simple connection  
✔ schema-based structure  
✔ easy CRUD operations

---

# 🔹 2. Importing Model

```js
const Contact = require("./models/contacts.models")
```

## 🧠 What is this?

- You are importing your **Contact Model**
    
- This model represents your MongoDB collection
    

---

## 📌 Meaning:

|Part|Meaning|
|---|---|
|Contact|model object|
|contacts.models|file where schema + model defined|

---

## 🧠 Why import model here?

Because:  
👉 index.js = controller / server logic  
👉 model file = database structure

This separation is called:

✔ MVC pattern (Model - View - Controller)

---

# 🔌 3. MongoDB Connection

```js
mongoose.connect("mongodb://localhost:27017/contacts-crud")
```

---

## 🧠 What is this?

This connects your Node.js app to MongoDB database.

---

## 📌 Breakdown:

```id="conn1"
mongodb://localhost:27017/contacts-crud
```

|Part|Meaning|
|---|---|
|mongodb://|protocol|
|localhost|your system|
|27017|MongoDB default port|
|contacts-crud|database name|

---

## ⚠️ Important Concept

👉 MongoDB creates database automatically if not exists  
👉 You don’t need to manually create DB

---

# 🔹 4. Promise Handling

```js
.then(() => console.log("Database connected"))
.catch((err) => console.log(err));
```

---

## 🧠 Meaning:

MongoDB connection is **asynchronous**

So:

- `.then()` → success case
    
- `.catch()` → error case
    

---

## 📌 Flow:

```id="flow1"
Try connect MongoDB
   ↓
Success → print "Database connected"
   ↓
Fail → print error
```

---

# 🧠 FILE 2: contacts.models.js (MODEL FILE)

This is the MOST IMPORTANT part of Mongoose.

---

# 🔹 1. Import Mongoose again

```js
const mongoose = require("mongoose");
```

## 🧠 Why again?

Because:  
👉 every file is independent  
👉 model file also needs mongoose features

---

# 🧱 2. Schema Definition

```js
const contactSchema = mongoose.Schema({
```

---

## 🧠 What is Schema?

Schema = structure of database document

👉 It defines:

- fields
    
- types
    
- rules
    

---

## 📌 Your Schema:

```js
{
  first_name: String,
  last_name: String,
  email_name: String,
  phone_name: String,
  address: String
}
```

---

# 🔹 Field-by-field explanation

---

## 👤 first_name

```js
first_name: {
  type: String,
}
```

### 🧠 Meaning:

- stores first name
    
- only string allowed
    

---

## 👤 last_name

Same as above:

- stores last name
    
- type = String
    

---

## 📧 email_name

```js
email_name: {
  type: String,
}
```

### 🧠 Meaning:

- stores email
    
- currently no validation (we can improve later)
    

---

## 📱 phone_name

```js
phone_name: {
  type: String,
}
```

### 🧠 Why string?

Because:

- phone numbers may contain +91, -, etc.
    
- number type is not safe
    

---

## 🏠 address

```js
address: {
  type: String,
}
```

Stores full address.

---

# ⚠️ IMPORTANT IMPROVEMENT (you should know)

You can also add validation:

```js
first_name: {
  type: String,
  required: true,
  trim: true
}
```

---

# 🧠 3. Creating Model

```js
const contacts = mongoose.model("Contacts", contactSchema);
```

---

## 🧠 What is Model?

Model = interface between Node.js and MongoDB collection

---

## 📌 Breakdown:

|Part|Meaning|
|---|---|
|Contacts|model name|
|contactSchema|structure|
|contacts collection|MongoDB collection|

---

## ⚠️ Important rule:

MongoDB automatically converts:

```id="auto1"
Contacts → contacts (plural lowercase)
```

---

# 📦 4. Export Model

```js
module.exports = contacts;
```

---

## 🧠 Meaning:

- allows model to be used in other files
    
- like index.js
    

---

## 📌 Example usage:

```js
const Contact = require("./models/contacts.models")
```

Now you can:

```js
Contact.create()
Contact.find()
Contact.update()
```

---

# 🧠 COMPLETE FLOW (VERY IMPORTANT)

```id="flow2"
User submits form
        ↓
index.js receives request
        ↓
Contact model imported
        ↓
mongoose.connect() connects DB
        ↓
Contact.create(req.body)
        ↓
MongoDB stores data
```

---

# 🧠 ARCHITECTURE UNDERSTANDING

## You are building MVC style app:

|Layer|File|
|---|---|
|Model|contacts.models.js|
|Controller|index.js routes|
|View|EJS files|

---

# 🚀 WHAT THIS CODE ACTUALLY DOES

✔ Connects to MongoDB  
✔ Defines structure of contact data  
✔ Creates reusable model  
✔ Enables CRUD operations

---

# ⚠️ COMMON BEGINNER MISTAKES

### ❌ Wrong field names

```js
email_name ❌ (confusing)
email ✔ (better)
```

---

### ❌ Not validating data

You should later add:

```js
required: true
```

---

### ❌ Not handling async properly

Later use:

```js
async/await
```

---

# ⭐ FINAL SIMPLE SUMMARY

👉 mongoose.connect() → connects database  
👉 schema → defines structure  
👉 model → allows database operations  
👉 module.exports → share model  
👉 index.js → uses model for CRUD

---


No — it is **NOT necessary** that the model name and the import variable must both be capital or identical in casing.

But there are **important rules + best practices** you should understand clearly.

---

# 🧠 1. Model Name (Mongoose side)

```js
mongoose.model("Contacts", contactSchema);
```

## 📌 This name:

- `"Contacts"` is the **MODEL NAME inside Mongoose**
    
- It is used to create MongoDB collection automatically
    

---

## ⚠️ Important:

Mongoose will convert this internally:

```
Contacts → contacts (collection name)
```

So casing here is mostly for **convention + readability**, not requirement.

---

# 🧠 2. Import Variable Name (Node.js side)

```js
const Contact = require("./models/contacts.models");
```

## 📌 This `Contact`:

- is just a **JavaScript variable name**
    
- you can name it anything
    

### Valid examples:

```js
const Contact = require(...)
const contactModel = require(...)
const abc = require(...)
```

All are correct.

---

# 🧠 3. Do they need to match?

## ❌ NO — not required

These two are independent:

|Part|Example|Required to match?|
|---|---|---|
|Model name|`"Contacts"`|❌ No|
|Import variable|`Contact`|❌ No|

---

# 🧠 4. Why people use SAME or CAPITAL names?

Because of **convention (best practice)**:

## ✔ Model name (Mongoose)

Usually:

```js
"Contact"
"User"
"Product"
```

👉 Capitalized (PascalCase)

---

## ✔ Import variable

Usually:

```js
const Contact = require(...)
```

👉 Also capitalized to match model concept

---

# 🧠 5. Why PascalCase is used for Models?

Because models represent:

👉 "real-world objects"

|Model|Meaning|
|---|---|
|User|user collection|
|Contact|contact collection|
|Product|product collection|

So we write:

```js
const Contact = mongoose.model("Contact", schema);
```

---

# 🧠 6. BEST PRACTICE (IMPORTANT)

### ✅ Recommended way:

```js
// model file
const Contact = mongoose.model("Contact", contactSchema);
module.exports = Contact;
```

```js
// import
const Contact = require("./models/contact.model");
```

---

# 🧠 7. What NOT to worry about

You DO NOT need:

- same capitalization between files
    
- same variable names everywhere
    
- matching import and model names
    

---

# 🚀 FINAL SIMPLE RULE

✔ Model name → for MongoDB (naming convention only)  
✔ Import name → JavaScript variable (you decide)  
✔ They do NOT need to match  
✔ But keeping same style = clean code

---

# ⭐ One-line summary

👉 Mongoose model name and import variable name do NOT need to match — casing is only a convention for readability, not a rule.

---

# **PART-02 (worked on fetching db and show-contact)**

    

---

# 🧠 What Part-02 Actually Does

Now your app:

✔ reads contacts from MongoDB  
✔ displays them in table  
✔ uses dynamic IDs  
✔ shows single contact  
✔ prepares update/delete routes

Before this → static HTML  
Now → database-driven UI

---

# 📁 index.js (FULL UNDERSTANDING)

---

# 1. Import Section

```js
const express = require("express");
const mongoose = require("mongoose");
const Contact = require("./models/contacts.models");
```

## What each import does

### express

Creates backend server

### mongoose

Connects MongoDB with Node.js

### Contact

Your model representing `contacts` collection

This model allows:

```js
Contact.find()
Contact.create()
Contact.findById()
Contact.deleteOne()
```

---

# 2. Create Express App

```js
const app = express();
const PORT = 3000;
```

This creates your server.

Your app will run on:

```
http://localhost:3000
```

---

# 3. MongoDB Connection

```js
mongoose
  .connect("mongodb://localhost:27017/contacts-crud")
```

This connects your app to MongoDB database.

---

## Connection URL Breakdown

```
mongodb://localhost:27017/contacts-crud
```

|Part|Meaning|
|---|---|
|mongodb://|MongoDB protocol|
|localhost|local computer DB|
|27017|default MongoDB port|
|contacts-crud|database name|

MongoDB automatically creates database if not exists.

---

# Connection Success

```js
.then(() => console.log("Database connected"))
```

Runs when DB connected successfully.

---

# Connection Error

```js
.catch((err) => console.log(err));
```

Runs when DB connection fails.

---

# 4. Middlewares

## Set EJS engine

```js
app.set("view engine", "ejs");
```

This tells Express:

"Render `.ejs` templates"

Without this:

```
res.render() will not work
```

---

## Form middleware

```js
app.use(express.urlencoded({ extended: false }));
```

This reads form data.

Example form:

```
first_name=John
email=john@test.com
```

becomes:

```js
req.body = {
  first_name: "John",
  email: "john@test.com"
}
```

---

## Static middleware

```js
app.use(express.static("public"));
```

This allows:

```
public/style.css
public/bootstrap.css
```

to be used in HTML.

---

# 5. HOME ROUTE (MOST IMPORTANT)

```js
app.get("/", async (req, res) => {
  const contacts = await Contact.find();
  res.render("home", { contacts: contacts });
});
```

This route:

✔ gets all contacts from DB  
✔ sends to EJS  
✔ displays table

---

# Step-by-step Flow

User opens:

```
http://localhost:3000/
```

This route runs.

---

## Step 1: Fetch contacts

```js
const contacts = await Contact.find();
```

This queries MongoDB.

It returns array:

```
[
 {first_name:"John"},
 {first_name:"Alex"}
]
```

---

## Step 2: Send data to EJS

```js
res.render("home", { contacts: contacts });
```

This sends:

```
contacts = array
```

to home.ejs

---

# 🧠 home.ejs Dynamic Loop

```ejs
<% contacts.forEach(contact=> { %>
```

This loops over contacts.

If 3 contacts:

Loop runs 3 times.

---

Example:

contacts =

```
[
 {name:"A"},
 {name:"B"},
 {name:"C"}
]
```

Loop prints 3 rows.

---

# Display fields

```ejs
<%= contact.first_name %>
```

Prints first name.

---

```ejs
<%= contact.last_name %>
```

Prints last name.

---

```ejs
<%= contact.email %>
```

Prints email.

---

```ejs
<%= contact.phone %>
```

Prints phone.

---
Got it — we’ll focus **ONLY on SHOW CONTACT feature** and go step-by-step from absolute zero, very slowly, like building blocks.

---

# 🧠 SHOW CONTACT (FULL DEEP NOTES FROM SCRATCH)

This feature means:

👉 When user clicks a contact → we show full details of ONLY that contact

---

# 🌍 BIG IDEA FIRST (VERY IMPORTANT)

Your app has many contacts like:

```json
[
  { "_id": "1", "name": "John" },
  { "_id": "2", "name": "Mike" },
  { "_id": "3", "name": "Sara" }
]
```

---

## 🧠 PROBLEM

If user clicks “Show”, we must know:

👉 WHICH contact to show?

Answer:

👉 We use `_id`

---

# 🧠 STEP 1: WHERE DOES SHOW BUTTON COME FROM?

In home.ejs:

```ejs
<a href="/show-contact/<%= contact._id %>">
```

---

## 🧠 BREAK THIS VERY SLOWLY

### Step 1: contact._id

Example:

```text
contact._id = "abc123"
```

---

### Step 2: EJS replaces it

This:

```ejs
/show-contact/<%= contact._id %>
```

becomes:

```text
/show-contact/abc123
```

---

## 🧠 MEANING

👉 “Open show page for contact with id abc123”

---

# 🧠 STEP 2: WHAT HAPPENS WHEN USER CLICKS?

User clicks link:

```text
/show-contact/abc123
```

Browser sends request to server:

👉 GET request

---

# 🧠 STEP 3: EXPRESS ROUTE CATCHES IT

You wrote:

```js
app.get("/show-contact/:id", async (req, res) => {
```

---

## 🧠 WHAT IS :id?

This means:

👉 “anything after /show-contact/ will be stored in a variable called id”

---

### Example:

```text
/show-contact/abc123
```

Express does:

```js
req.params.id = "abc123"
```

---

## 🧠 SIMPLE UNDERSTANDING

|URL|req.params.id|
|---|---|
|/show-contact/1|1|
|/show-contact/abc|abc|
|/show-contact/xyz|xyz|

---

# 🧠 STEP 4: FIND DATA IN DATABASE

Now code runs:

```js
const contact = await Contact.findById(req.params.id);
```

---

## 🧠 BREAK IT SLOWLY

We already know:

```js
req.params.id = "abc123"
```

So actual code becomes:

```js
Contact.findById("abc123")
```

---

## 🧠 WHAT DOES MONGOOSE DO?

Mongoose goes to MongoDB and says:

👉 “Find document where _id = abc123”

---

## 🧠 MongoDB returns:

```json
{
  "_id": "abc123",
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@test.com",
  "phone": "12345",
  "address": "India"
}
```

---

## 🧠 STORED IN VARIABLE

```js
contact = result
```

Now:

```js
contact.first_name = "John"
```

---

# 🧠 STEP 5: SEND DATA TO EJS

Now server sends data:

```js
res.render("show-contact", { contact: contact });
```

---

## 🧠 MEANING

👉 Open show-contact.ejs  
👉 Give it variable called contact  
👉 Put MongoDB data inside it

---

## 🧠 NOW EJS HAS ACCESS TO:

```js
contact.first_name
contact.email
contact.phone
```

---

# 🧠 STEP 6: SHOW DATA IN EJS PAGE

Now in show-contact.ejs:

```ejs
<%= contact.first_name %>
```

👉 Output:

```text
John
```

---

```ejs
<%= contact.email %>
```

👉 Output:

```text
john@test.com
```

---

```ejs
<%= contact.phone %>
```

👉 Output:

```text
12345
```

---

# 🧠 FULL STEP-BY-STEP FLOW (VERY IMPORTANT)

Now imagine full story:

---

## 🎬 STEP 1: User clicks button

```text
Show John
```

---

## 🎬 STEP 2: Browser opens link

```text
/show-contact/abc123
```

---

## 🎬 STEP 3: Express catches request

```js
app.get("/show-contact/:id")
```

👉 id = abc123

---

## 🎬 STEP 4: Server reads ID

```js
req.params.id = "abc123"
```

---

## 🎬 STEP 5: MongoDB search

```js
Contact.findById("abc123")
```

---

## 🎬 STEP 6: MongoDB returns data

```json
John's full record
```

---

## 🎬 STEP 7: Send to EJS

```js
res.render("show-contact", { contact })
```

---

## 🎬 STEP 8: EJS shows data

```text
John
john@test.com
12345
```

---

# 🧠 WHY THIS IS IMPORTANT

Because:

👉 We cannot show ALL contacts  
👉 We must show ONLY ONE contact  
👉 `_id` helps identify exactly which one

---

# 🧠 SIMPLE REAL LIFE EXAMPLE

Think like:

|System|Real Life|
|---|---|
|_id|Roll number|
|findById|Find student by roll number|
|show page|Student report card|

---

# 🧠 ONE LINE MEMORY

👉 Show Contact works by taking `_id` from URL → finding data in MongoDB → displaying it in EJS.

---

# 🚀 SUPER SIMPLE FINAL SUMMARY

✔ Button sends `_id` in URL  
✔ Express reads `:id`  
✔ MongoDB finds matching record  
✔ EJS displays data

---



# **📲 ADD CONTACT**


---

# 🧠 WHAT IS "ADD CONTACT" FEATURE?

User:

1. Opens add contact page
    
2. Fills form
    
3. Clicks save
    
4. Data goes to server
    
5. Server saves in MongoDB
    
6. Redirect to home page
    
7. New contact appears
    

---

# 🌍 STEP 1 — USER OPENS ADD PAGE

Route:

```js
app.get("/add-contact", (req, res) => {
  res.render("add-contact");
});
```

### What happens?

User goes to:

```
http://localhost:3000/add-contact
```

Server sends:

```
add-contact.ejs
```

---

# 🧠 STEP 2 — FORM IN EJS

Your form:

```html
<form action="/add-contact" method="post">
```

This is VERY IMPORTANT.

---

## What does this mean?

action="/add-contact"  
👉 send data to this route

method="post"  
👉 send data using POST request

---

# 🧠 When user clicks SAVE

Browser sends:

```
POST /add-contact
```

with form data.

---

# 🧠 STEP 3 — INPUT FIELDS

Example:

```html
<input type="text" name="first_name">
```

IMPORTANT:

The **name** attribute becomes the key.

---

So if user types:

```
John
```

Browser sends:

```
first_name = John
```

---

All inputs:

```html
name="first_name"
name="last_name"
name="email"
name="phone"
name="address"
```

Browser sends:

```json
{
first_name: "John",
last_name: "Doe",
email: "john@test.com",
phone: "1234",
address: "India"
}
```

---

# 🧠 STEP 4 — EXPRESS RECEIVES DATA

Route:

```js
app.post("/add-contact", async (req, res) => {
```

This route runs when form is submitted.

---

# 🧠 Where is form data?

Inside:

```
req.body
```

Example:

```js
console.log(req.body)
```

Output:

```json
{
first_name: "John",
last_name: "Doe",
email: "john@test.com",
phone: "1234",
address: "India"
}
```

---

# 🧠 WHY req.body WORKS?

Because you added middleware:

```js
app.use(express.urlencoded({ extended: false }));
```

This converts form data → JavaScript object.

---

# 🧠 STEP 5 — SAVE INTO DATABASE

Your code:

```js
await Contact.create(req.body);
```

This is VERY IMPORTANT.

---

## What does create() do?

It:

1. Takes data
    
2. Creates new document
    
3. Inserts into MongoDB
    

---

Equivalent long version:

```js
await Contact.create({
first_name: req.body.first_name,
last_name: req.body.last_name,
email: req.body.email,
phone: req.body.phone,
address: req.body.address
})
```

But shortcut:

```js
await Contact.create(req.body)
```

because names match.

---

# 🧠 WHAT HAPPENS IN MONGODB?

MongoDB creates:

```json
{
"_id": "randomID",
"first_name": "John",
"last_name": "Doe",
"email": "john@test.com",
"phone": "1234",
"address": "India"
}
```

Notice:

MongoDB automatically adds:

```
_id
```

---

# 🧠 STEP 6 — REDIRECT

After saving:

```js
res.redirect("/")
```

This means:

👉 go to home page

So browser loads:

```
GET /
```

---

# 🧠 STEP 7 — HOME ROUTE RUNS AGAIN

```js
app.get("/", async (req, res) => {
  const contacts = await Contact.find();
  res.render("home", { contacts });
});
```

Now database includes new contact.

So new contact appears in list.

---

# 🧠 FULL FLOW (VERY IMPORTANT)

User fills form:

```
John
Doe
```

↓

Click SAVE

↓

POST /add-contact

↓

req.body gets data

↓

Contact.create(req.body)

↓

MongoDB saves contact

↓

res.redirect("/")

↓

Home page reloads

↓

Contact visible

---

# 🧠 VISUAL FLOW

```
Form → POST → req.body → create() → MongoDB → redirect → home
```

---

# 🧠 SIMPLE REAL LIFE EXAMPLE

Think like:

You fill admission form  
↓

Submit to office  
↓

Office stores in register  
↓

They show your name in list

Same thing.

---

# 🧠 IMPORTANT POINTS

### 1. name attribute is important

```html
name="first_name"
```

must match schema.

---

### 2. POST route handles form

```
app.post("/add-contact")
```

---

### 3. create() inserts document

```
Contact.create()
```

---

### 4. redirect refreshes page

```
res.redirect("/")
```

---

# 🧠 COMPLETE ADD ROUTE

```js
app.post("/add-contact", async (req, res) => {

  await Contact.create(req.body);

  res.redirect("/");

});
```

---

# 🧠 WHAT YOU LEARNED

You now know:

✔ form submit  
✔ POST request  
✔ req.body  
✔ mongoose create  
✔ insert document  
✔ redirect  
✔ full flow

---

# 🧠 ONE LINE MEMORY

Add contact = form submit → req.body → create() → MongoDB → redirect

---

# **📲 UPDATE CONTACT**


Now we’ll learn **UPDATE CONTACT** from scratch — step-by-step, beginner friendly, very detailed.

This feature means:

> User clicks **Edit → form opens with old data → user changes → save → DB updates**

---

# 🧠 FULL UPDATE FLOW (BIG PICTURE)

```
Click Edit
   ↓
GET /update-contact/:id
   ↓
Find contact by id
   ↓
Send contact to EJS
   ↓
Form pre-filled with values
   ↓
User edits
   ↓
POST /update-contact/:id
   ↓
findByIdAndUpdate()
   ↓
redirect("/")
```

---

# STEP 1 — EDIT BUTTON (HOME PAGE)

Your code:

```ejs
<a href="update-contact/<%= contact._id %>" 
   class="btn btn-sm btn-circle btn-outline-secondary">
```

Suppose contact id:

```
contact._id = 123abc
```

Browser sees:

```
/update-contact/123abc
```

So clicking edit goes to:

```
GET /update-contact/123abc
```

This tells server:

👉 "I want to edit contact with id 123abc"

---

# STEP 2 — UPDATE GET ROUTE

Your code:

```js
app.get("/update-contact/:id", async (req, res) => {
  const contact = await Contact.findById(req.params.id);
  res.render("update-contact", { contact: contact });
});
```

Let's break slowly.

---

## PART 1 — :id

Route:

```
/update-contact/:id
```

URL:

```
/update-contact/123abc
```

Express stores:

```
req.params.id = "123abc"
```

---

## PART 2 — findById

```js
const contact = await Contact.findById(req.params.id);
```

This means:

1. Go to MongoDB
    
2. Find document with this ID
    
3. Return that contact
    

Example DB:

```
{
_id:123abc
first_name: John
last_name: Doe
email: john@test.com
}
```

Now `contact` variable contains:

```
contact.first_name → John
contact.last_name → Doe
```

---

## PART 3 — send to EJS

```js
res.render("update-contact", { contact: contact });
```

This sends data to EJS:

```
contact = {
first_name: "John"
last_name: "Doe"
}
```

---

# STEP 3 — UPDATE FORM

Your code:

```html
<form action="/update-contact/<%= contact._id %>" method="post">
```

This becomes:

```
/update-contact/123abc
```

So when user clicks save:

```
POST /update-contact/123abc
```

---

# STEP 4 — PREFILL INPUT VALUES

Your input:

```html
<input type="text" 
       name="first_name" 
       value=<%=contact.first_name %>>
```

EJS replaces:

```
value="John"
```

So form shows:

```
John
```

This is called:

👉 pre-filled form  
👉 edit form  
👉 populate form

---

# STEP 5 — ALL INPUTS PREFILLED

Your code:

```html
value=<%=contact.first_name %>
value=<%=contact.last_name %>
value=<%=contact.email %>
value=<%=contact.phone %>
```

Textarea:

```html
<textarea><%= contact.address %></textarea>
```

So user sees existing data.

---

# STEP 6 — USER EDITS

User changes:

```
John → Johnny
```

Clicks save.

Form sends:

```
POST /update-contact/123abc
```

Body:

```
{
first_name: "Johnny",
last_name: "Doe"
}
```

---

# STEP 7 — UPDATE POST ROUTE

Your code:

```js
app.post("/update-contact/:id", async (req, res) => {

  await Contact.findByIdAndUpdate(req.params.id, req.body);

  res.redirect("/");

});
```

---

# STEP 8 — findByIdAndUpdate

This line:

```js
await Contact.findByIdAndUpdate(req.params.id, req.body);
```

means:

1. find contact using id
    
2. update with new data
    
3. save in database
    

Example:

Before:

```
John
```

After:

```
Johnny
```

---

# STEP 9 — REDIRECT

```js
res.redirect("/")
```

This reloads home page.

Home route:

```js
const contacts = await Contact.find();
```

Now updated data appears.

---

# COMPLETE UPDATE FLOW

User clicks Edit  
↓

GET /update-contact/:id  
↓

findById()  
↓

send contact to EJS  
↓

form prefilled  
↓

user edits  
↓

POST /update-contact/:id  
↓

findByIdAndUpdate()  
↓

redirect("/")  
↓

updated contact shown

---

# VERY IMPORTANT RULE

Form action must include ID

Correct:

```html
<form action="/update-contact/<%= contact._id %>" method="post">
```

Wrong:

```
/update-contact
```

Because server must know WHICH contact to update.

---

# VERY IMPORTANT RULE 2

Input names must match schema keys

Schema:

```
first_name
last_name
email
phone
```

Inputs:

```
name="first_name"
name="last_name"
```

Otherwise update won't work.

---

# VERY IMPORTANT RULE 3

Always use quotes in value

Better version:

```html
value="<%= contact.first_name %>"
```

NOT

```
value=<%= contact.first_name %>
```

Because spaces break HTML.

---

# FINAL UPDATE ROUTES

GET:

```js
app.get("/update-contact/:id", async (req, res) => {
  const contact = await Contact.findById(req.params.id);
  res.render("update-contact", { contact });
});
```

POST:

```js
app.post("/update-contact/:id", async (req, res) => {
  await Contact.findByIdAndUpdate(req.params.id, req.body);
  res.redirect("/");
});
```

---

# ONE LINE MEMORY

Edit → GET id → fill form → POST id → findByIdAndUpdate → redirect

---

# CRUD COMPLETED

Create → Contact.create()  
Read → Contact.find()  
Show → findById()  
Update → findByIdAndUpdate()  
Delete → findByIdAndDelete()

---


# **📲 DELETE CONTACT**


Now we’ll learn **DELETE CONTACT** from scratch in **very beginner-friendly detailed notes**.

This is the last CRUD operation:

- Create ✅
    
- Read ✅
    
- Update ✅
    
- Delete ✅ (this one)
    

---

# 🧠 WHAT IS DELETE CONTACT?

User clicks delete button  
→ contact removed from database  
→ page refresh  
→ contact disappears

---

# 🌍 DELETE FLOW (BIG PICTURE)

```text
Click delete
   ↓
GET /delete-contact/:id
   ↓
Express receives id
   ↓
findByIdAndDelete()
   ↓
MongoDB deletes record
   ↓
res.redirect("/")
   ↓
Home reloads
   ↓
Contact removed
```

---

# STEP 1 — DELETE BUTTON (EJS)

Your code:

```ejs
<a href="/delete-contact/<%= contact._id %>"
   class="btn btn-sm btn-circle btn-outline-danger"
   onclick="confirm('Are you sure?')">
```

---

## What this does

When page renders:

```text
contact._id = abc123
```

EJS becomes:

```html
<a href="/delete-contact/abc123">
```

So clicking delete sends:

```text
GET /delete-contact/abc123
```

---

# STEP 2 — EXPRESS DELETE ROUTE

Your route:

```js
app.get("/delete-contact/:id", async (req, res) => {
  await Contact.findByIdAndDelete(req.params.id);
  res.redirect("/");
});
```

We explain line by line.

---

# STEP 3 — :id PARAMETER

Route:

```js
/delete-contact/:id
```

Example URL:

```
/delete-contact/abc123
```

Express stores:

```js
req.params.id = "abc123"
```

This is the ID of contact to delete.

---

# STEP 4 — DELETE FROM DATABASE

This line:

```js
await Contact.findByIdAndDelete(req.params.id);
```

means:

1. go to MongoDB
    
2. find document with this `_id`
    
3. delete it
    

---

# WHAT MONGODB DOES

Before:

```json
[
 { "_id":"1", name:"John"},
 { "_id":"2", name:"Mike"},
 { "_id":"3", name:"Sara"}
]
```

User deletes Mike:

```
/delete-contact/2
```

After delete:

```json
[
 { "_id":"1", name:"John"},
 { "_id":"3", name:"Sara"}
]
```

Mike removed.

---

# STEP 5 — REDIRECT

After delete:

```js
res.redirect("/")
```

This means:

Go back to home page.

---

# STEP 6 — HOME ROUTE RUNS AGAIN

```js
app.get("/", async (req, res) => {
  const contacts = await Contact.find();
  res.render("home", { contacts });
});
```

Now database updated.

So deleted contact not shown.

---

# FULL DELETE FLOW (VERY SLOW)

User clicks delete  
↓

Browser opens:

```
/delete-contact/abc123
```

↓

Express route matches:

```
/delete-contact/:id
```

↓

Express extracts id:

```
req.params.id = abc123
```

↓

Mongoose deletes:

```
findByIdAndDelete("abc123")
```

↓

MongoDB removes contact

↓

redirect("/")

↓

Home reloads

↓

Contact gone

---

# VERY IMPORTANT LINE

```js
await Contact.findByIdAndDelete(req.params.id);
```

Syntax:

```js
Model.findByIdAndDelete(id)
```

This:

- finds document
    
- deletes document
    
- returns deleted document
    

---

# WHY WE PASS ID IN URL?

Because database has many contacts:

```
John   → id 1
Mike   → id 2
Sara   → id 3
```

We must tell server WHICH to delete.

So we pass:

```
/delete-contact/2
```

---

# CONFIRM BOX

Your code:

```html
onclick="confirm('Are you sure?')"
```

This shows popup:

```
Are you sure?
OK / Cancel
```

NOTE:

Better version:

```html
onclick="return confirm('Are you sure?')"
```

Otherwise cancel still deletes.

---

# FINAL DELETE ROUTE

```js
app.get("/delete-contact/:id", async (req, res) => {

  await Contact.findByIdAndDelete(req.params.id);

  res.redirect("/");

});
```

---

# DELETE BUTTON FINAL

```ejs
<a href="/delete-contact/<%= contact._id %>"
   class="btn btn-danger"
   onclick="return confirm('Are you sure?')">
Delete
</a>
```

---

# WHAT YOU LEARNED

You now understand:

- delete button link
    
- dynamic route :id
    
- req.params.id
    
- findByIdAndDelete
    
- redirect
    
- full delete flow
    

---

# ONE LINE MEMORY

Delete contact = click delete → send id → findByIdAndDelete → redirect → removed

---

Now your CRUD app is complete:

Create → Contact.create()  
Read → Contact.find()  
Update → findByIdAndUpdate()  
Delete → findByIdAndDelete()