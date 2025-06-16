
---






---


### ✅ Code:

```js
let recipeMap = new Map([
    [ "cucumber", 500 ],
    [ "tomatoes", 530 ],
    [ "onions", 30 ]
]);

console.log(recipeMap);
```

---

### 💡 Explanation:

- `Map` is a **special object** in JavaScript that stores key-value pairs.
    
- Unlike plain objects, **keys in Map can be of any type** (not just strings).
    
- Here, you're using **strings as keys** (`"cucumber"`, `"tomatoes"`, `"onions"`), and their values are numbers.
    

---

### 🔍 Internally:

```js
// This is similar to doing:
recipeMap.set("cucumber", 500);
recipeMap.set("tomatoes", 530);
recipeMap.set("onions", 30);
```

But the `[[]]` syntax is a **shorter and cleaner way** to initialize the map with multiple entries.

---

### 🔎 Output:

When you run:

```js
console.log(recipeMap);
```

You’ll get something like:

```js
Map(3) {
  'cucumber' => 500,
  'tomatoes' => 530,
  'onions' => 30
}
```

---

### ✅ Features of `Map`:

- Maintains **insertion order** (unlike older versions of plain objects).
    
- Keys can be **any type** (objects, functions, NaN, etc.).
    
- Can be easily iterated using `.forEach()` or `for...of`.
    

---


#DOUBT **You said: what we will call it array of array in map**


When you write this:

```js
let recipeMap = new Map([
  ["cucumber", 500],
  ["tomatoes", 530],
  ["onions", 30]
]);
```

The part inside `Map([...])` is:

```js
[
  ["cucumber", 500],
  ["tomatoes", 530],
  ["onions", 30]
]
```

---

### ✅ What Do We Call This?

🔹 This is called an **array of key-value pairs**  
🔹 Technically, it's an **array of arrays** (or more specifically, a **2D array**, where each inner array has exactly 2 elements)

Each inner array is a pair:

- Index `0` is the **key**
    
- Index `1` is the **value**
    

So this format is also called:  **Entries (or Key-Value Pairs)**

---

### 🧠 In Map Terms:

> You are **initializing a Map using an array of entries** (key-value pairs).

This same format is returned when you do:

```js
Array.from(map.entries())
```

Or:

```js
[...map]
```

---

### ✅ Summary:

|Structure|Common Name|
|---|---|
|`[["cucumber", 500], ...]`|Array of arrays|
|Each `["cucumber", 500]`|Key-value pair / Entry|
|Whole input to `new Map([...])`|Array of key-value pairs|

---


# ✅ What is `Object.entries(obj)`?


It takes a **plain object** like this:

```js
let obj = {
  cucumber: 500,
  tomatoes: 350,
  onions: 50
};
```

And converts it to an **array of key-value pairs**:

```js
Object.entries(obj);
// Output: [["cucumber", 500], ["tomatoes", 350], ["onions", 50]]
```

---

### 🧠 Why is this useful?

Because `Map` accepts this format (array of key-value pairs), we can use it to convert an object into a `Map`:

---

### ✅ Convert Object → Map

```js
let obj = {
  cucumber: 500,
  tomatoes: 350,
  onions: 50
};

let map = new Map(Object.entries(obj));

console.log(map);
// Map(3) { 'cucumber' => 500, 'tomatoes' => 350, 'onions' => 50 }
```

---

### 🔄 Bonus: Convert Map → Object

You can also go **back** from a `Map` to a plain object using `Object.fromEntries()`:

```js
let backToObj = Object.fromEntries(map);

console.log(backToObj);
// { cucumber: 500, tomatoes: 350, onions: 50 }
```

---

### 🔁 Summary:

|Conversion|Syntax|
|---|---|
|Object → Map|`new Map(Object.entries(obj))`|
|Map → Object|`Object.fromEntries(map)`|

---


# 🧠 `Object.fromEntries()` in JavaScript


### ✅ What it does:

`Object.fromEntries()` **converts** a list of key-value pairs (like from a `Map` or an array of arrays) into a **plain JavaScript object**.

---

### 🔁 Reverse of:

`Object.fromEntries()` is the **opposite of** `Object.entries()`.

- `Object.entries(obj)` → gives array of key-value pairs
    
- `Object.fromEntries(pairs)` → builds an object from key-value pairs
    

---

### ✅ Syntax:

```js
Object.fromEntries(iterable)
```

- `iterable` must be something like an array of `[key, value]` pairs — commonly from `Map` or a 2D array.
    

---

### 🔷 Example 1: Convert `Map` → Object

```js
let recipeMap = new Map([
  ["cucumber", 500],
  ["tomatoes", 350],
  ["onions", 50]
]);

let recipeObj = Object.fromEntries(recipeMap);

console.log(recipeObj);
// Output: { cucumber: 500, tomatoes: 350, onions: 50 }
```

✅ Now `recipeObj` is a regular object you can use like:

```js
console.log(recipeObj.tomatoes); // 350
```

---

