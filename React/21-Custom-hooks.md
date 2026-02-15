
---

# 📘 Chapter 14: Custom Hooks – Detailed Notes

---

## 1️⃣ What are Custom Hooks?

### 🔹 Definition

A **Custom Hook** is a **JavaScript function** that:

- Uses **built-in React hooks** (like `useState`, `useEffect`, `useContext`)
    
- Encapsulates **reusable logic**
    
- Follows the naming convention: **starts with `use`**
    

---

### 🔹 Syntax

```js
function useCustomHook() {
  const [state, setState] = useState(initialValue);

  // some logic
  return [state, setState]; // or return object
}
```

---

### 🔹 Real-Life Analogy 🧠

Imagine you always **write logic to fetch data** in multiple components.  
Instead of repeating it:

- You put it in a function → **Custom Hook**
    
- Reuse anywhere → `const data = useFetch(url)`
    

✅ Cleaner, DRY (Don’t Repeat Yourself)

---

## 2️⃣ Why Custom Hooks?

- **Reusability**: Avoid repeating logic in multiple components
    
- **Readability**: Keep component files short & clean
    
- **Separation of concerns**: Logic moves out of UI components
    
- **Maintainability**: One place to fix logic
    

---

## 3️⃣ Rules of Custom Hooks (React Hooks Rules Recap)

1. **Must start with `use`**
    
    - Example: `useForm`, `useFetch`
        
    - Why? React detects them and tracks state internally
        
2. **Only call hooks at the top level**
    
    - Don’t call inside loops, conditions, or nested functions
        
3. **Only call hooks from React function components or custom hooks**
    

---

## 4️⃣ How to Create a Simple Custom Hook

### 🔹 Example: `useCounter`

```js
import { useState } from "react";

function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);

  const increment = () => setCount(prev => prev + 1);
  const decrement = () => setCount(prev => prev - 1);
  const reset = () => setCount(initialValue);

  return { count, increment, decrement, reset };
}

export default useCounter;
```

---

### 🔹 Using the Custom Hook

```js
import useCounter from "./useCounter";

function CounterComponent() {
  const { count, increment, decrement, reset } = useCounter(5);

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={increment}> + </button>
      <button onClick={decrement}> - </button>
      <button onClick={reset}> Reset </button>
    </div>
  );
}
```

✅ Clean, reusable, and state is encapsulated

---

## 5️⃣ Reusing Logic Across Components

Suppose multiple components need **form handling logic**:

- Without custom hook → duplicate code
    
- With custom hook → reuse
    

```js
// useForm.js
import { useState } from "react";

function useForm(initialValues) {
  const [formData, setFormData] = useState(initialValues);

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const resetForm = () => setFormData(initialValues);

  return { formData, handleChange, resetForm };
}

export default useForm;
```

---

### 🔹 Usage Example

```js
import useForm from "./useForm";

function LoginForm() {
  const { formData, handleChange, resetForm } = useForm({
    username: "",
    password: ""
  });

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(formData);
    resetForm();
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        name="username"
        value={formData.username}
        onChange={handleChange}
      />
      <input
        type="password"
        name="password"
        value={formData.password}
        onChange={handleChange}
      />
      <button type="submit">Login</button>
    </form>
  );
}
```

📌 Now, `useForm` can be used in **SignupForm, ContactForm, etc.**

---

## 6️⃣ Custom Hooks + useEffect (Fetching Data Example)

```js
import { useState, useEffect } from "react";

function useFetch(url) {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    setLoading(true);
    fetch(url)
      .then(res => res.json())
      .then(json => setData(json))
      .catch(err => setError(err))
      .finally(() => setLoading(false));
  }, [url]);

  return { data, loading, error };
}

export default useFetch;
```

**Usage:**

```js
function UsersList() {
  const { data: users, loading, error } = useFetch("https://jsonplaceholder.typicode.com/users");

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error!</p>;

  return (
    <ul>
      {users.map(u => <li key={u.id}>{u.name}</li>)}
    </ul>
  );
}
```

✅ All fetching logic is **encapsulated**, component stays clean

---

## 7️⃣ Benefits of Custom Hooks

- Reusable and composable
    
- Keeps UI & logic separate
    
- Works with any hook (`useState`, `useEffect`, `useContext`)
    
- Simplifies code
    
- Reduces prop drilling
    

---

## 8️⃣ Advanced Topics

1️⃣ **Multiple Custom Hooks Together**

- Example: `useForm` + `useFetch`
    

2️⃣ **Custom Hook Returning Functions**

- Example: `useCounter` returns `increment`, `decrement`
    

3️⃣ **Sharing Global State with Custom Hook + Context**

- Example: `useAuth` → accesses user context and login/logout functions
    

---

## 9️⃣ Common Beginner Mistakes

❌ Naming hook without `use` → React won’t track it  
❌ Calling hooks conditionally → breaks rules  
❌ Keeping too much logic inside component instead of hook  
❌ Forgetting to return all needed values from hook

---

## 🔟 Mental Model (Very Important)

> **Custom Hook = Mini component for logic**
> 
> - Doesn’t return JSX
>     
> - Returns **state, functions, or any value**
>     
> - Can call other hooks inside it
>     

---

## ✅ Summary

- Custom Hooks start with `use`
    
- Encapsulate reusable logic
    
- Improve readability & maintainability
    
- Can contain state, effects, or context
    
