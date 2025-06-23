
---

## 🔍 What’s the Core Concept?

In JavaScript, **functions are objects**.  
When you assign a function to another variable, you're **copying the reference**, not cloning the function.

---

## 🧪 Example

```js
let func1 = function() {
    return "Hello";
};

let func2 = func1;

console.log(func1()); // "Hello"
console.log(func2()); // "Hello"
```

✅ Both `func1` and `func2` point to the **same function in memory**.

---

## 📌 How It Works Under the Hood

```js
func1  --->  💾 function object in memory
func2  --->  ⬆️ same function object (not a copy)
```

- `func2 = func1` means: "Make `func2` point to the same memory location that `func1` points to."
    
- No new function is created.
    

---

## 🎯 Real-Life Analogy

Think of `func1` as a contact saved on your phone:

- `func2 = func1` is like creating another shortcut to the same contact.
    
- If you change the number via one shortcut, the other reflects it too.
    
- You’re not making a _new person_, just another way to reach them.
    

---

## 🔁 Modifying the Function via One Reference

```js
func1.customProp = "test";
console.log(func2.customProp); // "test"
```

✅ Shows they are pointing to the **same object**.

---

## ❗ What Happens If You Reassign One?

```js
func1 = function() {
    return "New func1";
};

console.log(func1()); // "New func1"
console.log(func2()); // "Hello"
```

❗ Now `func1` points to a **new function**, but `func2` still points to the **original** one.

---

## 🧠 Key Takeaways

|Concept|Explanation|
|---|---|
|Functions are objects|They behave like any other object in JS|
|Assignment copies reference|`func2 = func1` shares the same function|
|No duplication|No new function is created in memory|
|Reassigning creates new|Only if you do `func1 = function() {...}` again|

---

## ✅ Good to Know

- This same behavior applies to:
    
    - Arrays: `let arr2 = arr1`
        
    - Objects: `let obj2 = obj1`
        
- Only **primitive values** (like numbers, strings, booleans) get copied by value.
    

---

## 📚 Summary

**Assigning a function to another variable copies the reference**, not the function.  
They point to the **same function object in memory**, until one is **explicitly reassigned**.



---



## 🧠 Step-by-Step with Diagrams



### 🔹 1. Initial Function Declaration

```js
let func1 = function() {
    return "Hello";
};
```

### 💾 Memory Representation:

```sh
     func1
       │
       ▼
┌────────────────────┐
│  function object   │
│  returns "Hello"   │
└────────────────────┘
```

---

### 🔹 2. Assigning to func2

```js
let func2 = func1;
```

### 💾 Now Memory Looks Like:

```sh
     func1       func2
       │           │
       ▼           ▼
┌────────────────────┐
│  function object   │
│  returns "Hello"   │
└────────────────────┘
```

✅ Both variables point to the **same function** in memory.

---

### 🔹 3. Adding a Custom Property

```js
func1.custom = "Test";
```

### 💾 Still the Same Object:

vbnet

Copy code

     `func1       func2        │           │        ▼           ▼ ┌──────────────────────────────┐ │  function object             │ │  returns "Hello"             │ │  custom: "Test"              │ └──────────────────────────────┘`

✅ Accessing `func2.custom` will return `"Test"`.

---

### 🔹 4. Reassigning func1 to a New Function

js

Copy code

`func1 = function() {     return "New Hello"; };`

### 💾 Now Looks Like:

pgsql

Copy code

     `func1                     func2        │                         │        ▼                         ▼ ┌────────────────────┐   ┌────────────────────┐ │  function object   │   │  function object   │ │ returns "New Hello"│   │ returns "Hello"    │ └────────────────────┘   └────────────────────┘`

❗ Now `func1` and `func2` point to **different functions**.

---

## 🧩 Final Thought:

> As long as you don't reassign `func1`, both variables share the same memory reference — like two names for the same function.