### 🔷 Example 2: Convert array of pairs → Object

```js
let pairs = [
  ["name", "John"],
  ["age", 30],
  ["isAdmin", false]
];

let user = Object.fromEntries(pairs);

console.log(user);
// Output: { name: 'John', age: 30, isAdmin: false }
```

---

### 🧠 Why use `Object.fromEntries()`?

- Convert from `Map` or array of pairs to plain object.
    
- Useful for transforming data before using it in APIs, forms, UI, etc.
    
- Helps when working with `Object.entries()`, `Map.entries()`, or `formData.entries()`.
    

---

### 📝 Summary Table:

|Task|Method|
|---|---|
|Object → array of pairs|`Object.entries(obj)`|
|Map → Object|`Object.fromEntries()`|
|Array of pairs → Object|`Object.fromEntries()`|

---


# 📘 `map.entries()` in JavaScript


### ✅ What is `map.entries()`?

- `map.entries()` is a method that returns an **iterator** containing **[key, value] pairs** from a `Map` object.
    
- This iterator follows the **insertion order** of the elements in the `Map`.
    

---

### ✅ Syntax

```js
map.entries()
```

- Returns an iterator object of `[key, value]` pairs.

----

### 🧠 Use Cases

- `for...of` to loop over it.
    
- `Array.from()` or `[...map.entries()]` to convert it into an array.
	
- To **loop over key-value pairs**
    
- To **convert a Map to array**
    
- To **transform data**
    
- For exporting or debugging map content

---

## 🔹 Basic Example

```js
let fruits = new Map([
  ["apple", 5],
  ["banana", 10],
  ["mango", 7]
]);

for (let entry of fruits.entries()) {
  console.log(entry);
}
```

### 🟢 Output:

```sh
[ 'apple', 5 ]
[ 'banana', 10 ]
[ 'mango', 7 ]
```


### `.entries()` vs `.forEach()`

```js
for (let entry of fruits.entries()) {
  console.log(entry); 
}
```


```js
fruits.forEach((value, key) => {
  console.log(`${key} = ${value}`);
});
```

---

## 🔹 Destructuring Example

```js
let fruits = new Map([
  ["apple", 5],
  ["banana", 10],
  ["mango", 7]
]);

for (let [key, value] of fruits.entries()) {
  console.log(`${key} → ${value}`);
}
```

### 🟢 Output:

```sh
apple → 5
banana → 10
mango → 7
```

---

## 🔹 Convert Map to Array of Pairs

```js
let fruits = new Map([
  ["apple", 5],
  ["banana", 10],
  ["mango", 7]
]);

let arr = Array.from(fruits.entries());
console.log(arr);
```

### 🟢 Output:

```sh
[
  ["apple", 5],
  ["banana", 10],
  ["mango", 7]
]
```

---

### 🔹 Convert Back to Map using entries

```js
let arr = [
  ["apple", 5],
  ["banana", 10],
  ["mango", 7]
];

let newMap = new Map(arr);
console.log(newMap);
```

🟢 **Output:**

```sh
Map(3) {
  'apple' => 5,
  'banana' => 10,
  'mango' => 7
}
```

---

### 🔹 Use with Spread Operator

```js
let fruits = new Map([
  ["apple", 5],
  ["banana", 10],
  ["mango", 7]
]);

console.log([...fruits.entries()]);
```

🟢 **Output:**

```js
[
  [ 'apple', 5 ],
  [ 'banana', 10 ],
  [ 'mango', 7 ]
]
```

✅ Same output as `Array.from()` — just shorter syntax.

---

### 🔹 Use in Function

```js
function printMap(map) {
  for (let [key, value] of map.entries()) {
    console.log(`Key: ${key}, Value: ${value}`);
  }
}

let fruits = new Map([
  ["apple", 5],
  ["banana", 10],
  ["mango", 7]
]);

printMap(fruits);
```

🟢 **Output:**

```sh
Key: apple, Value: 5
Key: banana, Value: 10
Key: mango, Value: 7
```

---

## 🔹 Real-world Example: Map of Users

```js
let user1 = { name: "Alice" };
let user2 = { name: "Bob" };

let roles = new Map([
  [user1, "admin"],
  [user2, "editor"]
]);

for (let [user, role] of roles.entries()) {
  console.log(`${user.name} is ${role}`);
}
```

🟢 **Output:**

```sh
Alice is Admin
Bob is Editor
```

### 🧠 Note:

- This works because `Map` can have objects as keys (unlike plain objects).
    

---

## 🔍 Important Notes:

|Feature|Value|
|---|---|
|Returns|Iterator of [key, value] pairs|
|Preserves order|Yes|
|Can use with|`for...of`, `Array.from()`, spread|
|Can use destructuring|Yes (`[key, value]`)|
|Key types allowed|Any (object, number, string, etc.)|

---

## 🧠 Summary

- `map.entries()` gives all entries in `[key, value]` format.
    
- Can be looped over or converted to an array.
    
- Essential when transforming, displaying, or exporting map data.