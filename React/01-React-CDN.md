
---

# 📘 Chapter 1: React Basics Using CDN & `React.createElement`

_(Learning React from ZERO – No JSX, No Build Tools)_

---

## 1️⃣ What is React?

### Definition (Simple)

**React is a JavaScript library used to build user interfaces (UI)**.

- Created by **Facebook**
    
- Used to build **Single Page Applications (SPA)**
    
- UI updates automatically when data changes
    

### Why React exists?

In normal JavaScript:

- DOM manipulation is **slow**
    
- Code becomes **complex** as app grows
    

React solves this by:

- Using **Virtual DOM**
    
- Updating **only what changes**
    
- Making UI **component-based**
    

---

## 2️⃣ How React works in browser (Basic Idea)

React works in **3 steps**:

1. You create **React elements**
    
2. React builds a **Virtual DOM**
    
3. React updates the **Real DOM efficiently**
    

---

## 3️⃣ Why we need a `root` div

```js
<div id="root"></div>
```

### Explanation:

- React does **not control the full HTML page**
    
- React needs **one container**
    
- Everything React shows is rendered **inside root**
    

📌 Rule:

> React app always mounts on **one root element**

---

## 4️⃣ Including React using CDN

`<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script> <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>`

### Why two scripts?

|Library|Purpose|
|---|---|
|`react`|Core React logic (elements, components, hooks)|
|`react-dom`|Connects React to browser DOM|

👉 **React ≠ Browser**  
👉 **ReactDOM bridges React and Browser**

---

## 5️⃣ UI we want to build

`<div id="parent">   <div id="child">     <h1>I am h1 tag</h1>   </div> </div>`

❌ This is **not written directly in HTML**  
✅ This UI is created **using JavaScript + React**

---

## 6️⃣ `React.createElement()` (Core Concept)

### Syntax

`React.createElement(   tagName,   attributesObject,   children )`

### Example

`React.createElement("h1", {}, "Hello")`

Means:

`<h1>Hello</h1>`

---

## 7️⃣ Building elements step-by-step

### Step 1: Create `<h1>`

`React.createElement("h1", {}, "I am h1 tag")`

- `"h1"` → HTML tag
    
- `{}` → attributes (empty)
    
- `"I am h1 tag"` → text
    

---

### Step 2: Create child `<div>`

`React.createElement(   "div",   { id: "child" },   React.createElement("h1", {}, "I am h1 tag") )`

Represents:

`<div id="child">   <h1>I am h1 tag</h1> </div>`

---

### Step 3: Create parent `<div>`

`React.createElement(   "div",   { id: "parent", style: { border: "2px solid red" } },   childDiv )`

Represents:

`<div id="parent" style="border:2px solid red">   <div id="child">     <h1>I am h1 tag</h1>   </div> </div>`

---

## 8️⃣ Important Rule about `style` in React ⚠️

❌ Wrong (HTML style):

`style: "border: 2px solid red"`

✅ Correct (React style):

`style: { border: "2px solid red" }`

### Why?

- React expects **style as a JavaScript object**
    
- CSS properties are written in **camelCase**
    

Example:

`backgroundColor: "blue" fontSize: "20px"`

---

## 9️⃣ What is a React Element?

React Element is:

- A **plain JavaScript object**
    
- NOT actual HTML
    
- Lightweight & immutable
    

Example (simplified):

`{   type: "div",   props: {     id: "parent",     children: [...]   } }`

📌 React compares these objects using **Virtual DOM**

---

## 🔟 Virtual DOM (Beginner Explanation)

- Virtual DOM is a **copy of real DOM**
    
- Stored in **memory**
    
- Faster to update
    

### Flow:

1. State changes
    
2. New Virtual DOM created
    
3. React compares old vs new (Diffing)
    
4. Only changed nodes update in real DOM
    

---

## 1️⃣1️⃣ `ReactDOM.createRoot()`

`const root = ReactDOM.createRoot(   document.getElementById("root") );`

### Purpose:

- Tells React where to render UI
    
- Enables **React 18 features**
    
- Required in modern React
    

---

## 1️⃣2️⃣ Rendering UI

`root.render(parent);`

Meaning:  
👉 Convert React element  
👉 Into real browser DOM  
👉 Insert inside `#root`

---

## 1️⃣3️⃣ Why this method is not used normally?

Problems:

- Too much nesting
    
- Hard to read
    
- Hard to maintain
    

Solution:  
➡️ **JSX**

JSX is:

- HTML-like syntax
    
- Converted internally to `React.createElement`
    

---

## 1️⃣4️⃣ Interview Important Points ⭐

- React does NOT manipulate DOM directly
    
- `React.createElement` returns JS object
    
- JSX is optional
    
- React is declarative
    
- One root element per app
    

---

## ✅ Chapter Summary

✔ React uses Virtual DOM  
✔ UI is created using `React.createElement`  
✔ ReactDOM connects React to browser  
✔ JSX is just a wrapper over this logic  
✔ CDN React is best for learning internals