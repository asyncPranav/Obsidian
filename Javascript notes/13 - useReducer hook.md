
---

## **`useReducer` – The Deep Dive**

`useReducer` is an alternative to `useState` when your state logic:

- Is **more complex** than a few variables
    
- Has **multiple actions** that change it
    
- Depends on the **previous state**
    
- Is easier to manage with a **centralized update function (reducer)**
    

---

### **1. Basic Syntax**

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

- **`reducer`** → function that decides how to update state.
    
- **`initialState`** → starting value for your state.
    
- **`state`** → current state value.
    
- **`dispatch`** → function to send actions to the reducer.
    

---

### **2. The Reducer Function**

```jsx
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return { count: 0 };
    default:
      return state; // always return state for unknown actions
  }
}
```

- `state` → old state
    
- `action` → object describing what happened (usually `{ type: string, payload?: any }`)
    

---

### **3. Example – Counter**

`import React, { useReducer } from "react";  const initialState = { count: 0 };  function reducer(state, action) {   switch (action.type) {     case "increment":       return { count: state.count + 1 };     case "decrement":       return { count: state.count - 1 };     case "reset":       return initialState;     default:       return state;   } }  export default function App() {   const [state, dispatch] = useReducer(reducer, initialState);    return (     <div>       <h1>Count: {state.count}</h1>       <button onClick={() => dispatch({ type: "increment" })}>+</button>       <button onClick={() => dispatch({ type: "decrement" })}>-</button>       <button onClick={() => dispatch({ type: "reset" })}>Reset</button>     </div>   ); }`

---

### **4. Why Use `useReducer` Over `useState`?**

|**useState**|**useReducer**|
|---|---|
|Best for simple state (a few variables)|Best for complex state with many actions|
|Updates are inline|Updates are centralized in one reducer|
|Can be messy with multiple `setState` calls|Keeps logic organized in a single place|
|Example: `setCount(count + 1)`|Example: `dispatch({ type: "increment" })`|

---

### **5. `payload` Example**

`function reducer(state, action) {   switch (action.type) {     case "add":       return { items: [...state.items, action.payload] };     case "remove":       return { items: state.items.filter((_, i) => i !== action.payload) };     default:       return state;   } }  const initialState = { items: [] };`

- `payload` carries extra data needed for the update.
    

---

### **6. Best Practices**

- Always return **new state** (don’t mutate the old one).
    
- Always handle unknown actions with `default`.
    
- Keep reducer functions **pure** (no API calls, DOM changes inside).
    

---

### **7. How It Relates to Redux**

- Redux also has **reducers** and **dispatch**.
    
- Redux just moves them to a **global store** so all components can use them.
    
- `useReducer` is **local state** (inside one component unless you wrap it with `useContext`).
    

---

If you’re ready, the **next step** would be:

1. Wrap `useReducer` inside `useContext` to make a **global store**.
    
2. Then switch to **Redux Toolkit**, which automates a lot of boilerplate.
    

Do you want me to **immediately show the `useReducer + useContext` global state pattern** so it feels like a mini Redux before we jump to Redux Toolkit? That’s where the "aha" moment happens.