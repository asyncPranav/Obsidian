
---

# 📘 Chapter 5: State – Managing Data in React

---

## 1️⃣ What is State?

### Simple Definition

**State** is **data that belongs to a component and can change over time**.

When state changes → **UI updates automatically**.

### Example (Real Life)

- Like button → count changes
    
- Counter → number increases
    
- Input field → value changes
    

All of these use **state**.

---

## 2️⃣ Why State Is Needed?

Without state ❌:

- UI is static
    
- Data doesn’t change
    
- No interaction
    

With state ✅:

- Dynamic UI
    
- User interaction
    
- Real apps (forms, carts, dashboards)
    

📌 **Props alone are NOT enough** because props are read-only.

---

## 3️⃣ useState Hook

### What is a Hook?

A **hook** is a special function that lets you use React features in functional components.

### useState Syntax

```jsx
const [state, setState] = useState(initialValue);
```

### Example

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return <h1>{count}</h1>;
}
```

📌 `count` → current state  
📌 `setCount` → function to update state  
📌 `0` → initial value

---

## 4️⃣ Updating State

### ❌ Wrong (Direct Update)

```jsx
count = count + 1; // ❌
```

### ✅ Correct

```jsx
setCount(count + 1);
```

### Example with Button

```jsx
<button onClick={() => setCount(count + 1)}>
  Increment
</button>
```

📌 State must be updated **using setter function only**

---

## 5️⃣ State Re-Rendering (VERY IMPORTANT ⭐)

### What Happens When State Changes?

1. setState is called
    
2. React re-runs the component
    
3. JSX recalculates
    
4. DOM updates
    

📌 React does **NOT** reload the page  
📌 Only affected components update

---

## 6️⃣ State vs Props

|State|Props|
|---|---|
|Internal data|External data|
|Mutable|Read-only|
|Managed by component|Passed by parent|
|Changes trigger re-render|Changes trigger re-render|

Example:

```jsx
function Child({ name }) { // props
  const [age, setAge] = useState(20); // state
}
```

---

## 7️⃣ Multiple State Variables

```jsx
const [name, setName] = useState("Rahul");
const [age, setAge] = useState(22);
```

✔ Each state is independent  
✔ Clean and readable

---

## 8️⃣ Functional State Update (IMPORTANT ⭐)

### Problem

State updates are **asynchronous**.

❌ Wrong:

```jsx
setCount(count + 1);
setCount(count + 1);
```

### ✅ Correct (Functional Update)

`setCount(prevCount => prevCount + 1); setCount(prevCount => prevCount + 1);`

📌 Always use functional update when:

- New state depends on old state
    

---

## 9️⃣ State with Objects

`const [user, setUser] = useState({   name: "Rahul",   age: 22 });`

### Updating Object State

`setUser({ ...user, age: 23 });`

📌 Spread operator is mandatory  
📌 React does **NOT** merge objects automatically

---

## 🔟 State with Arrays

`const [items, setItems] = useState([]);`

Add item:

`setItems([...items, "Apple"]);`

📌 Never mutate arrays directly (`push` ❌)

---

## 1️⃣1️⃣ Lazy Initial State (Extra Topic ⭐)

If initial state is heavy computation:

`const [value, setValue] = useState(() => {   return expensiveCalculation(); });`

📌 Function runs only once

---

## 1️⃣2️⃣ State Update Batching (Extra Topic)

React batches multiple state updates for performance.

`setCount(c => c + 1); setCount(c => c + 1);`

✔ Results in +2  
✔ Single re-render

---

## 1️⃣3️⃣ Common State Mistakes ❌

1. Mutating state directly
    
2. Forgetting setter function
    
3. Using props as state
    
4. Expecting immediate update
    
5. Not using functional update
    
6. Overusing state
    

---

## 1️⃣4️⃣ When NOT to Use State (Extra Topic)

❌ Don’t use state for:

- Constants
    
- Derived values
    
- Props copies
    

`// ❌ Bad const [fullName, setFullName] = useState(   firstName + lastName );`

---

## 1️⃣5️⃣ Lifting State Up (Intro Topic ⭐)

When multiple components need same data:  
👉 Move state to **common parent**

`function Parent() {   const [count, setCount] = useState(0);   return <Child count={count} />; }`

(We’ll cover deeply later)

---

## 🧠 Mental Model (Easy)

> **Props → passed from parent**  
> **State → owned by component**

---

## ✅ Chapter 5 Summary

✔ State makes UI dynamic  
✔ useState manages state  
✔ State updates trigger re-render  
✔ Never mutate state directly  
✔ Use functional update when needed  
✔ Props ≠ State