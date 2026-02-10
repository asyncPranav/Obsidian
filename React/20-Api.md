
---

# 📘 Chapter: Fetching Data from APIs

---

## 1️⃣ What is an API?

### 🔹 Definition

**API (Application Programming Interface)** allows one application to communicate with another and exchange data.

In React:

- Frontend → requests data
    
- Backend / Server → responds with data (mostly JSON)
    

---

### 🔹 Real-Life Example 🧠

📱 Food App  
➡ App asks: _“Give me restaurant list”_  
➡ Server replies: _“Here is the data”_

---

## 2️⃣ Why Fetch Data in React?

✔ Display dynamic data  
✔ Connect frontend with backend  
✔ Real-time updates  
✔ User-based content

---

## 3️⃣ Where Do We Fetch Data in React?

👉 Inside **`useEffect()`**

### Why?

- Fetching is a **side effect**
    
- We don’t want infinite loops
    
- Runs after component render
    

---

## 4️⃣ Using `fetch()` in `useEffect()`

---

### 🔹 Basic Syntax of fetch()

```jsx
fetch("https://api.example.com/users")
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

---

### 🔹 Fetch inside useEffect

```jsx
import { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("https://api.example.com/users")
      .then(res => res.json())
      .then(data => setUsers(data))
      .catch(err => console.log(err));
  }, []);

  return <div>Users Component</div>;
}
```

📌 Empty dependency array `[]` → runs **only once**

---

## 5️⃣ Displaying Fetched Data

---

### 🔹 Mapping Over Data

```j
```

📌 `key` is mandatory  
📌 Avoid using index as key

---

### 🔹 Conditional Rendering

`{users.length === 0 ? <p>No Data</p> : users.map(...)}`

---

## 6️⃣ Loading and Error States (VERY IMPORTANT)

---

### 🔹 Why Needed?

✔ Better UX  
✔ Avoid blank screen  
✔ Handle failures gracefully

---

### 🔹 State Setup

`const [loading, setLoading] = useState(true); const [error, setError] = useState(null);`

---

### 🔹 Full Example

`useEffect(() => {   setLoading(true);   fetch("https://api.example.com/users")     .then(res => {       if (!res.ok) throw new Error("Failed to fetch");       return res.json();     })     .then(data => setUsers(data))     .catch(err => setError(err.message))     .finally(() => setLoading(false)); }, []);`

---

### 🔹 UI Handling

`if (loading) return <h2>Loading...</h2>; if (error) return <h2>{error}</h2>;`

---

## 7️⃣ Axios vs Fetch

---

### 🔹 What is Axios?

Axios is a **third-party HTTP client library**.

`npm install axios`

---

### 🔹 Axios Example

`import axios from "axios";  useEffect(() => {   axios.get("https://api.example.com/users")     .then(res => setUsers(res.data))     .catch(err => console.log(err)); }, []);`

---

### 🔹 Comparison Table

|Feature|Fetch|Axios|
|---|---|---|
|Built-in|Yes|No|
|JSON conversion|Manual|Automatic|
|Error handling|Manual|Better|
|Request cancel|Hard|Easy|
|Interceptors|❌|✅|

📌 Axios is preferred in **production apps**

---

## 8️⃣ POST Requests to APIs

---

### 🔹 Using fetch() – POST

`fetch("https://api.example.com/users", {   method: "POST",   headers: {     "Content-Type": "application/json"   },   body: JSON.stringify({     name: "Rahul",     email: "rahul@gmail.com"   }) });`

---

### 🔹 Using Axios – POST

`axios.post("https://api.example.com/users", {   name: "Rahul",   email: "rahul@gmail.com" });`

📌 Axios auto converts object → JSON

---

## 9️⃣ Handling Forms + POST

`function handleSubmit(e) {   e.preventDefault();    axios.post("/api/users", formData)     .then(() => alert("User Created")); }`

---

## 🔟 Handling Pagination (IMPORTANT)

---

### 🔹 What is Pagination?

Fetching data **page by page** instead of all at once.

Example:

`?page=1 ?page=2`

---

### 🔹 State Setup

`const [page, setPage] = useState(1);`

---

### 🔹 Fetch with Pagination

``useEffect(() => {   fetch(`https://api.example.com/posts?page=${page}`)     .then(res => res.json())     .then(data => setPosts(data)); }, [page]);``

---

### 🔹 Pagination Buttons

`<button onClick={() => setPage(page - 1)} disabled={page === 1}>   Prev </button>  <button onClick={() => setPage(page + 1)}>   Next </button>`

---

## 1️⃣1️⃣ Infinite Scroll (Concept)

Instead of buttons:

- Load data when user scrolls
    
- Common in social media
    

📌 Uses `IntersectionObserver`

---

## 1️⃣2️⃣ Cleanup & Abort Fetch

---

### 🔹 Problem

Component unmounts before fetch completes → memory leak

---

### 🔹 Solution: AbortController

`useEffect(() => {   const controller = new AbortController();    fetch(url, { signal: controller.signal })     .then(res => res.json())     .then(data => setData(data));    return () => controller.abort(); }, []);`

---

## 1️⃣3️⃣ Best Practices ⭐

✔ Always handle loading & error  
✔ Use Axios for complex apps  
✔ Keep API logic separate  
✔ Use environment variables  
✔ Avoid fetching in render

---

## 1️⃣4️⃣ Common Mistakes

❌ Missing dependency array  
❌ Infinite loop  
❌ No error handling  
❌ Fetching inside JSX

---

## 🧠 Mental Model

> React renders UI  
> API gives data  
> State connects both

---

## ✅ Summary

- Fetch inside `useEffect`
    
- Display using map
    
- Handle loading & errors
    
- Use Axios for cleaner code
    
- Implement pagination properly