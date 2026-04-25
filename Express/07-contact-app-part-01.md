
---


Below are **VERY detailed beginner notes — Part 01: Contact App (Express + EJS UI only)**  
(No MongoDB yet — just structure, routing, templates)

This part builds:

- UI pages
    
- routes
    
- navigation
    
- layout (header/footer)
    
- form pages
    

No database yet.

---

# 🧠 What this Contact App is

This is a **Contact Management App UI**

Features (not functional yet):

- Show all contacts
    
- Add contact
    
- View contact
    
- Update contact
    
- Delete contact
    

Right now it’s only **frontend + routing structure**

---

# 📁 Project Structure

```
project
│
├── index.js
│
├── views
│   ├── home.ejs
│   ├── add-contact.ejs
│   ├── show-contact.ejs
│   ├── update-contact.ejs
│   │
│   └── partials
│       ├── header.ejs
│       └── footer.ejs
│
└── public
    ├── bootstrap.min.css
    └── custom.css
```

---

# 🧠 index.js (Main Server File)

This file controls:

- routes
    
- rendering
    
- middleware
    

---

# 1. Import Express

```js
const express = require("express");
const app = express();
const PORT = 3000;
```

Creates Express server.

---

# 2. Middleware Section

### Set EJS engine

```js
app.set("view engine", "ejs");
```

Meaning:

- Express will render `.ejs` files
    
- located inside `views` folder
    

---

### Form data middleware

```js
app.use(express.urlencoded({ extended: false }));
```

Purpose:

- read form data
    
- store in `req.body`
    

Without this:

```
req.body = undefined
```

---

### Static files middleware

```js
app.use(express.static("public"));
```

Purpose:  
serve static files:

- CSS
    
- images
    
- JS
    

Example:

```
public/bootstrap.min.css
```

Accessible in HTML:

```
bootstrap.min.css
```

---

# ROUTES SECTION

This defines pages of your app.

---

# Route 1 — Home Page

```js
app.get("/", (req, res) => {
  res.render("home")
});
```

Meaning:  
When user visits:

```
localhost:3000/
```

Express renders:

```
views/home.ejs
```

This page shows:

- all contacts table
    
- Add button
    

---

# Route 2 — Show Contact

```js
app.get("/show-contact", (req, res) => {
  res.render("show-contact")
});
```

This page shows:

- single contact details
    

Currently static (hardcoded)

Later MongoDB data will come.

---

# Route 3 — Add Contact (GET)

```js
app.get("/add-contact", (req, res) => {
  res.render("add-contact")
});
```

Purpose:  
Show form page.

User fills form here.

---

# Route 4 — Add Contact (POST)

```js
app.post("/add-contact", (req, res) => {});
```

This will handle:

- form submission
    
- save to MongoDB
    

Currently empty.

---

# Route 5 — Update Contact

```js
app.get("/update-contact", (req, res) => {
  res.render("update-contact")
});
```

Shows update form.

Later:

- pre-filled with data
    
- update in DB
    

---

# Route 6 — Update POST

```js
app.post("/update-contact", (req, res) => {});
```

Will update database.

Currently empty.

---

# Route 7 — Delete Contact

```js
app.get("/delete-contact", (req, res) => {});
```

Will delete contact later.

---

# Server Start

```js
app.listen(PORT, () => {
  console.log("Server started at port 3000");
});
```

Runs server.

---

# 🧠 header.ejs (Layout Top)

This file contains:

- HTML head
    
- navbar
    
- CSS links
    

Used in every page.

---

Include header

```ejs
<%- include("partials/header.ejs") %>
```

This inserts header layout.

---

# Navbar

```html
<nav class="navbar navbar-expand-lg navbar-light">
```

Creates top navigation bar.

---

Logo

```html
<strong>Contact</strong> App
```

App title.

---

# 🧠 footer.ejs

Closes layout:

```html
</body>
</html>
```

Used in every page.

---

# 🧠 home.ejs (Main Contacts Page)

This page shows:

- All contacts table
    
- Add button
    
- Action buttons
    

---

Include header

```ejs
<%- include("partials/header.ejs") %>
```

---

Add contact button

```html
<a href="/add-contact" class="btn btn-success">
```

Navigates to add page.

---

Contacts table

```html
<table class="table">
```

Columns:

- id
    
- first name
    
- last name
    
- email
    
- phone
    
- actions
    

---

Action buttons

Show

```html
<a href="/show-contact">
```

Edit

```html
<a href="update-contact">
```

Delete

```html
<a href="#">
```

Currently static.

Later dynamic.

---

# 🧠 add-contact.ejs

This page contains:

Form for adding contact.

Fields:

- first_name
    
- last_name
    
- email
    
- phone
    
- address
    

---

Example input

```html
<input type="text" name="first_name">
```

Important:

`name="first_name"`

This becomes:

```
req.body.first_name
```

---

Submit button

```html
<button type="submit">
```

Should send to:

```
POST /add-contact
```

---

# 🧠 show-contact.ejs

This page shows:

Contact details.

Currently hardcoded:

```
Alfred
Kuhlman
```

Later dynamic:

```
<%= contact.first_name %>
```

---

Buttons

Edit

```
/update-contact
```

Delete

```
/delete-contact
```

Cancel

```
/
```

---

# 🧠 update-contact.ejs

Same as add form.

But used for editing.

Later:

fields will be pre-filled

Example:

```
value="<%= contact.first_name %>"
```

---

# App Flow (Important)

User clicks Add

```
/add-contact
```

User submits form

```
POST /add-contact
```

Server saves data

Redirect to home

```
/
```

User sees list.

---

# Navigation Flow

```
Home
 ├── Add Contact
 ├── Show Contact
 ├── Update Contact
 └── Delete Contact
```

---

# What You Learned in Part 01

Express routing  
EJS templates  
partials (header/footer)  
form UI  
navigation  
CRUD UI structure

---

# Next Part (MongoDB)

You will implement:

Create contact  
Read contacts  
Update contact  
Delete contact

Full CRUD.

---

# Important Beginner Concepts Used

res.render()  
GET routes  
POST routes  
EJS includes  
form inputs  
req.body (later)  
CRUD structure