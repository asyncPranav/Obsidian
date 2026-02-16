
---

# 📘 Chapter: useReducer Hook – Detailed Notes

---

## 1️⃣ What is `useReducer`?

- `useReducer` is a **React hook** for managing state.
    
- It is an **alternative to `useState`**.
    
- Useful when state logic is **complex**, depends on **previous state**, or involves **multiple sub-values**.
    

**Analogy:**

- If `useState` is like a **single light switch**,
    
- `useReducer` is like a **control panel** with multiple buttons controlling multiple lights.
    

---

## 2️⃣ Syntax of `useReducer`

const [state, dispatch] = useReducer(reducer, initialState);

**Where:**

- `state` → current state value
    
- `dispatch` → function to send **actions**
    
- `reducer` → function that determines **how state changes**
    
- `initialState` → starting state
    

---

### 🔹 Reducer Function

```js
function reducer(state, action) {  
  switch(action.type) {  
    case "INCREMENT":  
      return { count: state.count + 1 };  
    case "DECREMENT":  
      return { count: state.count - 1 };  
    default:  
      return state;  
  }  
}
```

---

## 3️⃣ Basic Example – Counter

import { useReducer } from "react";  
  
const initialState = { count: 0 };  
  
function reducer(state, action) {  
  switch(action.type) {  
    case "INCREMENT":  
      return { count: state.count + 1 };  
    case "DECREMENT":  
      return { count: state.count - 1 };  
    case "RESET":  
      return { count: 0 };  
    default:  
      return state;  
  }  
}  
  
function Counter() {  
  const [state, dispatch] = useReducer(reducer, initialState);  
  
  return (  
    <div>  
      <h1>Count: {state.count}</h1>  
      <button onClick={() => dispatch({ type: "INCREMENT" })}>+</button>  
      <button onClick={() => dispatch({ type: "DECREMENT" })}>-</button>  
      <button onClick={() => dispatch({ type: "RESET" })}>Reset</button>  
    </div>  
  );  
}  
  
export default Counter;

**Explanation:**

1. `state` → `{ count: 0 }`
    
2. `dispatch({ type: "INCREMENT" })` → calls reducer
    
3. Reducer returns **new state** → triggers re-render
    

---

## 4️⃣ Why useReducer over useState?

|useState|useReducer|
|---|---|
|Simple state|Complex state (objects, arrays)|
|Multiple `useState` for same component|Single state object with many properties|
|Easy to use|Better for predictable state updates|
|Can cause multiple re-renders|Centralized logic in reducer|
|Example: toggle a boolean|Example: counter, todo list, form|

✅ Rule of thumb: **If state is complex or depends on previous state → useReducer**

---

## 5️⃣ useReducer with Object State

const initialState = { count: 0, step: 1 };  
  
function reducer(state, action) {  
  switch(action.type) {  
    case "INCREMENT":  
      return { ...state, count: state.count + state.step };  
    case "DECREMENT":  
      return { ...state, count: state.count - state.step };  
    case "SET_STEP":  
      return { ...state, step: action.payload };  
    default:  
      return state;  
  }  
}

**Usage:**

<input  
  type="number"  
  value={state.step}  
  onChange={(e) => dispatch({ type: "SET_STEP", payload: Number(e.target.value) })}  
/>

- `payload` → optional data passed to reducer
    

---

## 6️⃣ useReducer with Arrays – Todo Example

const initialState = [];  
  
function reducer(state, action) {  
  switch(action.type) {  
    case "ADD_TODO":  
      return [...state, { id: Date.now(), text: action.payload, completed: false }];  
    case "TOGGLE_TODO":  
      return state.map(todo =>  
        todo.id === action.payload ? { ...todo, completed: !todo.completed } : todo  
      );  
    case "DELETE_TODO":  
      return state.filter(todo => todo.id !== action.payload);  
    default:  
      return state;  
  }  
}

**Usage:**

const [todos, dispatch] = useReducer(reducer, initialState);  
  
// Add todo  
dispatch({ type: "ADD_TODO", payload: "Learn useReducer" });  
  
// Toggle todo  
dispatch({ type: "TOGGLE_TODO", payload: todoId });  
  
// Delete todo  
dispatch({ type: "DELETE_TODO", payload: todoId });

✅ Perfect for **complex CRUD operations** in React

---

## 7️⃣ useReducer + useContext

- Great combination for **global state management**
    
- Example: Auth, Theme, Cart, Todo App
    

// AuthContext.js  
import { createContext, useReducer } from "react";  
  
export const AuthContext = createContext();  
  
const initialState = { user: null };  
  
function reducer(state, action) {  
  switch(action.type) {  
    case "LOGIN":  
      return { user: action.payload };  
    case "LOGOUT":  
      return { user: null };  
    default:  
      return state;  
  }  
}  
  
export const AuthProvider = ({ children }) => {  
  const [state, dispatch] = useReducer(reducer, initialState);  
  return <AuthContext.Provider value={{ state, dispatch }}>{children}</AuthContext.Provider>;  
};

- Any component can **dispatch LOGIN/LOGOUT actions globally**
    

---

## 8️⃣ Why useReducer is better for large apps

- Centralized state updates
    
- Predictable state changes
    
- Debuggable via action logs
    
- Compatible with middleware like **Redux**
    
- Works well with **complex forms, arrays, nested objects**
    

---

## 9️⃣ Common Mistakes

❌ Mutating state directly inside reducer → always return **new state**  
❌ Forgetting `default` case in reducer → may return `undefined`  
❌ Using `useReducer` for **very simple states** → overkill  
❌ Calling `dispatch` inside reducer → infinite loop

---

## 🔟 Mental Model

> **Reducer = Manager**
> 
> - Takes current state + action
>     
> - Decides new state
>     
> - Returns it
>     
> - Dispatch → tells manager to act
>     

---

## ✅ Summary

- `useReducer` = useState alternative for **complex state**
    
- Syntax: `[state, dispatch] = useReducer(reducer, initialState)`
    
- Reducer = function(state, action) → returns new state
    
- Handles **objects, arrays, forms, todos**
    
- Combines very well with **Context API**
    
- Actions can have `type` + optional `payload`