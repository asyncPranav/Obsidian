
---

## 📘 What is `eval()`?

`eval()` is a **built-in JavaScript function** that lets you execute **a string of JavaScript code** as if it were written normally.

---

## 🧠 Syntax

```js
eval(codeAsString)
```

- `codeAsString` → a string containing valid JavaScript code.
    

---

## ✅ Basic Examples

### 1. **Simple expression**

```js
let result = eval("2 + 3");
console.log(result); // 5
```

### 2. **Variable declaration**

```js
eval("let a = 10;");
console.log(a); // ❌ ReferenceError: a is not defined
```

> Why? Because `let`/`const` declared inside `eval()` stay inside that scope.

But if you use `var`:

```js
eval("var b = 20;");
console.log(b); // ✅ 20
```

---

### 3. **Function inside eval**

```js
eval("function greet() { return 'Hello'; }");
console.log(greet()); // ✅ Hello
```

---

## 🚨 Why is `eval()` dangerous?

### 1. **Security Risks (Code Injection)**

```js
let userInput = "alert('Hacked!')";
eval(userInput); // 🚨 Unsafe if user controls the input!
```

If someone gives you a string that includes malicious code, `eval()` will run it.

---

### 2. **Performance Issues**

- JavaScript engines can’t optimize code inside `eval()`.
    
- It slows down execution.
    

---

### 3. **Confusing Scope**

```js
let x = 5;
eval("x = 10");
console.log(x); // 10 ✅

eval("let y = 100;");
console.log(y); // ❌ ReferenceError: y is not defined
```

---

## 🔧 Real Use Case (Calculator)

```js
function calculate(expr) {
  return eval(expr);
}

console.log(calculate("10 + 20")); // 30
console.log(calculate("Math.sqrt(16)")); // 4
```

> ⚠️ Don’t use `eval()` like this with user input!

---

## 💡 eval() vs Function()

A **safer alternative** to `eval()` is the **`Function` constructor**:

js

Copy code

``let expr = "10 * 2"; let result = new Function(`return ${expr}`)(); console.log(result); // ✅ 20``

🧠 It runs in its **own scope**, unlike `eval`.

---

## 🧪 Other Examples

### 1. Define multiple variables

js

Copy code

`eval("var a = 1; var b = 2;"); console.log(a + b); // 3`

### 2. Use inside a loop (❌ bad practice)

js

Copy code

``for (let i = 1; i <= 3; i++) {   eval(`console.log("Number: " + ${i})`); } // Prints: Number: 1, 2, 3``

---

## 🛑 When NOT to Use `eval()`

- ❌ Never with **user input**
    
- ❌ Never for JSON (use `JSON.parse()` instead)
    
- ❌ Never to define functions dynamically
    

---

## 🧰 When It’s (Slightly) Acceptable

- JavaScript **playgrounds**
    
- **Education tools**
    
- Some **framework internals**
    
- **REPLs** (read-eval-print loops)
    

---

## 🧾 Summary Table

|Feature|eval()|
|---|---|
|Type|Function|
|Accepts|String of JS code|
|Security Risk|🚨 Very High|
|Optimized by JS|❌ No|
|Scope Access|✅ Accesses local scope|
|Performance|❌ Slower|
|Safer Alternative|✅ `Function()` constructor|

---

## ✅ TL;DR

- `eval()` runs code from a string.
    
- It’s **powerful** but **dangerous**.
    
- Avoid using it in real apps, especially with user input.
    
- Prefer safer alternatives like `Function()` or other design patterns.