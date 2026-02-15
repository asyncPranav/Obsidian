
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

function useCustomHook() {  
  const [state, setState] = useState(initialValue);  
  
  // some logic  
  return [state, setState]; // or return object  
}

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

import { useState } from "react";  
  
function useCounter(initialValue = 0) {  
  const [count, setCount] = useState(initialValue);  
  
  const increment = () => setCount(prev => prev + 1);  
  const decrement = () => setCount(prev => prev - 1);  
  const reset = () => setCount(initialValue);  
  
  return { count, increment, decrement, reset };  
}  
  
export default useCounter;

---

### 🔹 Using the Custom Hook

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

✅ Clean, reusable, and state is encapsulated

---

## 5️⃣ Reusing Logic Across Components

Suppose multiple components need **form handling logic**:

- Without custom hook → duplicate code
    
- With custom hook → reuse
    

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

---

### 🔹 Usage Example

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

📌 Now, `useForm` can be used in **SignupForm, ContactForm, etc.**

---

## 6️⃣ Custom Hooks + useEffect (Fetching Data Example)

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

**Usage:**

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