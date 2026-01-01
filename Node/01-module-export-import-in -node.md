
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

```js
// app.js
const math = require('./math');  // importing user-defined module

console.log(math.add(5, 3));     // 8
console.log(math.subtract(5, 3));// 2
```

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
	
-  `module.exports` **starts as an empty object `{}`** or is an empty object by default.
	
```js
console.log(module.exports) // {}
```
    
- You can **add properties gradually** using `module.exports.x = x`.

**Examples:**

**a) Exporting multiple things:**

```js
// greetings.js
function sayHello(name) {
    return `Hello ${name}`;
}

function sayBye(name) {
    return `Bye ${name}`;
}

module.exports = { sayHello, sayBye };
```

**b) Exporting a single thing:**

```js
// logger.js
module.exports = function(message) {
    console.log(`Log: ${message}`);
};
```

**c) Adding properties instead of overwriting `module.exports`**

```js
// math.js
function calculateSum(a, b) { return a + b; }
function calculateDiff(a, b) { return a - b; }

module.exports.calculateSum = calculateSum;
module.exports.calculateDiff = calculateDiff;

// or shorthand
exports.calculateSum = calculateSum;
exports.calculateDiff = calculateDiff;
```

### **Importing**

```js
// app.js
const greetings = require('./greetings');

console.log(greetings.sayHello('Alice')); // Hello Alice
console.log(greetings.sayBye('Bob'));     // Bye Bob

const log = require('./logger');
log('Server started'); // Log: Server started
```

#NOTE : If we export 

###  **`module.exports` vs `exports`**

|Feature|`module.exports`|`exports` (shorthand)|
|---|---|---|
|Actual object used|Yes|No, points to `module.exports`|
|Overwriting allowed|Yes (`module.exports = ...`)|No (`exports = ...` breaks link)|
|Adding properties|Yes (`module.exports.x = x`)|Yes (`exports.x = x`)|

**Rule:** Always consider `module.exports` as the **true export object**.

---

## **4. ES Modules (ESM)**

### **Exporting**

**Theory:**

- Can export **named exports** or a **default export**.
    

**a) Named exports**

```js
// utils.mjs
export function multiply(a, b) {
    return a * b;
}

export function divide(a, b) {
    return a / b;
}
```

**b) Default export**

```js
// greet.mjs
export default function(name) {
    return `Hi ${name}`;
}
```

### **Importing**

```js
// app.mjs
import { multiply, divide } from './utils.mjs';
console.log(multiply(5, 2)); // 10
console.log(divide(10, 2));  // 5

import greet from './greet.mjs';
console.log(greet('Alice')); // Hi Alice
```

**Important:**

- To use ESM, either:
    - Name file `.mjs` **OR**
    - Add `"type": "module"` in `package.json`.
        

---

## **5. Difference Between CommonJS & ESM**

| Feature        | CommonJS                     | ES Modules (ESM)            |
| -------------- | ---------------------------- | --------------------------- |
| File extension | `.js`                        | `.mjs` or `"type":"module"` |
| Import         | `require('./file')`          | `import ... from './file'`  |
| Export         | `module.exports` / `exports` | `export` / `export default` |
| Execution      | Synchronous                  | Asynchronous capable        |
| Code run in    | Non strict mode              | Strict mode                 |

---

## **6. Practical Example**

**Scenario:** We have a **calculator module** and want to use it in an app.

**calculator.js (CJS):**

```js
function add(a, b) { return a + b; }
function subtract(a, b) { return a - b; }
function multiply(a, b) { return a * b; }
function divide(a, b) { return a / b; }

module.exports = { add, subtract, multiply, divide };
```

**app.js:**

```js
const calc = require('./calculator');

console.log(calc.add(10, 5));      // 15
console.log(calc.subtract(10, 5)); // 5
console.log(calc.multiply(10, 5)); // 50
console.log(calc.divide(10, 5));   // 2
```

