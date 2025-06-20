
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