- Solve repetitive logic problem
    
- Perfect for form handling, fetch requests, counters, etc.
    

---

If you want, I can **next create a full practical project** combining:

- `useState`
    
- `useEffect`
    
- `Custom Hooks`
    
- `Context API`
    

So you can **practice real-world reusable code** 🚀


---

Perfect! 🚀 Let’s build a **full practical project** that combines **useState, useEffect, Custom Hooks, and Context API**.

We’ll make a **“User Management Dashboard”** (simple, text-based UI for clarity) where you can:

- Fetch users from an API
    
- Add / Delete users locally
    
- Update user status
    
- Share theme or authentication state globally via Context
    
- Encapsulate logic in **Custom Hooks**
    

---

# 📝 Project Plan: User Management Dashboard

---

## **1️⃣ Folder Structure**

```js
src/
├─ App.jsx
├─ index.jsx
├─ context/
│   └─ ThemeContext.jsx
├─ hooks/
│   ├─ useUsers.jsx
│   └─ useForm.jsx
├─ components/
│   ├─ UserList.jsx
│   ├─ UserForm.jsx
│   └─ Header.jsx
```

---

## **2️⃣ Context – Theme / Auth**

```js
// src/context/ThemeContext.jsx
import { createContext, useState } from "react";

export const ThemeContext = createContext();

export const ThemeProvider = ({ children }) => {
  const [darkMode, setDarkMode] = useState(true);

  const toggleTheme = () => setDarkMode(prev => !prev);

  return (
    <ThemeContext.Provider value={{ darkMode, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};
```

- Global theme available to all components
    
- Avoids passing `darkMode` as props everywhere
    

---

## **3️⃣ Custom Hooks**

### a) useUsers – Fetch + Manage Users

```js
// src/hooks/useUsers.jsx
import { useState, useEffect } from "react";

export const useUsers = () => {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Fetch users
  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then(res => res.json())
      .then(data => setUsers(data))
      .catch(err => setError(err.message))
      .finally(() => setLoading(false));
  }, []);

  const addUser = (user) => setUsers(prev => [...prev, user]);
  const deleteUser = (id) => setUsers(prev => prev.filter(u => u.id !== id));

  return { users, loading, error, addUser, deleteUser };
};
```

✅ Encapsulates **all user logic**

---

### b) useForm – Form Handling

```js
// src/hooks/useForm.jsx
import { useState } from "react";

export const useForm = (initialValues) => {
  const [formData, setFormData] = useState(initialValues);

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const resetForm = () => setFormData(initialValues);

  return { formData, handleChange, resetForm };
};
```

---

## **4️⃣ Components**

### a) Header.jsx – Theme Toggle

```js
import { useContext } from "react";
import { ThemeContext } from "../context/ThemeContext";

export const Header = () => {
  const { darkMode, toggleTheme } = useContext(ThemeContext);

  return (
    <header style={{ background: darkMode ? "#333" : "#eee", color: darkMode ? "#fff" : "#000", padding: "1rem" }}>
      <h1>User Dashboard</h1>
      <button onClick={toggleTheme}>Toggle Theme</button>
    </header>
  );
};
```

---

### b) UserForm.jsx – Add User

import { useForm } from "../hooks/useForm";  
  
export const UserForm = ({ addUser }) => {  
  const { formData, handleChange, resetForm } = useForm({ name: "", email: "" });  
  
  const handleSubmit = (e) => {  
    e.preventDefault();  
    addUser({ ...formData, id: Date.now() });  
    resetForm();  
  };  
  
  return (  
    <form onSubmit={handleSubmit}>  
      <input name="name" placeholder="Name" value={formData.name} onChange={handleChange} required />  
      <input name="email" placeholder="Email" value={formData.email} onChange={handleChange} required />  
      <button type="submit">Add User</button>  
    </form>  
  );  
};

---

### c) UserList.jsx – Display + Delete

export const UserList = ({ users, deleteUser }) => {  
  return (  
    <ul>  
      {users.map(u => (  
        <li key={u.id}>  
          {u.name} - {u.email}  
          <button onClick={() => deleteUser(u.id)}>Delete</button>  
        </li>  
      ))}  
    </ul>  
  );  
};

---

## **5️⃣ App.jsx – Putting Everything Together**

import { ThemeProvider } from "./context/ThemeContext";  
import { useUsers } from "./hooks/useUsers";  
import { Header } from "./components/Header";  
import { UserForm } from "./components/UserForm";  
import { UserList } from "./components/UserList";  
  
function App() {  
  const { users, loading, error, addUser, deleteUser } = useUsers();  
  
  return (  
    <ThemeProvider>  
      <Header />  
      <UserForm addUser={addUser} />  
      {loading && <p>Loading...</p>}  
      {error && <p>Error: {error}</p>}  
      <UserList users={users} deleteUser={deleteUser} />  
    </ThemeProvider>  
  );  
}  
  
export default App;

---

## **6️⃣ Key Features Implemented**

- `useState` → state management for users and forms
    
- `useEffect` → fetching data on mount
    
- **Custom Hooks** → `useUsers` & `useForm`
    
- **Context API** → global theme
    
- Add/Delete user functionality
    
- Loading/Error handling
    

---

## ✅ Learning Outcomes

- You see **Custom Hooks + Context + State + Effects** in action
    
- UI logic and data logic are **separated**
    
- Reusability and scalability are enforced