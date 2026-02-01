
---

# 📘 Chapter 10: `useEffect` – Side Effects in React

---

## 1️⃣ What is a Side Effect?

### 🔹 Simple Definition

A **side effect** is **anything that happens outside normal rendering** of a component.

React’s main job:  
👉 **Render UI based on state & props**

Side effects are:

- NOT directly related to returning JSX
    
- Things that affect **outside world**
    

### 🔹 Examples of Side Effects

|Side Effect|Why it’s a side effect|
|---|---|
|API calls|Data comes from server|
|`setTimeout`, `setInterval`|Time-based|
|`console.log` (sometimes)|External output|
|Event listeners (`window.addEventListener`)|Browser interaction|
|Updating `document.title`|Outside React|

❌ These should **NOT** be written directly in render logic.

---

## 2️⃣ Why Effects Are Needed

### ❌ Wrong Way (Anti-pattern)

```jsx
function App() {
  const [count, setCount] = useState(0);

  document.title = `Count: ${count}`; // ❌ BAD

  return <button onClick={() => setCount(count + 1)}>+</button>;
}
```

❌ Problem:

- Runs on **every render**
    
- Can cause **bugs & performance issues**
    

---

### ✅ Correct Way: useEffect

```jsx
useEffect(() => {
  document.title = `Count: ${count}`;
});
```

👉 React says:

> “Side effects belong in **useEffect**, not in render”

---

## 3️⃣ What is `useEffect`?

### 🔹 Definition

`useEffect` is a React Hook that lets you perform **side effects** in **functional components**.

### 🔹 Syntax

```jsx
useEffect(() => {
  // side effect code
}, [dependencies]);
```

### Breakdown:

|Part|Meaning|
|---|---|
|Callback function|Code to run|
|Dependency array|When to run effect|

---

## 4️⃣ Basic useEffect Usage

### 🟢 Runs on Every Render

```jsx
useEffect(() => {
  console.log("Component rendered");
});
```

📌 No dependency array → runs after **every render**

---

### 🟢 Runs Only Once (On Mount)

```jsx
useEffect(() => {
  console.log("Component mounted");
}, []);
```

📌 Empty dependency array `[]`  
👉 Runs **only once** when component loads

---

### 🟢 Runs When State Changes

```jsx
useEffect(() => {
  console.log("Count changed:", count);
}, [count]);
```

📌 Runs only when `count` changes

---

## 5️⃣ Dependency Array Explained (Very Important)

### 🔹 What is Dependency Array?

The dependency array tells React:

> “Run this effect **only when these values change**”

### Rules:

|Dependency|Behavior|
|---|---|
|No array|Runs every render|
|Empty `[]`|Runs once|
|`[count]`|Runs when `count` changes|
|`[count, name]`|Runs when either changes|

---

### ❌ Common Mistake

```jsx
useEffect(() => {
  setCount(count + 1);
}, [count]);
```

⚠️ Infinite loop!

- Effect updates state
    
- State change triggers effect again
    

---

## 6️⃣ Component Lifecycle (Functional)

In class components:

- `componentDidMount`
    
- `componentDidUpdate`
    
- `componentWillUnmount`
    

### In functional components:

|Lifecycle|useEffect Equivalent|
|---|---|
|Mount|`useEffect(() => {}, [])`|
|Update|`useEffect(() => {}, [dep])`|
|Unmount|Cleanup function|

---

## 7️⃣ Cleanup Function

### 🔹 Why Cleanup is Needed?

To **remove side effects** when component unmounts.

Examples:

- Remove event listeners
    
- Clear intervals
    
- Cancel subscriptions
    

---

### 🧹 Cleanup Syntax

```jsx
useEffect(() => {
  return () => {
    // cleanup code
  };
}, []);
```

---

### Example: setInterval

```jsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log("Running...");
  }, 1000);

  return () => {
    clearInterval(timer);
  };
}, []);
```

---

## 8️⃣ Common useEffect Examples

---

### 🔹 1. Fetching Data (API Calls)

```jsx
useEffect(() => {
  fetch("https://api.example.com/users")
    .then(res => res.json())
    .then(data => setUsers(data));
}, []);
```

📌 Runs once when component mounts  
📌 Fetching is a **side effect**

---

### 🔹 2. Listening to Window Events

```jsx

```

📌 Cleanup removes event listener

---

### 🔹 3. Subscriptions (Example)

`useEffect(() => {   const subscription = subscribeToService();    return () => {     subscription.unsubscribe();   }; }, []);`

---

## 9️⃣ Common useEffect Mistakes (Very Important)

❌ Forgetting dependency array  
❌ Wrong dependencies  
❌ Infinite loops  
❌ Updating state unnecessarily  
❌ Using async directly in useEffect

---

### ❌ Wrong async usage

`useEffect(async () => { // ❌   const data = await fetchData(); }, []);`

### ✅ Correct way

`useEffect(() => {   async function fetchData() {     const data = await fetchData();   }   fetchData(); }, []);`

---

## 🔟 When to Use useEffect (Golden Rule)

Use `useEffect` when:

- You need **external interaction**
    
- You need **side effects**
    
- You need **sync with browser / server**
    

❌ Don’t use `useEffect` for:

- Simple calculations
    
- Rendering logic
    

---

## 🧠 Mental Model (Beginner Friendly)

Think like this:

> “Render = What UI looks like”  
> “Effect = What happens outside UI”

---

## ✅ Summary

- Side effects ≠ rendering
    
- `useEffect` handles side effects
    
- Dependency array controls **when**
    
- Cleanup prevents memory leaks
    
- Used for APIs, events, timers