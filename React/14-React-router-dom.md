
---


# 📘 Chapter 12: React Router (Very Detailed Notes)

> **Version used:** React Router DOM v6+ (modern & recommended)

---

## 1️⃣ What is React Router?

### 🔹 Problem in Normal Websites (Multi-Page Apps)

In traditional websites:

- Clicking a link loads a **new HTML page**
    
- Browser sends request to server
    
- Full page reload happens
    

This is called **Multi Page Application (MPA)**.

---

### 🔹 React is a SPA (Single Page Application)

React apps:

- Load **only one HTML file** (`index.html`)
    
- Page content changes **without reload**
    
- Faster and smoother UX
    

👉 But browser URL still needs to change.

This is where **React Router** comes in.

---

### 🔹 Definition

**React Router** is a library that enables:

- Page navigation **without page reload**
    
- URL-based component rendering
    
- SPA-style routing
    

---

## 2️⃣ SPA Routing Concept (Very Important)

### 🔹 How SPA Routing Works

1. User clicks a link
    
2. URL changes
    
3. React Router detects the change
    
4. React renders a different component
    
5. **No page reload**
    

📌 Browser thinks page changed  
📌 Actually React swapped components

---

## 3️⃣ Installing React Router

`npm install react-router-dom`

📌 This installs:

- `BrowserRouter`
    
- `Routes`
    
- `Route`
    
- `Link`, `NavLink`
    
- Hooks like `useParams`, `useNavigate`
    

---

## 4️⃣ BrowserRouter (Root of Routing)

### 🔹 What is BrowserRouter?

- Uses **HTML5 history API**
    
- Listens to URL changes
    
- Must wrap your entire app
    

---

### 🔹 Setup (Main Entry)

`import { BrowserRouter } from "react-router-dom";  ReactDOM.createRoot(document.getElementById("root")).render(   <BrowserRouter>     <App />   </BrowserRouter> );`

📌 Without `BrowserRouter`, routing **will not work**

---

## 5️⃣ Routes & Route

### 🔹 Routes

- Container for all routes
    
- Replaces old `<Switch>` (v5)
    

### 🔹 Route

- Defines **path → component mapping**
    

---

### 🔹 Basic Example

`import { Routes, Route } from "react-router-dom";  function App() {   return (     <Routes>       <Route path="/" element={<Home />} />       <Route path="/about" element={<About />} />       <Route path="/contact" element={<Contact />} />     </Routes>   ); }`

📌 `element` receives JSX  
📌 React renders component based on URL

---

## 6️⃣ Link vs `<a>` Tag (Very Important)

### ❌ `<a>` Tag (Wrong for React)

`<a href="/about">About</a>`

❌ Causes full page reload  
❌ SPA behavior breaks

---

### ✅ `<Link>` (Correct Way)

`import { Link } from "react-router-dom";  <Link to="/about">About</Link>`

✔ No reload  
✔ Faster navigation  
✔ Preserves state

---

### 🔹 NavLink (Active Styling)

`import { NavLink } from "react-router-dom";  <NavLink   to="/about"   className={({ isActive }) => (isActive ? "active" : "")} >   About </NavLink>`

📌 `NavLink` automatically knows:

- Which link is active
    

---

### 🔹 Link vs NavLink vs `<a>`

|Feature|`<a>`|`Link`|`NavLink`|
|---|---|---|---|
|Reload|Yes|No|No|
|Active style|No|No|Yes|
|SPA friendly|❌|✅|✅|

---

## 7️⃣ Dynamic Routing with `useParams`

### 🔹 What are URL Params?

Dynamic parts of URL:

`/products/101 /products/102`

---

### 🔹 Route with Param

`<Route path="/product/:id" element={<Product />} />`

---

### 🔹 Access Param using `useParams`

`import { useParams } from "react-router-dom";  function Product() {   const { id } = useParams();   return <h2>Product ID: {id}</h2>; }`

📌 `id` comes from URL  
📌 Useful for:

- Product pages
    
- User profiles
    

---

## 8️⃣ Navigation Programmatically (`useNavigate`)

### 🔹 What is `useNavigate`?

- Navigate using JavaScript
    
- Useful after:
    
    - Form submit
        
    - Login success
        
    - Button click
        

---

### 🔹 Example

`import { useNavigate } from "react-router-dom";  function Login() {   const navigate = useNavigate();    const handleLogin = () => {     navigate("/dashboard");   };    return <button onClick={handleLogin}>Login</button>; }`

📌 Similar to `history.push()` (old)

---

## 9️⃣ Nested Routing (Modern Way)

### 🔹 What is Nested Routing?

Routes inside routes  
Used for:

- Dashboard layouts
    
- Sidebar layouts
    

---

### 🔹 Parent Route

`<Route path="/dashboard" element={<Dashboard />}>   <Route path="profile" element={<Profile />} />   <Route path="settings" element={<Settings />} /> </Route>`

---

### 🔹 Outlet (Where child renders)

`import { Outlet } from "react-router-dom";  function Dashboard() {   return (     <>       <h2>Dashboard</h2>       <Outlet />     </>   ); }`

📌 `<Outlet />` is **placeholder** for child routes

---

## 🔟 404 Page Handling

### 🔹 Why 404?

When user enters:

`/some-random-page`

---

### 🔹 Catch All Route

`<Route path="*" element={<NotFound />} />`

📌 Must be last route

---

## 1️⃣1️⃣ Modern Routing Best Practices

### ✔ Use Routes instead of Switch

### ✔ Use element instead of component

### ✔ Prefer `NavLink` for menus

### ✔ Use nested routes + Outlet

### ✔ Use hooks (`useParams`, `useNavigate`)

---

## 1️⃣2️⃣ Complete Flow (Mental Model)

`URL change    ↓ BrowserRouter detects    ↓ Routes checks matching path    ↓ Correct component renders    ↓ Outlet renders nested routes`

---

## ❌ Common React Router Mistakes

- Forgetting BrowserRouter
    
- Using `<a>` tag
    
- Missing `Outlet`
    
- Wrong path nesting
    
- Forgetting `*` route
    

---

## ✅ Summary

- React Router enables SPA navigation
    
- `BrowserRouter` wraps app
    
- `Routes` contains routes
    
- `Route` maps URL → component
    
- `Link` replaces `<a>`
    
- `NavLink` handles active styling
    
- `useParams` reads URL params
    
- `useNavigate` navigates via JS
    
- `Outlet` renders nested routes
    
- `*` handles 404 pages