
---

Here are **very detailed beginner-friendly notes for EJS (Embedded JavaScript)** used with Express.

I’ll cover:

- what is EJS
    
- why use EJS
    
- setup
    
- syntax
    
- tags
    
- variables
    
- loops
    
- conditionals
    
- partials
    
- layouts
    
- includes
    
- passing data
    
- best practices
    
- examples
    

---

# 1. What is EJS

EJS = **Embedded JavaScript Templates**

It lets you **write JavaScript inside HTML**.

So instead of static HTML:

```html
<h1>Hello</h1>
```

You can write dynamic HTML:

```html
<h1>Hello <%= name %></h1>
```

Server inserts data → then sends HTML to browser.

---

# 2. Why use EJS

Without EJS:

You must send static HTML.

With EJS you can:

- show dynamic data
    
- loop arrays
    
- conditionally render UI
    
- reuse components
    
- build templates
    

---

# 3. Install EJS

```bash
npm install ejs
```

---

# 4. Setup EJS in Express

```js
const express = require('express')
const app = express()

app.set('view engine','ejs')

app.listen(3000)
```

Now Express automatically looks inside:

```
views/
```

folder.

---

# Folder structure

```
project
 ├── app.js
 └── views
      └── index.ejs
```

---

# 5. Render EJS File

app.js

```js
app.get('/',(req,res)=>{
    res.render("index")
})
```

views/index.ejs

```html
<h1>Hello EJS</h1>
```

---

# 6. Passing Data to EJS

```js
app.get('/',(req,res)=>{
    res.render("index",{
        name:"Pranav"
    })
})
```

EJS file:

```html
<h1>Hello <%= name %></h1>
```

Output:

```
Hello Pranav
```

---

# 7. EJS Tags

These are **very important**

### `<%= %>` output value

```html
<h1><%= name %></h1>
```

Prints value.

---

### `<%- %>` unescaped HTML

```html
<%- "<h1>Hello</h1>" %>
```

Renders HTML.

---

### `<% %>` JavaScript logic

```html
<% let x = 10 %>
```

No output.

---

### `<%# %>` comment

```html
<%# this is comment %>
```

---

### `<%% %>` print literal

```html
<%% prints <% %>
```

---

# 8. Variables Example

```js
app.get('/',(req,res)=>{
    res.render("index",{
        name:"John",
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

# 9. If Condition in EJS

```html
<% if(age >= 18){ %>
    <h1>Adult</h1>
<% } else { %>
    <h1>Minor</h1>
<% } %>
```

---

# 10. Loop in EJS

Example array:

```js
app.get('/',(req,res)=>{
    res.render("index",{
        users:["john","sam","alex"]
    })
})
```

EJS:

```html
<ul>
<% users.forEach(user => { %>

    <li><%= user %></li>

<% }) %>
</ul>
```

Output:

```
john
sam
alex
```

---

# 11. Object Example

```js
app.get('/',(req,res)=>{
    res.render("index",{
        user:{
            name:"john",
            age:20
        }
    })
})
```

EJS:

```html
<h1><%= user.name %></h1>
<p><%= user.age %></p>
```

---

# 12. Include Partials (Very Important)

Used to reuse components like:

- navbar
    
- footer
    
- header
    

---

Create:

```
views/partials/header.ejs
```

header.ejs

```html
<h1>Header</h1>
```

Use in index.ejs

```html
<%- include('partials/header') %>
```

---

# 13. Layout Example

header.ejs

```html
<header>My Header</header>
```

footer.ejs

```html
<footer>My Footer</footer>
```

index.ejs

```html
<%- include('partials/header') %>

<h1>Home Page</h1>

<%- include('partials/footer') %>
```

---

# 14. Passing Data to Include

```html
<%- include('partials/header',{title:"Home"}) %>
```

header.ejs

```html
<h1><%= title %></h1>
```

---

# 15. Using JS inside EJS

```html
<% let total = 10 + 20 %>

<p><%= total %></p>
```

---

# 16. Ternary Operator

```html
<h1>
<%= age > 18 ? "Adult" : "Minor" %>
</h1>
```

---

# 17. Loop with index

```html
<% users.forEach((user,index)=>{ %>

<p><%= index %> - <%= user %></p>

<% }) %>
```

---

# 18. Form Example

```html
<form action="/user" method="POST">

<input name="name">
<button>Submit</button>

</form>
```

---

# 19. Rendering Multiple Pages

app.js

```js
app.get('/',(req,res)=>{
    res.render("home")
})

app.get('/about',(req,res)=>{
    res.render("about")
})
```

---

# 20. Real Example

app.js

```js
app.get('/users',(req,res)=>{

const users = [
 {name:"john",age:20},
 {name:"sam",age:25}
]

res.render("users",{users})

})
```

users.ejs

```html
<h1>Users</h1>

<ul>

<% users.forEach(user=>{ %>

<li>
<%= user.name %> - <%= user.age %>
</li>

<% }) %>

</ul>
```

---

# 21. Important EJS Rules

1. `<%= %>` prints value
    
2. `<% %>` JS logic
    
3. `<%- %>` render HTML
    
4. include for partials
    
5. res.render() to render
    

---

# 22. EJS vs Static HTML

Static HTML:

```html
<h1>Hello</h1>
```

EJS:

```html
<h1>Hello <%= name %></h1>
```

Dynamic.

---

# 23. Most Important for Exams

Remember:

res.render()  
<%= %>  
<% %>  
<%- include() %>  
loops  
if condition  
passing data

---

# 24. Complete Example

app.js

```js
const express = require('express')
const app = express()

app.set("view engine","ejs")

app.get('/',(req,res)=>{

const users = ["john","sam","alex"]

res.render("index",{users})

})

app.listen(3000)
```

index.ejs

```html
<h1>Users</h1>

<ul>

<% users.forEach(user=>{ %>

<li><%= user %></li>

<% }) %>

</ul>
```