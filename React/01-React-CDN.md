
---

# 📘 Chapter 1: React Basics Using CDN & `React.createElement`

_(Learning React from ZERO – No JSX, No Build Tools)_


```html
<!DOCTYPE html>

<html lang="en">

  

<head>

  <meta charset="UTF-8">

  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Document</title>

</head>

  

<body>

  

  <div id="root"></div>

  

  <!-- react cdn -->

  <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>

  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>

  
  

  <script>

  

    /*

      <div id="parent">

        <div id="child">

          <h1>I am h1 tag</h1>

        </div>

      </div>

    */

  

    // SYNTAX : React.createElement("tagName", attributeObject, children)

    const parent = React.createElement("div",{ id: "parent", style: { background: "salmon" } },React.createElement("div",{ id: "child" },React.createElement("h1",{},"I am h1 tag")));

    const root = ReactDOM.createRoot(document.getElementById("root"));

    root.render(parent);

  

  </script>

</body>

  

</html>
```

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

```js
<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
```

### Why two scripts?

|Library|Purpose|
|---|---|
|`react`|Core React logic (elements, components, hooks)|
|`react-dom`|Connects React to browser DOM|

👉 **React ≠ Browser**  
👉 **ReactDOM bridges React and Browser**

---

## 5️⃣ UI we want to build

```jss
<div id="parent">
  <div id="child">
    <h1>I am h1 tag</h1>
  </div>
</div>
```

❌ This is **not written directly in HTML**  
✅ This UI is created **using JavaScript + React**

---

## 6️⃣ `React.createElement()` (Core Concept)

### Syntax

```js
React.createElement(
  tagName,
  attributesObject,
  children
)
```

### Example

```js
React.createElement("h1", {}, "Hello")
```

Means:

```html
<h1>Hello</h1>
```

---

## 7️⃣ Building elements step-by-step

### Step 1: Create `<h1>`

```js
React.createElement("h1", {}, "I am h1 tag")
```

- `"h1"` → HTML tag
    
- `{}` → attributes (empty)
    
- `"I am h1 tag"` → text
    

---

### Step 2: Create child `<div>`

```js
React.createElement(
  "div",
  { id: "child" },
  React.createElement("h1", {}, "I am h1 tag")
)
```

Represents:

```html
<div id="child">
  <h1>I am h1 tag</h1>
</div>
```

---

### Step 3: Create parent `<div>`

```js
React.createElement(
  "div",
  { id: "parent", style: { border: "2px solid red" } },
  childDiv
)
```

Represents:

```js
<div id="parent" style="border:2px solid red">
  <div id="child">
    <h1>I am h1 tag</h1>
  </div>
</div>
```

---

## 8️⃣ Important Rule about `style` in React ⚠️

❌ Wrong (HTML style):

```js
style: "border: 2px solid red"
```

✅ Correct (React style):

```js
style: { border: "2px solid red" }
```

### Why?

- React expects **style as a JavaScript object**
    
- CSS properties are written in **camelCase**
    

Example:

```js
backgroundColor: "blue"
fontSize: "20px"
```

---

## 9️⃣ What is a React Element?

React Element is:

- A **plain JavaScript object**
    
- NOT actual HTML
    
- Lightweight & immutable
    

Example (simplified):

```js
{
  type: "div",
  props: {
    id: "parent",
    children: [...]
  }
}
```

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

```js
const root = ReactDOM.createRoot(
  document.getElementById("root")
);
```

### Purpose:

- Tells React where to render UI
    
- Enables **React 18 features**
    
- Required in modern React
    

---

## 1️⃣2️⃣ Rendering UI

```js
root.render(parent);
```

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

# 🔁 How React Elements Are Created (Inside → Out)

## Rule (Very Important)

> **React elements are always created from the innermost element to the outermost element.**

---

## 1️⃣ Look at your UI again

Target UI:

```html
<div id="parent">
  <div id="child">
    <h1>I am h1 tag</h1>
  </div>
</div>
```

### Visual hierarchy

```txt
parent
 └── child
      └── h1
```

---

## 2️⃣ Why creation must be inside → out

JavaScript works like this:

- A function must **finish executing**
    
- Before it can be passed as an argument to another function
    

So:

- `h1` must exist first
    
- Then `child div`
    
- Then `parent div`
    

---

## 3️⃣ Step-by-step creation order (REAL EXECUTION)

### Step 1: Create `<h1>` (innermost)

```js
const h1 = React.createElement(
  "h1",
  {},
  "I am h1 tag"
);
```

✅ First element created  
✅ No children inside it

---

### Step 2: Create `child` div (uses h1)

```js
const child = React.createElement(
  "div",
  { id: "child" },
  h1
);
```

➡️ `child` **depends on h1**

---

### Step 3: Create `parent` div (uses child)

```js
const parent = React.createElement(
  "div",
  { id: "parent" },
  child
);
```

➡️ `parent` **depends on child**

---

## 4️⃣ Same thing written inline (what you wrote)

```js
const parent = React.createElement(
  "div",
  { id: "parent" },
  React.createElement(
    "div",
    { id: "child" },
    React.createElement(
      "h1",
      {},
      "I am h1 tag"
    )
  )
);
```

### Execution order (internally)

1️⃣ `React.createElement("h1")`  
2️⃣ `React.createElement("div", {id:"child"})`  
3️⃣ `React.createElement("div", {id:"parent"})`

➡️ **Inside → Out**

---

## 5️⃣ Key beginner confusion cleared ❌

❌ React does NOT create parent first  
❌ React does NOT parse like HTML

✅ React follows **JavaScript execution rules**

---

## 6️⃣ Important Interview Question 💡

**Q: In which order are React elements created?**  
**A:** From **innermost child to outermost parent**, because children must exist before parent can reference them.

---

## 7️⃣ Relation with JSX (Very Important)

JSX:

```jsx
<div id="parent">
  <div id="child">
    <h1>I am h1 tag</h1>
  </div>
</div>
```

Behind the scenes:

```js
React.createElement(
  "div",
  { id: "parent" },
  React.createElement(
    "div",
    { id: "child" },
    React.createElement("h1", {}, "I am h1 tag")
  )
);
```

👉 JSX **looks outside → in**  
👉 React execution is **inside → out**

---

## 8️⃣ Add this line to your notes (IMPORTANT ⭐)

> **React elements are created from inside to outside (child → parent) because JavaScript evaluates function arguments first.**

---

## ✅ Final Summary

✔ React element creation follows JavaScript execution  
✔ Children must be created before parents  
✔ JSX hides this complexity  
✔ Understanding this makes React much easier later