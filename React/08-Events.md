
---

# 📘 Chapter 6: Events & Handling User Actions in React

---

## 1️⃣ What Are React Events?

### Simple Definition

**React events** are used to **handle user actions**, such as:

- Clicking a button
    
- Typing in an input
    
- Submitting a form
    
- Hovering over elements
    

React events are **similar to JavaScript events**, but with some differences.

---

## 2️⃣ React Events vs JavaScript Events

|JavaScript|React|
|---|---|
|`onclick`|`onClick`|
|`onchange`|`onChange`|
|lowercase|camelCase|
|`addEventListener`|JSX event props|

📌 **React uses camelCase event names**

---

## 3️⃣ Basic `onClick` Event

### Example

```jsx
function App() {
  function handleClick() {
    console.log("Button clicked");
  }

  return <button onClick={handleClick}>Click Me</button>;
}
```

📌 **Do NOT call the function**  
❌ `onClick={handleClick()}`  
✅ `onClick={handleClick}`

---

## 4️⃣ Inline Event Handlers

### Example

```jsx
<button onClick={() => console.log("Clicked")}>
  Click
</button>
```

✔ Useful for small logic  
❌ Avoid for heavy logic

---

## 5️⃣ Passing Functions as Props (VERY IMPORTANT ⭐)

### Parent

```jsx
function Parent() {
  function sayHello() {
    alert("Hello from Parent");
  }

  return <Child onGreet={sayHello} />;
}
```

### Child

```jsx
function Child({ onGreet }) {
  return <button onClick={onGreet}>Greet</button>;
}
```

📌 Used for **child → parent communication**

---

## 6️⃣ Event Object (`event` or `e`)

### What is Event Object?

React automatically passes an **event object** containing information about the event.

### Example

```jsx
function handleClick(e) {
  console.log(e);
}
```

📌 Common properties:

- `e.target`
    
- `e.type`
    
- `e.preventDefault()`
    

---

## 7️⃣ `onChange` Event (Inputs)

### Example

```jsx
function App() {
  function handleChange(e) {
    console.log(e.target.value);
  }

  return <input onChange={handleChange} />;
}
```

📌 `onChange` fires on **every keystroke**

---

## 8️⃣ Controlled Components (Extra Topic ⭐)

React controls input value using state.

```jsx
const [text, setText] = useState("");

<input
  value={text}
  onChange={(e) => setText(e.target.value)}
/>
```

✔ React state is the **single source of truth**

---

## 9️⃣ Event Binding in Functional Components

In functional components:

- No need for `.bind(this)`
    
- Arrow functions auto bind
    

```jsx
<button onClick={handleClick}>Click</button>
```

📌 Binding issues exist only in class components

---

## 🔟 Passing Arguments to Event Handlers

### ❌ Wrong

`<button onClick={handleClick(5)}>Click</button>`

### ✅ Correct

`<button onClick={() => handleClick(5)}>   Click </button>`

---

## 1️⃣1️⃣ Inline vs Function Handlers

|Inline|Function|
|---|---|
|Quick logic|Reusable|
|Less readable if large|Clean code|
|Arrow function|Named function|

📌 Best practice:

- Small → inline
    
- Large → separate function
    

---

## 1️⃣2️⃣ Common React Event Mistakes ❌

1. Calling function instead of passing reference
    
2. Using lowercase event names
    
3. Forgetting arrow function when passing args
    
4. Mutating state inside event
    
5. Using `this` in functional components
    

---

## 1️⃣3️⃣ Prevent Default Behavior (Extra Topic)

### Example (Form)

`function handleSubmit(e) {   e.preventDefault();   console.log("Form submitted"); }`

📌 Prevents page reload

---

## 1️⃣4️⃣ Synthetic Events (Extra Topic ⭐)

React wraps native browser events into **Synthetic Events**.

Benefits:

- Cross-browser consistency
    
- Performance optimization
    

📌 You still use them like normal events.

---

## 1️⃣5️⃣ Event Flow (Extra Topic)

- React events **bubble up**
    
- Same as DOM bubbling
    

`<div onClick={() => console.log("Parent")}>   <button onClick={() => console.log("Child")}>     Click   </button> </div>`

---

## 🧠 Mental Model

> **Event → Handler → State Update → Re-render**

---

## ✅ Chapter 6 Summary

✔ React uses camelCase events  
✔ onClick & onChange are most common  
✔ Functions are passed, not called  
✔ Event object gives event details  
✔ Inline vs function handlers matter  
✔ Controlled inputs use state