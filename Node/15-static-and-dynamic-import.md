
---


- #### xyz.js
	
```js
console.log("xyz.js file exeecuted");
```

- #### math.js
	
```js
console.log("math.js file executed");

const add = (a, b) => {
  return a + b;
};

function subtract(a, b) {
  return a - b;
}

const multiply = function (a, b) {
  return a * b;
};

export { add, subtract, multiply };
```

- #### app.js
	
```js
import { add, subtract, multiply } from "./utils/math.js";
import "./utils/xyz.js";

console.log(add(3, 5));
console.log(subtract(10, 4));
console.log(multiply(3, 7));
```

- ### Output of app.js
```sh
math.js file executed
xyz.js file exeecuted
8
6
21
```


#DOUBT  - **is `import("./utils/xyz.js")` different from `import "./xyz.js"`**


Yes. They are **different**.

---

# 1. Static Import

```javascript
import "./xyz.js";
```

### Characteristics

- Executed **before** your code starts running.
    
- Synchronous from your code's perspective (module loading happens before execution).
    
- Cannot be placed inside `if`, loops, or functions.
    
- Used for modules that should always be loaded.
    

Example:

```javascript
import "./xyz.js";

console.log("App started");
```

Output:

```text
xyz loaded
App started
```

---

# 2. Dynamic Import

```javascript
import("./utils/xyz.js");
```

### Characteristics

- Executed **at runtime**, when that line is reached.
    
- Can be used inside functions, conditions, loops, etc.
    
- Returns a **Promise**.
    
- Useful for lazy loading (loading modules only when needed).
    

Example:

```javascript
console.log("Before");

import("./utils/xyz.js");

console.log("After");
```

Output might be:

```text
Before
After
xyz loaded
```

because the import is asynchronous.

---

## If you need the exported values

```javascript
const math = await import("./math.mjs");

console.log(math.add(2, 3));
```

or

```javascript
import("./math.mjs").then((math) => {
    console.log(math.add(2, 3));
});
```

Here, `math` is the module object.

---

# Comparison

|Feature|`import "./xyz.js"`|`import("./xyz.js")`|
|---|---|---|
|Type|Static import|Dynamic import|
|When loaded|Before program execution|At runtime|
|Returns|Nothing|Promise|
|Can be inside `if`/function|❌ No|✅ Yes|
|Used for|Always-needed modules|Lazy loading / conditional loading|

---

## Memory Trick

- **`import "..."`** → **Static import** → Load **before** the program starts.
    
- **`import("...")`** → **Dynamic import** → Load **when** the code reaches that line.


