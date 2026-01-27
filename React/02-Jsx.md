
# 📘 Chapter 2: JSX – Writing React UI in a Simple & Readable Way

---

## 1️⃣ What is JSX?

### Definition (Very Simple)

**JSX stands for JavaScript XML**.

JSX allows us to **write HTML-like code inside JavaScript**.

Example:

```jsx
<h1>Hello World</h1>
```

⚠️ Important:

> **JSX is NOT HTML**  
> **JSX is NOT a new language**  
> JSX is just **syntax sugar** for `React.createElement()`

---

## 2️⃣ Why JSX was introduced?

In Chapter 1, we wrote this:

```js
React.createElement(
  "div",
  {},
  React.createElement(
    "h1",
    {},
    "Hello"
  )
);
```

### Problems ❌

- Hard to read
    
- Hard to write
    
- Deep nesting becomes confusing
    
- Not developer-friendly
    

### JSX Solution ✅

```jsx
<div>
  <h1>Hello</h1>
</div>
```

➡️ Same result, **much cleaner**

---

## 3️⃣ JSX is converted into `React.createElement`

### JSX code:

`<h1>Hello</h1>`

### Behind the scenes:

`React.createElement("h1", {}, "Hello");`

📌 **JSX does NOT run in browser directly**  
It is **converted (transpiled)** into JavaScript.

---

## 4️⃣ How JSX works (Behind the scenes)

JSX is converted using:

- **Babel** (in real projects)
    
- Or React internally (CDN examples)
    

### Flow:

`JSX  ↓ React.createElement()  ↓ React Element Object  ↓ Virtual DOM  ↓ Real DOM`

---

## 5️⃣ JSX Syntax Rules (VERY IMPORTANT)

### Rule 1️⃣: One Parent Element Required

❌ Wrong:

`<h1>Hello</h1> <h2>World</h2>`

✅ Correct:

`<div>   <h1>Hello</h1>   <h2>World</h2> </div>`

📌 Reason:

> JSX must return **one single parent element**

---

### Rule 2️⃣: Use `className` instead of `class`

❌ Wrong:

`<div class="box"></div>`

✅ Correct:

`<div className="box"></div>`

📌 Why?

- `class` is a **reserved keyword** in JavaScript
    

---

### Rule 3️⃣: Style must be an object

❌ Wrong:

`<div style="color:red"></div>`

✅ Correct:

`<div style={{ color: "red" }}></div>`

### Explanation:

- Outer `{}` → JavaScript expression
    
- Inner `{}` → style object
    

---

### Rule 4️⃣: JSX expressions use `{}`

You can use JavaScript inside JSX using `{}`.

Example:

`const name = "Rahul";  <h1>Hello {name}</h1>`

Output:

`<h1>Hello Rahul</h1>`

---

## 6️⃣ JSX with Numbers & Expressions

`const a = 10; const b = 20;  <h1>Sum: {a + b}</h1>`

✔ Expressions allowed  
❌ Statements not allowed

---

## 7️⃣ What is NOT allowed in JSX ❌

❌ if-else statements

`{ if (x > 10) {} }`

❌ for loops

`{ for (let i=0; i<5; i++) {} }`

✔ Only **expressions**, not statements

---

## 8️⃣ JSX with Conditional Rendering (Basic idea)

### Using ternary operator:

`{isLoggedIn ? <h1>Welcome</h1> : <h1>Please Login</h1>}`

📌 Ternary is allowed because it is an **expression**

---

## 9️⃣ JSX must be closed properly

❌ Wrong:

`<img src="img.png">`

✅ Correct:

`<img src="img.png" />`

📌 All JSX tags must be **closed**

---

## 🔟 JSX is JavaScript (VERY IMPORTANT)

This is valid JSX:

`const element = <h1>Hello</h1>;`

JSX becomes:

`const element = React.createElement("h1", {}, "Hello");`

➡️ JSX produces **React Elements**

---

## 1️⃣1️⃣ JSX Execution Order (Revision from Chapter 1)

Even in JSX:

- Elements are created **inside → out**
    
- Children first, parent last
    

JSX hides this complexity but logic remains same.

---

## 1️⃣2️⃣ JSX with ReactDOM.render / root.render

`const root = ReactDOM.createRoot(   document.getElementById("root") );  root.render(   <h1>Hello JSX</h1> );`

➡️ JSX passed directly to `render()`

---

## 1️⃣3️⃣ Common Beginner Mistakes ⚠️

- Forgetting parent element
    
- Using `class` instead of `className`
    
- Using string styles
    
- Forgetting to close tags
    
- Writing statements inside `{}`
    

---

## 1️⃣4️⃣ Interview Important Points ⭐

- JSX is optional
    
- JSX is converted to `React.createElement`
    
- JSX improves readability
    
- JSX follows JavaScript rules
    
- One root element required
    

---

## ✅ Chapter 2 Summary

✔ JSX makes React readable  
✔ JSX is not HTML  
✔ JSX converts to `React.createElement`  
✔ Use `{}` to embed JavaScript  
✔ JSX follows strict syntax rules