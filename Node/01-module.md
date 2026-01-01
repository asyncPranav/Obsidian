
---

## **1. What is a Module in Node.js?**

**Theory:**

- A **module** is basically a reusable piece of code in Node.js.
    
- Every file in Node.js is considered a **module**.
    
- Modules allow us to **organize code**, **re-use code**, and **separate concerns**.
    

**Types of modules:**

1. **Built-in modules** → Provided by Node.js (`fs`, `http`, `path`, etc.)
    
2. **User-defined modules** → You create them (`math.js`, `helper.js`)
    
3. **Third-party modules** → Installed via npm (`express`, `axios`)
    

**Example:**

```js
// math.js
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

// export functions
module.exports = { add, subtract };
```

`// app.js const math = require('./math');  // importing user-defined module  console.log(math.add(5, 3));     // 8 console.log(math.subtract(5, 3));// 2`

---

## **2. Module System in Node.js**

Node.js has **two main module systems**:

1. **CommonJS (CJS)** → Default in Node.js
    
    - Uses `require` to import
        
    - Uses `module.exports` or `exports` to export
        
2. **ES Modules (ESM)** → Modern JS (like in browsers)
    
    - Uses `import` to import
        
    - Uses `export` to export
        
    - Works with `.mjs` files or `"type": "module"` in `package.json`
        

---

## **3. CommonJS Module (CJS)**

### **Exporting**

**Theory:**

- `module.exports` exports a single object, function, or variable.
    
- `exports` is shorthand for `module.exports` (but cannot replace `module.exports` completely).
    

**Examples:**

**a) Exporting multiple things:**

``// greetings.js function sayHello(name) {     return `Hello ${name}`; }  function sayBye(name) {     return `Bye ${name}`; }  module.exports = { sayHello, sayBye };``

**b) Exporting a single thing:**

``// logger.js module.exports = function(message) {     console.log(`Log: ${message}`); };``

### **Importing**

`// app.js const greetings = require('./greetings');  console.log(greetings.sayHello('Alice')); // Hello Alice console.log(greetings.sayBye('Bob'));     // Bye Bob  const log = require('./logger'); log('Server started'); // Log: Server started`

---

## **4. ES Modules (ESM)**

### **Exporting**

**Theory:**

- Can export **named exports** or a **default export**.
    

**a) Named exports**

`// utils.mjs export function multiply(a, b) {     return a * b; }  export function divide(a, b) {     return a / b; }`

**b) Default export**

``// greet.mjs export default function(name) {     return `Hi ${name}`; }``

### **Importing**

`// app.mjs import { multiply, divide } from './utils.mjs'; console.log(multiply(5, 2)); // 10 console.log(divide(10, 2));  // 5  import greet from './greet.mjs'; console.log(greet('Alice')); // Hi Alice`

**Important:**

- To use ESM, either:
    
    - Name file `.mjs` **OR**
        
    - Add `"type": "module"` in `package.json`.
        

---

## **5. Difference Between CommonJS & ESM**

|Feature|CommonJS|ES Modules (ESM)|
|---|---|---|
|File extension|`.js`|`.mjs` or `"type":"module"`|
|Import|`require('./file')`|`import ... from './file'`|
|Export|`module.exports` / `exports`|`export` / `export default`|
|Execution|Synchronous|Asynchronous capable|

---

## **6. Practical Example**

**Scenario:** We have a **calculator module** and want to use it in an app.

**calculator.js (CJS):**

`function add(a, b) { return a + b; } function subtract(a, b) { return a - b; } function multiply(a, b) { return a * b; } function divide(a, b) { return a / b; }  module.exports = { add, subtract, multiply, divide };`

**app.js:**

`const calc = require('./calculator');  console.log(calc.add(10, 5));      // 15 console.log(calc.subtract(10, 5)); // 5 console.log(calc.multiply(10, 5)); // 50 console.log(calc.divide(10, 5));   // 2`

---

## **7. Tips for Import/Export in Node.js**

1. **Always use relative path** for local files (`./file` or `../file`).
    
2. **Node built-in modules** don’t need `./` (`fs`, `http`).
    
3. **Mixing CJS and ESM** requires care. Usually:
    
    - ESM files can import CJS using `import pkg from './file.cjs'`
        
    - CJS cannot directly import ESM without dynamic import.
        
4. **Avoid overwriting `exports` directly**. Use `module.exports` if exporting a single item.
    

---

✅ **Summary:**

- **CommonJS:** `require` + `module.exports` → Default Node.js module system.
    
- **ESM:** `import` + `export` → Modern JS standard.
    
- **Modules** help keep code organized, reusable, and modular.