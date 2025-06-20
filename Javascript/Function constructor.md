
---

## 🧠 What is the `Function` Constructor?

JavaScript allows you to **create functions dynamically at runtime** using the `Function` constructor.

### ✅ Syntax:

```js
new Function([arg1, arg2, ...,] functionBody)
```

- All arguments are strings.
    
- Last argument is the body of the function.
    
- All other arguments are parameter names.
    

---

## 🧪 Basic Example

```js
let sum = new Function('a', 'b', 'return a + b');
console.log(sum(5, 3)); // ✅ Output: 8
```

This is equivalent to:

```js
function sum(a, b) {
  return a + b;
}
```

---

## 🎯 Use Cases

### ✅ 1. **Dynamic Formula Evaluation**

Imagine user input is `"x * y + 10"`:

```js
let userFormula = "x * y + 10";
let calc = new Function("x", "y", `return ${userFormula}`);
console.log(calc(5, 3)); // 5*3 + 10 = 25
```

Useful in calculators or spreadsheet apps.

---

### ✅ 2. **Building a Function from Strings**

```js
let greet = new Function("name", "return 'Hello, ' + name + '!';");
console.log(greet("Pranav")); // ✅ Hello, Pranav!
```

---

### ✅ 3. **No Parameters**

```js
let sayHi = new Function("console.log('Hi!')");
sayHi(); // ✅ Hi!
```

---

### ✅ 4. **Inside Another Function**

You can generate logic inside functions dynamically:

```js
function createPowerFunction(exp) {
  return new Function("x", `return x ** ${exp}`);
}

let square = createPowerFunction(2);
let cube = createPowerFunction(3);

console.log(square(4)); // ✅ 16
console.log(cube(2));   // ✅ 8
```

---

## ⚠️ Important Notes

### ⚠️ 1. **Always Uses Global Scope**

```js
let x = 10;
let f = new Function("return x");
console.log(f()); // ❌ ReferenceError: x is not defined
```

✅ This works only if `x` is global:

```js
globalThis.x = 10;
let f = new Function("return x");
console.log(f()); // ✅ 10
```

---

### ⚠️ 2. **Security Risk (Like eval)**

If you're using user input:

```js
let userCode = "alert('Hacked!')";
let dangerous = new Function(userCode);
dangerous(); // ⚠️ Very dangerous! Never do this with untrusted input!
```

Never use `Function` with external or user-provided strings unless fully sanitized.

---

## 🏗️ Advanced Examples

### 🧪 1. Function with multiple parameters

```js
let fn = new Function("a", "b", "c", "return a * b + c");
console.log(fn(2, 3, 4)); // ✅ 10
```

---

### 🧪 2. Returning object

```js
let createUser = new Function("name", "age", `
    return {
        name: name,
        age: age
    };
`);

console.log(createUser("Pranav", 21)); 
// ✅ { name: 'Pranav', age: 21 }
```

---

### 🧪 3. With Array and Loop

```js
let arrSum = new Function("arr", `
    let sum = 0;
    for (let num of arr) {
        sum += num;
    }
    return sum;
`);
console.log(arrSum([1, 2, 3])); // ✅ 6
```

---

### 🧪 4. Immediately call it

```js
console.log(new Function("return 10 * 2")()); // ✅ 20
```

---

## 💡 Comparison: `Function` Constructor vs `eval`

|Feature|`Function`|`eval`|
|---|---|---|
|Scope|Global only|Current (local) scope|
|Security Risk|High|Even higher|
|Performance|Slow|Very slow|
|Use Case|Dynamic function build|Evaluate short expressions|

---

## 🧾 Summary

|✅ Use Case|❌ Avoid When|
|---|---|
|Dynamic formula builder|Working with untrusted user input|
|Code playground / sandbox|Needing closures or local scope access|
|Lightweight interpreters|Needing good performance or readability|

---

## ✅ TL;DR

- `new Function("a", "b", "return a + b")` creates a function at runtime.
    
- It only has access to **global** scope, not local variables.
    
- Powerful for **formulas**, **interpreters**, **custom logic**.
    
- ❗ Use with care — has same risks as `eval`.


---


## 🔍 What is `eval()`?

`eval()` is a built-in JavaScript function that **evaluates a string as code**.

### 📌 Syntax:

js

Copy code

`eval(codeAsString)`

---

## 🧪 Basic Example

js

Copy code

`let result = eval("2 + 2"); console.log(result); // ✅ 4`

You can also use it to execute statements:

js

Copy code

`eval("let x = 10; console.log(x);"); // ✅ prints 10`

---

## ⚠️ DANGERS of `eval()`

### 🚨 1. **Security Risk**

If you're taking code or input from the user and passing it into `eval()`, you’re vulnerable to **code injection**.

js

Copy code

`let userInput = "alert('Hacked!')"; eval(userInput); // ⚠️ Dangerous!`

### 🚨 2. **Performance**

JavaScript engines can’t optimize `eval()` well because they can’t predict what it will run.

### 🚨 3. **Scope Pollution**

`eval()` runs in the **current scope**, so it can create variables dynamically and mess up your scope:

js

Copy code

`let x = 5; eval("let x = 10; console.log(x);"); // 10 console.log(x); // still 5 (but confusing)`

---

## ✅ When is `eval()` used (carefully)?

- Code playgrounds (like JSFiddle, CodePen)
    
- Custom interpreters
    
- Educational tools
    
- Only in **very controlled** environments
    

---

## 🧾 Real Use Example (Not recommended in most cases):

js

Copy code

`function calculateExpression(expr) {   try {     return eval(expr);   } catch (e) {     return "Invalid expression";   } }  console.log(calculateExpression("5 + 3 * 2")); // ✅ 11 console.log(calculateExpression("alert('Hack')")); // ❌ risky`

---

## 🔐 Safe Alternative: `Function` constructor

js

Copy code

``let expr = "5 + 10"; let result = new Function(`return ${expr}`)(); console.log(result); // ✅ 15``

---

## ✅ TL;DR

|Feature|Description|
|---|---|
|✅ Exists?|Yes|
|🚨 Risky|Very — avoid user input|
|🧠 Use cases|Playgrounds, interpreters, experiments|
|🛑 Don't use it|In production, or with external input|