---

## **7. Tips for Import/Export in Node.js**

1. **Always use relative path** for local files (`./file` or `../file`).
    
2. **Node built-in modules** don’t need `./` (`fs`, `http`).
    
3. **Mixing CJS and ESM** requires care. Usually:
    
    - ESM files can import CJS using `import pkg from './file.cjs'`
        
    - CJS cannot directly import ESM without dynamic import.
        
4. **Avoid overwriting `exports` directly**. Use `module.exports` if exporting a single item.
	
5.  **Default export** → one main thing, imported without `{}`
    
6. **Named export** → multiple items, imported with `{}`
    
7. **Gradual exports** in CJS → `module.exports.x = x` or `exports.x = x`

---

## **1. Single Export in ESM**

**Theory:**

- Single export usually means you are exporting **one main thing** from a module.
    
- Use **`export default`** for this.
    

**Syntax:**

```js
export default value;   // can be function, object, class, variable
```

**Example 1 – Single function export**

```js
// greet.mjs
export default function greet(name) {
    return `Hello ${name}`;
}
```

**Importing it:**

```js
// app.mjs
import greet from './greet.mjs';
console.log(greet('Alice')); // Hello Alice
```

**Example 2 – Single object export**

```js
// config.mjs
const config = { port: 3000, host: 'localhost' };
export default config;
```

```js
// app.mjs
import config from './config.mjs';
console.log(config.port); // 3000
```

✅ **Rule of thumb:**  
Use `default export` when your module **represents a single main functionality or object**.
- e.g., a single class, single utility function, or configuration object.

---

## **2. Multiple Export in ESM**

**Theory:**

- Multiple exports let you **export many functions, objects, or variables** from one module.
    
- Use **`export { ... }`** → these are called **named exports**.
    

**Syntax:**

```js
export { name1, name2, name3 };
```

Or directly when declaring:

```js
export function add(a, b) { return a + b; }
export function subtract(a, b) { return a - b; }
```

**Example – Named exports**

```js
// math.mjs
export function add(a, b) { return a + b; }
export function subtract(a, b) { return a - b; }
export function multiply(a, b) { return a * b; }
export const PI = 3.14;
```

**Importing named exports**

```js
// app.mjs
import { add, multiply, PI } from './math.mjs';

console.log(add(5, 3));      // 8
console.log(multiply(5, 3)); // 15
console.log(PI);              // 3.14
```

**Import everything as an object**

```js
import * as math from './math.mjs';

console.log(math.add(2, 3));       // 5
console.log(math.multiply(2, 3));  // 6
console.log(math.PI);               // 3.14
```

✅ **Rule of thumb:**  
Use **named exports** when your module **has multiple utilities or objects** and none of them is the single main feature.

- e.g., a utility library with many functions (`math.js`, `string-utils.js`).
    

---

## **3. Default vs Named – When to Use Which**

| Use Case                                       | Export Type | Example                                       |
| ---------------------------------------------- | ----------- | --------------------------------------------- |
| Module has **one main thing**                  | `default`   | Single class, single function, config object  |
| Module has **multiple utilities or constants** | `named`     | Math functions, multiple helpers              |
| You want **easy import without curly braces**  | `default`   | `import greet from './greet.mjs';`            |
| You want **selective import**                  | `named`     | `import { add, multiply } from './math.mjs';` |

**Mixing both in one module:**

```js
// example.mjs
export const a = 10;
export const b = 20;
export default function sum(x, y) { return x + y; }
```

```js
// app.mjs
import sum, { a, b } from './example.mjs';
console.log(sum(a, b)); // 30
```

---

### ✅ **Key Takeaways**

1. **Default export** → One main thing, imported without `{}`.
    
2. **Named export** → Multiple things, imported using `{}`.
    
3. **You can mix** both in one file, but usually **stick to one style per module** for clarity.

---

✅ **Summary:**

