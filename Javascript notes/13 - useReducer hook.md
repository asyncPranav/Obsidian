
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

```jsx
import React, { useReducer } from "react";

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    case "reset":
      return initialState;
    default:
      return state;
  }
}

export default function App() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <h1>Count: {state.count}</h1>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
      <button onClick={() => dispatch({ type: "reset" })}>Reset</button>
    </div>
  );
}
```

---

### **4. Why Use `useReducer` Over `useState`?**

|**useState**|**useReducer**|
|---|---|
|Best for simple state (a few variables)|Best for complex state with many actions|
|Updates are inline|Updates are centralized in one reducer|
|Can be messy with multiple `setState` calls|Keeps logic organized in a single place|
|Example: `setCount(count + 1)`|Example: `dispatch({ type: "increment" })`|

---

### How this works step-by-step:

1. **Initial state** is `{ count: 0 }`.
    
2. When you click **+**, it runs:
    
    `dispatch({ type: "INCREMENT" })`
    
3. That sends `{ type: "INCREMENT" }` to the **reducer** function.
    
4. Reducer sees the type `"INCREMENT"` and returns:
    
    `{ count: state.count + 1 }`
    
5. React updates the state with the new object.
    

---

### Now, what is “payload”?

In the example above, the only info we sent was:

`{ type: "INCREMENT" }`

But sometimes we also need to send **extra data** along with the action.

For example, if we want to add a specific number instead of just `+1`:

`dispatch({ type: "INCREMENT_BY", payload: 5 })`

Here:

- `type` tells the reducer **what happened**
    
- `payload` is **extra information** needed to update the state
    

---

✅ If you want, next I can rewrite this counter so you **use payload** and see exactly why it’s useful. That will prepare you for the todo list example later.

Do you want me to do that?

##### You said:

yes

###### ChatGPT said:

Alright — let’s take our **counter** and make it smarter by adding **payload** so we can increase/decrease by _any number_ instead of just 1.

---

## `useReducer` Counter with Payload

`import React, { useReducer } from "react";  // Step 1: Initial state const initialState = { count: 0 };  // Step 2: Reducer function function reducer(state, action) {   switch (action.type) {     case "INCREMENT_BY":       return { count: state.count + action.payload };     case "DECREMENT_BY":       return { count: state.count - action.payload };     case "RESET":       return { count: 0 };     default:       return state;   } }  export default function App() {   const [state, dispatch] = useReducer(reducer, initialState);    return (     <div style={{ textAlign: "center" }}>       <h1>Count: {state.count}</h1>        {/* Increase by 1 */}       <button onClick={() => dispatch({ type: "INCREMENT_BY", payload: 1 })}>         +1       </button>        {/* Increase by 5 */}       <button onClick={() => dispatch({ type: "INCREMENT_BY", payload: 5 })}>         +5       </button>        {/* Decrease by 1 */}       <button onClick={() => dispatch({ type: "DECREMENT_BY", payload: 1 })}>         -1       </button>        {/* Decrease by 3 */}       <button onClick={() => dispatch({ type: "DECREMENT_BY", payload: 3 })}>         -3       </button>        <button onClick={() => dispatch({ type: "RESET" })}>         Reset       </button>     </div>   ); }`

---

### What’s happening with **payload** here:

1. `dispatch` sends an object like:
    
    `{ type: "INCREMENT_BY", payload: 5 }`
    
2. Reducer receives it as:
    
    `action.type // "INCREMENT_BY" action.payload // 5`
    
3. Then it uses `payload` to update the count dynamically:
    
    `return { count: state.count + action.payload }`
    

---

💡 **Key takeaway:**

- `type` → tells the reducer **what kind of update** to make
    
- `payload` → carries **extra information needed for that update**

---

### **5. `payload` Example**

```jsx
function reducer(state, action) {
  switch (action.type) {
    case "add":
      return { items: [...state.items, action.payload] };
    case "remove":
      return { items: state.items.filter((_, i) => i !== action.payload) };
    default:
      return state;
  }
}

const initialState = { items: [] };
```

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