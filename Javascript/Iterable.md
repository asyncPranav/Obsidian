
---

## 🧠 1. What Are Iterables?

> An **iterable** is **any object** that can be **looped over** using `for...of` loop, or can be **spread** using the `...` operator.

**Examples of built-in iterables in JavaScript:**

- Arrays
    
- Strings
    
- Maps
    
- Sets
    
- NodeLists (in browsers)
    
- TypedArrays
    
- `arguments` object
    
- Generators
    

---

## 🔍 2. Why Are Iterables Useful?

Because they allow you to:

- Use `for...of` loop
    
- Use the spread operator `...`
    
- Use `Array.from()`, destructuring, etc.
    

---

## 📦 3. Common Built-in Iterables

|Type|Example|
|---|---|
|Array|`[1, 2, 3]`|
|String|`"hello"`|
|Set|`new Set([1, 2, 3])`|
|Map|`new Map()`|
|Arguments|`function(...args)`|

---

## 🔁 4. Examples

### ✅ Using `for...of` on an Array:

```js
let arr = [10, 20, 30];
for (let val of arr) {
  console.log(val);
}
// Output: 10, 20, 30
```

---

### ✅ Using `for...of` on a String:

```js
for (let char of "Hi") {
  console.log(char);
}
// Output: H, i
```

---

### ✅ Using Spread `...`:

```js
let chars = [..."abc"];
console.log(chars); // ['a', 'b', 'c']
```

---

### ✅ Using `Array.from()` on Set:

```js
let set = new Set(["a", "b"]);
let arr = Array.from(set);
console.log(arr); // ['a', 'b']
```

---

## ⚠️ 5. Not Everything Is Iterable!

Objects `{}` are **not iterable** by default:

```js
let obj = {a: 1, b: 2};
for (let item of obj) {
  console.log(item); // ❌ TypeError: obj is not iterable
}
```

To make it iterable, you must define `[Symbol.iterator]` (advanced topic).

---

## 🔧 6. Custom Iterable (Advanced)

You can create your **own iterable object** by implementing `[Symbol.iterator]`:

```js
let custom = {
  from: 1,
  to: 3,
  [Symbol.iterator]() {
    let current = this.from;
    let end = this.to;
    return {
      next() {
        if (current <= end) {
          return { done: false, value: current++ };
        } else {
          return { done: true };
        }
      }
    };
  }
};

for (let val of custom) {
  console.log(val); // 1, 2, 3
}
```

---

## 💡 Summary Table

|Feature|Iterable?|Usable in `for...of`|Usable with Spread|
|---|---|---|---|
|Array|✅|✅|✅|
|String|✅|✅|✅|
|Object (`{}`)|❌|❌|❌|
|Set|✅|✅|✅|
|Map|✅|✅|✅|

---

## ✅ Use Cases in Real Projects

|Task|Iterable Used|
|---|---|
|Loop through data list|Array|
|Handle characters in string|String|
|Unique values|Set|
|Key-value pairs|Map|
|Combine all elements|Spread `...`|



---


## 📘 JavaScript Iterables – Detailed Notes

---

## 📌 1. What is an Iterable?

> An **Iterable** is a data structure in JavaScript that allows sequential **iteration over its elements** — one at a time — using the **`for...of` loop** or **spread syntax**.

🔹 A data structure is considered **iterable** if it has a special method called **`Symbol.iterator()`**.

```js
typeof arr[Symbol.iterator]; // "function" → array is iterable
```

---

## 📌 2. How Iterables Work Internally

When you use a `for...of` loop, JavaScript:

1. Calls the `[Symbol.iterator]()` method on the object.
    
2. Gets an **iterator** object.
    
3. Calls the `next()` method on that iterator repeatedly.
    
4. The `next()` method returns an object:  
    `{ value: ..., done: false }` or `{ done: true }`
    

---

## 📌 3. Built-in Iterables in JavaScript

| Type       | Example                       |
| ---------- | ----------------------------- |
| Array      | `[1, 2, 3]`                   |
| String     | `"Hello"`                     |
| Map        | `new Map()`                   |
| Set        | `new Set()`                   |
| TypedArray | `new Uint8Array()`            |
| Generator  | `function*`                   |
| DOM List   | `document.querySelectorAll()` |

✅ These all implement the `Symbol.iterator()` method.

---

## 📌 4. Non-Iterable Examples

|❌ Type|Example|
|---|---|
|Object|`{ a: 1, b: 2 }`|
|Error object|`new Error("err")`|

You **cannot** use `for...of` or spread `...` on them **unless** you define your own `Symbol.iterator`.

---

## 📌 5. How to Use Iterables

### ✅ Using `for...of` loop

```js
let str = "Hi";
for (let char of str) {
  console.log(char);
}
// Output: H, i
```

---

### ✅ Using Spread `...`

```js
let arr = [1, 2, 3];
let clone = [...arr];
console.log(clone); // [1, 2, 3]
```

---

### ✅ Using `Array.from()`

```js
let set = new Set(["a", "b"]);
let arr = Array.from(set);
console.log(arr); // ['a', 'b']
```

---

### ✅ Destructuring

```js
let [a, b] = "AB";
console.log(a, b); // A B
```

---

## 📌 6. Custom Iterable Example

You can make your **own iterable object**:

```js
let range = {
  from: 1,
  to: 3,

  [Symbol.iterator]() {
    let current = this.from;
    let last = this.to;
	
    return {
      next() {
        if (current <= last) {
          return { done: false, value: current++ };
        } else {
          return { done: true };
        }
      }
    };
  }
};

for (let num of range) {
  console.log(num); // 1, 2, 3
}
```

---

## 📌 7. Iterable vs Iterator

|Feature|Iterable|Iterator|
|---|---|---|
|Has `Symbol.iterator()`|✅ Yes|❌ No|
|Used in `for...of`|✅ Yes|✅ Yes (internally)|
|Has `next()` method|❌ No|✅ Yes|
|Returns sequence|✅ Yes, through iterator|✅ Yes|

---

## 📌 8. Difference: `for...in` vs `for...of`

|Feature|`for...in`|`for...of`|
|---|---|---|
|Iterates over|Property names (keys)|Values of iterable|
|Use on objects|✅ Yes|❌ No (unless custom iterable)|
|Use on arrays|✅ But not recommended|✅ Recommended|

```js
let arr = [10, 20, 30];
for (let i in arr) console.log(i);  // 0, 1, 2 (indexes)
for (let val of arr) console.log(val); // 10, 20, 30 (values)
```

---

## 📌 9. Use Cases in Real Projects

|Task|Iterable Used|
|---|---|
|Loop through users|Array|
|Loop through characters in text|String|
|Store unique elements|Set|
|Store key-value pairs|Map|
|Process stream of values|Generator|

---

## 📌 10. Common Interview Question

🧠 **"How does `for...of` work internally?"**  
🧠 **"What is `Symbol.iterator` and how is it used?"**  
🧠 **"How to make a custom iterable object?"**

---

## ✅ Summary Points

- **Iterable** = an object with `[Symbol.iterator]()` method.
    
- Used in `for...of`, `...spread`, destructuring, `Array.from()`, etc.
    
- Common examples: Array, String, Map, Set.
    
- Objects `{}` are **not iterable** by default.
    
- You can define custom iterables using `Symbol.iterator`.
