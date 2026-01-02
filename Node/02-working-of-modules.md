
---

# 🧠 HOW NODE.JS MODULES WORK (FROM ABSOLUTE SCRATCH)

---

## 0️⃣ First: Imagine Node.js Without Modules

Suppose you write code like this 👇

```js
function add(a, b) {
  return a + b;
}

function subtract(a, b) {
  return a - b;
}

console.log(add(5, 3));
```

Now imagine:

- 100 functions
    
- 1000 lines
    
- Everything in **one file**
    

❌ Problems:

- Very hard to read
    
- Cannot reuse code
    
- One bug affects everything
    

👉 **Solution: split code into files (modules)**

---

## 1️⃣ What is a Module? (Very Simple Meaning)

> 📦 **A module is just a file**

In Node.js:

- Every `.js` file = **one module**
    
- Every module has **its own private space**
    

Example:

```js
math.js  → module
app.js   → module
```

They **cannot see each other’s variables directly**.

---

## 2️⃣ The BIG SECRET: Node Wraps Every File in a Function

You write this 👇

```js
// math.js
const secret = 123;

function add(a, b) {
  return a + b;
}
```

But Node.js actually runs this 👇 (you never see it):

```js
(function (exports, require, module, __filename, __dirname) {
  const secret = 123;

  function add(a, b) {
    return a + b;
  }
});
```

⚠️ **THIS IS THE MOST IMPORTANT CONCEPT**

### Why does Node do this?

- To give each file its **own scope**
    
- To prevent variable collision
    
- To control what is shared
    

---

## 3️⃣ What Are These Hidden Things?

Every module automatically gets:

### 🔹 `module`

An object representing the current file

```js
module = {
  exports: {}
}
```

---

### 🔹 `module.exports`

This is the **ONLY thing** that gets sent outside the file

Think of it as:

> 📤 “What this file gives to others”

---

### 🔹 `exports`

Just a shortcut to `module.exports`

`exports === module.exports  // true (initially)`

---

### 🔹 `require`

A function to **load another module**

---

### 🔹 `__filename`

Full path of current file

---

### 🔹 `__dirname`

Folder path of current file

---

## 4️⃣ module.exports Starts Empty (VERY IMPORTANT)

At the start of every file:

`module.exports = {}; exports = module.exports;`

So initially:

`{}`

Nothing is exported yet.

---

## 5️⃣ How Exporting ACTUALLY Works

### Example 1: Exporting ONE thing

`// greet.js module.exports = function greet(name) {   return "Hello " + name; };`

What happened internally?

`module.exports = [Function]`

So when someone does:

`const greet = require('./greet');`

They receive:

`greet → function`

❌ This will FAIL:

`const { greet } = require('./greet');`

Why?

- You are destructuring
    
- But export is NOT an object
    
- It’s a function
    

---

### Example 2: Exporting MULTIPLE things

`// math.js function add(a, b) {   return a + b; }  function subtract(a, b) {   return a - b; }  module.exports = { add, subtract };`

Internally:

`module.exports = {   add: [Function],   subtract: [Function] };`

Now importing:

`const math = require('./math'); math.add(5, 3);`

OR using destructuring ✅

`const { add, subtract } = require('./math');`

✔ Works because export is an **object**

---

## 6️⃣ Why `module.exports.x = x` Exists

Because `module.exports` starts as `{}`

`// math.js module.exports.add = function (a, b) {   return a + b; };  module.exports.subtract = function (a, b) {   return a - b; };`

This is SAME AS:

`module.exports = {   add,   subtract };`

### Why people use this style?

- When adding exports gradually
    
- When writing large files
    
- When conditionally exporting
    

---

## 7️⃣ exports vs module.exports (Common Confusion)

### ✅ Correct

`exports.add = add; exports.subtract = subtract;`

Why?

`exports → points to module.exports`

---

### ❌ Wrong

`exports = add;`

Why?

- You changed the reference
    
- Node only returns `module.exports`
    

📌 **Golden Rule**

> Always assign final value to `module.exports`

---

## 8️⃣ How `require()` Works Internally (STEP BY STEP)

When Node sees:

`const math = require('./math');`

Node does:

### Step 1: Resolve path

Looks for:

1. `math.js`
    
2. `math/index.js`
    

---

### Step 2: Check cache

If already loaded:

`return cachedExports;`

👉 Module runs **only once**

---

### Step 3: Read file

Reads JS code as text

---

### Step 4: Wrap in function

`(function (exports, require, module) {   // file code });`

---

### Step 5: Execute code

Runs everything line by line

---

### Step 6: Return exports

`return module.exports;`

---

## 9️⃣ Module Caching (Very Important)

`const a = require('./math'); const b = require('./math');  console.log(a === b); // true`

Why?

- Node stores result in cache
    
- Same object reused
    

💡 This is why:

- Global state is shared
    
- DB connections don’t reopen
    

---

## 🔟 Folder as a Module

Structure:

`utils/  ├─ add.js  ├─ subtract.js  └─ index.js`

### index.js

`const add = require('./add'); const subtract = require('./subtract');  module.exports = { add, subtract };`

### app.js

`const utils = require('./utils'); utils.add(5, 3);`

📌 Node automatically loads `index.js`

---

## 1️⃣1️⃣ JSON Modules (Behind the Scenes)

`const data = require('./data.json');`

Internally Node does:

`const file = readFile(); const parsed = JSON.parse(file); module.exports = parsed;`

So JSON is just **converted to object**.

---

## 1️⃣2️⃣ ES Modules (Quick Internal Difference)

### CommonJS

- `require()` → sync
    
- exports → copied value
    

### ES Modules

- `import` → async
    
- exports → live binding
    
- strict mode always
    

---

## 1️⃣3️⃣ BEST MENTAL MODEL (REMEMBER THIS)

> 🧠 **Every file is a function**
> 
> - `module.exports` → return value
>     
> - `require()` → function call
>     

Example:

`const math = require('./math');`

Same as thinking:

`const math = runMathModule();`

---

## ✅ FINAL BEGINNER SUMMARY

- Every file is a module
    
- Node wraps each file in a function
    
- `module.exports` is what leaves the file
    
- `require()` runs file once and caches it
    
- Destructuring works only if export is an object
    
- Folder import works via `index.js`