- **CommonJS:** `require` + `module.exports` → Default Node.js module system.
    
- **ESM:** `import` + `export` → Modern JS standard.
    
- **Modules** help keep code organized, reusable, and modular.


---


# **🤖 Advance knowledge**


# **Exporting and Importing a Folder in Node.js**

## **1. The Concept**

- Suppose you have a **folder `utils`** containing multiple JS files (modules).
    
- You want to **collect all exports from the folder** in one file (usually `index.js`).
    
- Then you can **import the whole folder** in your app using just the folder path.
    

**Why do this?**

- Cleaner imports
    
- Avoid writing many `require` or `import` statements
    
- Centralizes all exports of a folder
    

---

## **2. Folder Structure Example**

```txt
project/
│
├─ app.js
└─ utils/
    ├─ add.js
    ├─ subtract.js
    └─ index.js
```

**`add.js`**

```js
function add(a, b) {
    return a + b;
}

module.exports = add; // CJS style
```

**`subtract.js`**

```js
function subtract(a, b) {
    return a - b;
}

module.exports = subtract; // CJS style
```

---

## **3. Exporting Folder via `index.js`**

**`index.js` inside `utils` folder**

```js
// Import individual modules
const add = require('./add');
const subtract = require('./subtract');

// Export them together as a single object
module.exports = { add, subtract };
```

- Now `utils` folder can be treated as a **single module**.
    
- `index.js` is **automatically picked up** when you require the folder in Node.js.
    

---

## **4. Importing the Folder in App**

**`app.js`**

```js
// Import the whole folder
const utils = require('./utils');
// Or
const { add, subtract } = require('./utils');

console.log(utils.add(10, 5));      // 15
console.log(utils.subtract(10, 5)); // 5
```

✅ Notice: You **don’t have to write `./utils/index.js`**, Node.js **automatically resolves `index.js`** in the folder.

---

## **5. Using ES Modules (ESM)**

Folder structure can be similar, but syntax changes:

**`add.mjs`**

```js
export function add(a, b) { return a + b; }
```

**`subtract.mjs`**

```js
export function subtract(a, b) { return a - b; }
```

**`index.mjs`**

```js
export { add } from './add.mjs';
export { subtract } from './subtract.mjs';
```

**`app.mjs`**

```js
import * as utils from './utils/index.mjs';

console.log(utils.add(10, 5));      // 15
console.log(utils.subtract(10, 5)); // 5
```

- In ESM, you **must import/export explicitly**.
    
- You can also do **default export** from `index.mjs`:
    

```js
// index.mjs
import { add } from './add.mjs';
import { subtract } from './subtract.mjs';

export default { add, subtract };
```

```js
// app.mjs
import utils from './utils/index.mjs';
console.log(utils.add(2,3)); // 5
```

---

## **6. Advantages of Folder Exports**

1. Cleaner imports: only `const utils = require('./utils')`
    
2. Easier to manage large projects
    
3. Centralizes all related modules in one place
    
4. Works with both **CJS** and **ESM**
    

---

### **7. Bonus: Dynamic Folder Export (Optional)**

You can **automatically export all files in a folder** without manually importing each:

**CJS example (`index.js`):**

```js
const fs = require('fs');
const path = require('path');

const files = fs.readdirSync(__dirname);
const modules = {};

files.forEach(file => {
    if (file === 'index.js') return; // skip index.js
    const name = path.basename(file, '.js');
    modules[name] = require(`./${file}`);
});

module.exports = modules;
```

- Now any new module added to the folder is **automatically exported**.
    

---

### ✅ **Key Takeaways**

1. **Folder as module** → Use `index.js` to collect and export modules.
    
2. **Import folder** → Just `require('./folder')` in CJS.
    
3. **ESM version** → Explicit `export { ... }` or default export in `index.mjs`.
    
4. **Dynamic exports** → Useful for large folders with many modules.