
---
# 📘 Chapter 8: Lists & Keys in React

---

## 1️⃣ Rendering Lists in React

### What does “rendering a list” mean?

Rendering a list means:

> **Displaying multiple similar UI elements from an array of data**

Example:

- List of users
    
- Products
    
- Todo items
    
- Comments
    

In React, we **don’t write repeated JSX manually**.  
We **generate components dynamically from arrays**.

---

## 2️⃣ Rendering Lists with `.map()`

### Why `.map()`?

- `.map()` transforms one array into another
    
- Perfect for converting **data → JSX**
    

### Basic JavaScript Example

```js
const numbers = [1, 2, 3];
const doubled = numbers.map(num => num * 2);
```

---

### Basic React Example

```jsx
const fruits = ["Apple", "Banana", "Mango"];

function App() {
  return (
    <ul>
      {fruits.map(fruit => (
        <li>{fruit}</li>
      ))}
    </ul>
  );
}
```

📌 JSX can render arrays  
📌 Each iteration returns JSX

---

## 3️⃣ Dynamic Components from Arrays

Instead of HTML elements, we can render **components**.

```jsx
function User({ name }) {
  return <li>{name}</li>;
}

function App() {
  const users = ["Rahul", "Amit", "Neha"];

  return (
    <ul>
      {users.map(user => (
        <User name={user} />
      ))}
    </ul>
  );
}
```

✔ Clean  
✔ Reusable  
✔ Dynamic

---

## 4️⃣ What Are Keys?

### Simple Definition

A **key** is a **special attribute** that helps React **identify which list item has changed, added, or removed**.

```jsx
<li key="1">Apple</li>
```

📌 Keys are **not available as props**  
📌 Used internally by React

---

## 5️⃣ Why Keys Are Important? (VERY IMPORTANT ⭐)

React uses **Virtual DOM diffing**.

Without keys ❌:

- React can’t track items properly
    
- Wrong UI updates
    
- Performance issues
    

With keys ✅:

- Efficient re-rendering
    
- Correct DOM updates
    
- Stable UI
    

---

## 6️⃣ Using Keys with `.map()`

```jsx
const fruits = ["Apple", "Banana", "Mango"];

function App() {
  return (
    <ul>
      {fruits.map((fruit, index) => (
        <li key={index}>{fruit}</li>
      ))}
    </ul>
  );
}
```

⚠️ This works, but **NOT recommended** (explained below).

---

## 7️⃣ Index as Key – Why NOT? ❌

### Problem with Index as Key

If list order changes:

- React thinks items didn’t change
    
- Causes incorrect UI updates
    

Example scenario:

```js
["Apple", "Banana", "Mango"]
```

Remove `Apple`:

```js
["Banana", "Mango"]
```

Indexes change:

- Banana: 1 → 0
    
- Mango: 2 → 1
    

❌ React reuses wrong DOM nodes

---

### When Index is Acceptable (Rare Cases)

✔ Static list  
✔ No reordering  
✔ No add/remove

Otherwise ❌ avoid

---

## 8️⃣ Correct Way to Use Keys (BEST PRACTICE ⭐)

### Use **Unique & Stable IDs**

```jsx
const users = [
  { id: 1, name: "Rahul" },
  { id: 2, name: "Amit" },
  { id: 3, name: "Neha" }
];

function App() {
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

✔ Unique  
✔ Stable  
✔ Best performance

---

## 9️⃣ Keys with Components (IMPORTANT)

```jsx
<User key={user.id} name={user.name} />
```

📌 Key goes on **component**, not inside it

❌ Wrong:

```jsx
<User name={user.name} />
```

---

## 🔟 Keys Are NOT Props (Common Confusion)

```jsx
function User(props) {
  console.log(props.key); // ❌ undefined
}
```

✔ Keys are used internally  
❌ Not accessible via props

---

## 1️⃣1️⃣ Rendering Lists with Fragments

```jsx
{items.map(item => (
  <React.Fragment key={item.id}>
    <h3>{item.title}</h3>
    <p>{item.description}</p>
  </React.Fragment>
))}
```

📌 Fragment can also have a key

---

## 1️⃣2️⃣ Conditional Rendering inside Lists (Extra Topic ⭐)

`{users.map(user =>   user.isActive ? (     <li key={user.id}>{user.name}</li>   ) : null )}`

---

## 1️⃣3️⃣ Common Mistakes ❌

1. Forgetting `key`
    
2. Using index as key
    
3. Using non-unique key
    
4. Using random keys (`Math.random()`)
    
5. Putting key inside child instead of map
    
6. Expecting `key` in props
    

---

## 1️⃣4️⃣ Interview Questions Points ⭐

- Why keys are required?
    
- Why index is bad?
    
- Where to place key?
    
- Can keys be duplicated? ❌
    

---

## 🧠 Mental Model (Easy)

> **Array → map() → JSX → key → Efficient UI**

---

## ✅ Chapter 8 Summary

✔ Lists are rendered using `.map()`  
✔ Keys help React identify elements  
✔ Use unique & stable keys  
✔ Avoid index as key  
✔ Keys improve performance & correctness