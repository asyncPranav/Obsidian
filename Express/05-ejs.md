
---


# 1. What is EJS (Deep Explanation)

EJS = **Embedded JavaScript Templates**

It is a **server-side templating engine** that allows you to:

- write HTML
    
- embed JavaScript
    
- render dynamic content
    
- generate final HTML on server
    

Flow:

```
Client request
     ↓
Express route
     ↓
res.render("index", data)
     ↓
EJS compiles template
     ↓
HTML generated
     ↓
HTML sent to browser
```

So **EJS runs on server**, not in browser.

---

# 2. How EJS Works Internally

Example:

index.ejs

```html
<h1>Hello <%= name %></h1>
```

Express:

```js
res.render("index", {name:"John"})
```

EJS converts template into JS function:

```js
function(){
 return "<h1>Hello " + name + "</h1>"
}
```

Then executes and returns HTML.

---

# 3. Setting Up EJS Properly

Install:

```bash
npm install ejs
```

Express config:

```js
const express = require("express")
const app = express()

app.set("view engine", "ejs")
app.set("views", "./views")
```

Important:

- `view engine` → template engine
    
- `views` → folder location
    

---

# 4. res.render() Deep Explanation

Syntax:

```js
res.render(view, data, callback)
```

Example:

```js
res.render("index", {name:"john"})
```

This means:

- find views/index.ejs
    
- pass data
    
- render HTML
    

---

# 5. EJS Tag Types (Very Important)

These define how EJS behaves.

---

## 5.1 `<%= %>` (Output Escaped)

Prints value and **escapes HTML**

```html
<%= name %>
```

If:

```
name = "<h1>hello</h1>"
```

Output:

```
&lt;h1&gt;hello&lt;/h1&gt;
```

Safe from XSS.

---

## 5.2 `<%- %>` (Unescaped Output)

Prints raw HTML.

```html
<%- html %>
```

If:

```
html = "<h1>Hello</h1>"
```

Output:

```html
<h1>Hello</h1>
```

Used for:

- partials
    
- dynamic HTML
    

---

## 5.3 `<% %>` (Logic Only)

Runs JavaScript but **does not output**

```html
<% if(user){ %>
   <h1>Logged in</h1>
<% } %>
```

---

## 5.4 `<%# %>` Comment

```html
<%# this is comment %>
```

Not rendered.

---

## 5.5 `<%% %>` Literal

Print `<%` literally

```html
<%% hello %>
```

Output:

```
<% hello %>
```

---

# 6. Passing Data to EJS

Express:

```js
app.get('/', (req,res)=>{

res.render("index",{
 name:"john",
 age:20
})

})
```

EJS:

```html
<h1><%= name %></h1>
<p><%= age %></p>
```

---

# 7. Locals in EJS

All passed variables become **locals**

```js
res.render("index",{
 name:"john"
})
```

Inside EJS:

```
name available directly
```

Also accessible as:

```html
<%= locals.name %>
```

---

# 8. Control Flow in EJS

## If Condition

```html
<% if(age > 18){ %>
<h1>Adult</h1>
<% } else { %>
<h1>Minor</h1>
<% } %>
```

---

## Switch

```html
<% switch(role){ %>

<% case "admin": %>
Admin
<% break; %>

<% case "user": %>
User
<% break; %>

<% } %>
```

---

# 9. Loops in EJS

## forEach loop

```html
<% users.forEach(user=>{ %>
<li><%= user %></li>
<% }) %>
```

---

## for loop

```html
<% for(let i=0;i<5;i++){ %>

<p><%= i %></p>

<% } %>
```

---

## map loop

```html
<% users.map(user=>{ %>

<p><%= user %></p>

<% }) %>
```

---

# 10. Objects in EJS

```js
res.render("index",{
 user:{
  name:"john",
  age:20
 }
})
```

EJS:

```html
<%= user.name %>
<%= user.age %>
```

---

# 11. Array of Objects

```js
const users = [
 {name:"john",age:20},
 {name:"sam",age:22}
]
```

EJS:

```html
<% users.forEach(u=>{ %>

<p><%= u.name %> - <%= u.age %></p>

<% }) %>
```

---

# 12. Includes (Partials) — Very Important

Used to reuse UI components.

Example:

views/partials/header.ejs

```html
<header>Header</header>
```

Use:

```html
<%- include("partials/header") %>
```

Important:  
Use `<%-` not `<%=` for include.

---

# 13. Passing Data to Include

```html
<%- include("partials/header",{title:"Home"}) %>
```

header.ejs

```html
<h1><%= title %></h1>
```

---

# 14. Layout Pattern (Manual Layout)

layout.ejs

```html
<html>
<body>

<%- body %>

</body>
</html>
```

index.ejs

```html
<%- include("layout",{body:`
<h1>Home</h1>
`}) %>
```

---

# 15. Nested Includes

You can include inside include.

```
layout
  └ header
  └ footer
```

layout.ejs

```html
<%- include("header") %>

<%- body %>

<%- include("footer") %>
```

---

# 16. Using JavaScript in EJS

```html
<% let total = price * qty %>

<p>Total: <%= total %></p>
```

---

# 17. Ternary Operator

```html
<%= age > 18 ? "Adult" : "Minor" %>
```

---

# 18. Escaping vs Unescaped

Escaped:

```html
<%= html %>
```

Unescaped:

```html
<%- html %>
```

Important for **security**

---

# 19. Partials Folder Structure

```
views
 ├ partials
 │   ├ header.ejs
 │   ├ footer.ejs
 │
 ├ index.ejs
```

---

# 20. Rendering Multiple Pages

```js
app.get('/',(req,res)=>{
 res.render("home")
})

app.get('/about',(req,res)=>{
 res.render("about")
})
```

---

# 21. EJS with Forms

```html
<form action="/user" method="POST">

<input name="name">
<button>Submit</button>

</form>
```

---

# 22. Using Data from Route

```js
app.get('/user',(req,res)=>{

res.render("index",{
 name:"john"
})

})
```

---

# 23. Global Variables

Middleware:

```js
app.use((req,res,next)=>{

res.locals.siteName = "My App"

next()

})
```

EJS:

```html
<%= siteName %>
```

Available everywhere.

---

# 24. Conditional Rendering

```html
<% if(user){ %>

Welcome <%= user %>

<% } %>
```

---

# 25. Complete Example

app.js

```js
app.get('/',(req,res)=>{

const users = [
 {name:"john",age:20},
 {name:"sam",age:25}
]

res.render("index",{users})

})
```

index.ejs

```html
<h1>Users</h1>

<ul>

<% users.forEach(u=>{ %>

<li>
<%= u.name %> - <%= u.age %>
</li>

<% }) %>

</ul>
```