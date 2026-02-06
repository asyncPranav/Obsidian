
---


# 📘 Chapter: Context API (Very Detailed Notes)

---

## 1️⃣ Why Context API Exists (Problem First)

### 🔴 Problem: Prop Drilling

```jsx
App
 └── Layout
      └── Navbar
           └── Profile
                └── UserName
```

If `user` is in `App`:

```jsx
<App user={user}>
  <Layout user={user}>
    <Navbar user={user}>
      <Profile user={user}>
        <UserName user={user} />
```

❌ Ugly  
❌ Hard to maintain  
❌ Unnecessary passing

This problem = **Prop Drilling**

---

## 2️⃣ What is Context API?

### 🔹 Simple Definition

**Context API** allows you to:

> Share data globally across components **without passing props manually** at every level.

---

### 🔹 Real-Life Analogy 🧠

Think of Context like:

- Wi-Fi in a building
    
- Any device can connect
    
- No need to pass cables room to room
    

---

## 3️⃣ When Should You Use Context?

✔ User authentication  
✔ Theme (dark/light)  
✔ Language (i18n)  
✔ Global settings  
✔ Cart data

❌ Not for every small state  
❌ Not for local component data

---

## 4️⃣ Core Concepts of Context API

There are **3 main parts**:

|Part|Role|
|---|---|
|`createContext`|Creates context|
|`Provider`|Supplies data|
|`Consumer / useContext`|Uses data|

---

## 5️⃣ Step 1: Creating Context

```jsx
import { createContext } from "react";

const UserContext = createContext();
```

📌 This creates a **Context object**  
📌 Default value can be passed (optional)

```jsx
const UserContext = createContext("Guest");8 
```

---

## 6️⃣ Step 2: Provider (VERY IMPORTANT)

### 🔹 What is Provider?

Provider:

- Wraps components
    
- Supplies value to all children
    

---

### 🔹 Basic Example

`<UserContext.Provider value="Rahul">   <App /> </UserContext.Provider>`

📌 Every child of `<App />` can access `"Rahul"`

---

### 🔹 Provider with State (Most Common)

`function UserProvider({ children }) {   const [user, setUser] = useState("Rahul");    return (     <UserContext.Provider value={{ user, setUser }}>       {children}     </UserContext.Provider>   ); }`

📌 Context can share **data + functions**

---

## 7️⃣ Step 3: Consuming Context

### 🔹 Using `useContext` (Modern Way)

`import { useContext } from "react";  const { user, setUser } = useContext(UserContext);`

✔ Clean  
✔ Easy  
✔ Recommended

---

### 🔹 Old Way (Consumer Component)

`<UserContext.Consumer>   {(value) => <h1>{value}</h1>} </UserContext.Consumer>`

❌ Rarely used now

---

## 8️⃣ Complete Working Example (End-to-End)

---

### 🟢 Context File

`// UserContext.js import { createContext } from "react";  export const UserContext = createContext();`

---

### 🟢 Provider

`export function UserProvider({ children }) {   const [user, setUser] = useState("Guest");    return (     <UserContext.Provider value={{ user, setUser }}>       {children}     </UserContext.Provider>   ); }`

---

### 🟢 Wrap App

`<UserProvider>   <App /> </UserProvider>`

---

### 🟢 Consume Anywhere

`function Profile() {   const { user, setUser } = useContext(UserContext);    return (     <>       <h2>{user}</h2>       <button onClick={() => setUser("Admin")}>Login</button>     </>   ); }`

---

## 9️⃣ How Context Solves Prop Drilling

Without context:

`App → A → B → C → D`

With context:

`App (Provider)    ↓ D (useContext)`

📌 Direct access  
📌 No unnecessary props

---

## 🔟 Context Update & Re-rendering

### 🔹 Important Rule

When context value changes:

> All consuming components re-render

---

### 🔹 Performance Consideration

Avoid:

`<UserContext.Provider value={{ user, theme, settings }}>`

Better:

- Split contexts
    
- Keep values minimal
    

---

## 1️⃣1️⃣ Multiple Contexts

`<AuthContext.Provider>   <ThemeContext.Provider>     <App />   </ThemeContext.Provider> </AuthContext.Provider>`

✔ Separation of concerns  
✔ Better performance

---

## 1️⃣2️⃣ Context + useEffect Example

`useEffect(() => {   if (user) {     fetchUserData(user);   } }, [user]);`

📌 Context value used in effects normally

---

## 1️⃣3️⃣ Common Beginner Mistakes

❌ Using context everywhere  
❌ Forgetting Provider  
❌ Mutating context value  
❌ Storing derived data

---

## 🧠 Golden Mental Model

> Props = **local wires**  
> Context = **global broadcast**

---

## 1️⃣4️⃣ Context vs Redux (Quick Compare)

|Feature|Context|Redux|
|---|---|---|
|Built-in|Yes|No|
|Boilerplate|Low|High|
|Best for|Small–Medium apps|Large apps|

---

## 1️⃣5️⃣ When NOT to Use Context

❌ High-frequency updates  
❌ Animation states  
❌ Very local UI state

---

## ✅ Summary

- Context solves prop drilling
    
- Provides global data access
    
- Uses Provider + useContext
    
- Keep context focused
    
- Combine with custom hooks