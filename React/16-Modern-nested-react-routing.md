
---


# **Nested Routing in React Router v6.4+ Explained for Beginners**

We’re building a **Dashboard app** with 3 pages: Home, Profile, Settings, plus 404 pages. We’ll also use **nested routing**, **loader data**, and a **layout**.

---

## **1️⃣ Folder Structure**

Here’s how our project looks:

```jsx
import { useLoaderData } from "react-router-dom";

const Profile = () => {
  const user = useLoaderData(); // this gets data from loader
  return (
    <div>
      <h2>Profile Page</h2>
      <p>Name: {user.name}</p>
      <p>Email: {user.email}</p>
    </div>
  );
};

export default Profile;
```

**Explanation:**

- **public/index.html** → Standard HTML file for React app.
    
- **src/main.jsx** → Bootstraps React app.
    
- **src/App.jsx** → Our main App component.
    
- **src/router.jsx** → All the routes live here.
    
- **layouts/** → Components that wrap pages (like Navbar, Sidebar).
    
- **pages/** → Individual page components.
    
- **api/** → Functions to fetch data from "backend" or API.
    

---

## **2️⃣ Create Pages**

### **Home.jsx**

```jsx
const Home = () => {
  return <h2>Dashboard Home</h2>;
};

export default Home;
```

- Simple page, just text.
    
- This will show when we go to `/dashboard`.
    

---

### **Profile.jsx (with loader example)**

```jsx
import { useLoaderData } from "react-router-dom";

const Profile = () => {
  const user = useLoaderData(); // this gets data from loader
  return (
    <div>
      <h2>Profile Page</h2>
      <p>Name: {user.name}</p>
      <p>Email: {user.email}</p>
    </div>
  );
};

export default Profile;
```

- `useLoaderData()` → Special React Router hook that **gets data before the page renders**.
    
- We'll fetch user info using a **loader**.


## **1️⃣ What is a loader?**

A **loader** in React Router is a function that **runs before a route renders**.

Think of it like this:

> “Before showing this page, fetch the data it needs.”

For example, your **Profile page** needs user data (`name` and `email`). Instead of fetching inside `useEffect`, you can use a loader to fetch **before** the page renders.

---

### **Why use a loader?**

1. Data is ready when the page loads → no flicker or empty content.
    
2. Cleaner code → you don’t need `useEffect` + state inside the component.
    
3. Works well with **nested routes** and **React Router’s data APIs**.
    

---

## **2️⃣ How loader works**

1. You define a loader function.
    
2. Attach it to a route using `loader: fetchUser`.
    
3. Inside the component, use `useLoaderData()` to get the data.
    

---

### **Example: Profile page**

**fetchUser.js**

```js
export const fetchUser = async () => {
  // Simulate API call
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({ name: "John Doe", email: "john@example.com" });
    }, 500); // wait 0.5s
  });
};
```

**router.jsx**

```jsx
{
  path: "profile",
  element: <Profile />,
  loader: fetchUser, // ← attach loader here
}
```

**Profile.jsx**

```jsx
import { useLoaderData } from "react-router-dom";

const Profile = () => {
  const user = useLoaderData(); // ← this gets the data from loader

  return (
    <div>
      <h2>Profile Page</h2>
      <p>Name: {user.name}</p>
      <p>Email: {user.email}</p>
    </div>
  );
};

export default Profile;
```

**What happens:**

1. User navigates to `/dashboard/profile`.
    
2. React Router sees the route has a `loader`.
    
3. It calls `fetchUser()`.
    
4. Once the loader resolves, it renders `<Profile />` with data from `useLoaderData()`.
    

---

## **3️⃣ Key points for beginners**

|Concept|Explanation|
|---|---|
|`loader`|A function that fetches data **before the route renders**.|
|`useLoaderData()`|React Router hook that **returns the data loaded by the loader**.|
|Route waits for loader|The page only renders **after loader resolves**.|
|Good for dashboard/pages|Perfect for pages that need data (user info, posts, etc.) before showing.|

---

## **4️⃣ Why not `useEffect`?**

You could write this inside `<Profile />`:

```jsx
useEffect(() => {
  fetchUser().then(setUser);
}, []);
```

- Problem: The page first renders **without data** → you need loading states.
    
- Using **loader** solves this automatically → `<Profile />` renders **already with data**.
    

---

## **5️⃣  Full example flow**

1. Go to `/dashboard/profile`
    
2. React Router sees loader attached
    
3. Calls `fetchUser()`
    
4. Waits for promise to resolve (0.5s)
    
5. Renders `<Profile />` with user data already available
    

No need for `<Spinner>` unless you want a loading state explicitly.

---

### **Settings.jsx**

```jsx
const Settings = () => {
  return <h2>Settings Page</h2>;
};

export default Settings;
```

- Another simple page.
    

---

### **NotFound.jsx**

```jsx
const NotFound = () => <h2>404 - Page Not Found</h2>;
export default NotFound;
```

- This page appears when a URL **doesn’t match** any route.
    

---

## **3️⃣ Layout Component**

### **DashboardLayout.jsx**

```jsx
import { NavLink, Outlet } from "react-router-dom";

const DashboardLayout = () => {
  return (
    <div>
      <h1>Dashboard</h1>
      <nav>
        <NavLink to="/dashboard" end>Home</NavLink>{" | "}
        <NavLink to="/dashboard/profile">Profile</NavLink>{" | "}
        <NavLink to="/dashboard/settings">Settings</NavLink>
      </nav>
      <hr />
      <Outlet />
    </div>
  );
};

export default DashboardLayout;
```

**Key Concepts:**

- **`NavLink`** → Like `<a>` but **reactive** (highlights active link automatically).
    
- `end` → Makes "Home" active only on `/dashboard`, not `/dashboard/profile`.
    
- **`Outlet`** → Placeholder for **nested routes**.
    
    - When user clicks Profile, `Outlet` renders `<Profile />`.
        
    - Layout itself (header + nav) stays the same.
        

So **DashboardLayout** wraps all pages.

---

## **4️⃣ API Loader Example**

### **fetchUser.js**

```jsx
export const fetchUser = async () => {
  // Simulate API call
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({ name: "John Doe", email: "john@example.com" });
    }, 500);
  });
};
```

- Simulates **fetching user data** from a server.
    
- Returns **fake user data** after 0.5 seconds.
    
- Will be used in Profile page **loader**.
    

---

## **5️⃣ Router Setup**

### **router.jsx**

```jsx
import { createBrowserRouter } from "react-router-dom";
import DashboardLayout from "./layouts/DashboardLayout";
import Home from "./pages/Home";
import Profile from "./pages/Profile";
import Settings from "./pages/Settings";
import NotFound from "./pages/NotFound";
import { fetchUser } from "./api/fetchUser";

export const router = createBrowserRouter([
  {
    path: "/dashboard",
    element: <DashboardLayout />,
    children: [
      { index: true, element: <Home /> },           // /dashboard
      { path: "profile", element: <Profile />, loader: fetchUser }, // /dashboard/profile
      { path: "settings", element: <Settings /> }, // /dashboard/settings
      { path: "*", element: <NotFound /> },         // nested 404
    ],
  },
  { path: "*", element: <NotFound /> }, // global 404
]);
```

**Key Concepts:**

- **Nested Routes** → Use `children` array under `/dashboard`.
    
- **Index Route** → `index: true` makes Home page default.
    
- **Loader** → Fetch data before rendering Profile page.
    
- **Nested 404** → If you go `/dashboard/unknown`, it shows nested 404.
    
- **Global 404** → If you go `/unknown`, it shows global 404.
    

---

## **6️⃣ App.jsx**

```jsx
import { RouterProvider } from "react-router-dom";
import { router } from "./router";

const App = () => {
  return <RouterProvider router={router} />;
};

export default App;
```

- `RouterProvider` → Connects **router configuration** to the app.
    
- **No routing logic inside App.jsx**. Clean and simple.
    

---

## **7️⃣ main.jsx**

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

- This is **entry point**.
    
- React renders `<App />` inside `<div id="root">` in HTML.
    

---

## **8️⃣ How It Works**

1. You go to `/dashboard`
    
    - Layout renders header/nav.
        
    - `<Outlet />` shows `<Home />`.
        
2. You go to `/dashboard/profile`
    
    - Layout still renders header/nav.
        
    - `<Outlet />` now shows `<Profile />`.
        
    - Loader fetches user data → shows Name + Email.
        
3. You go to `/dashboard/settings`
    
    - Layout still renders header/nav.
        
    - `<Outlet />` shows `<Settings />`.
        
4. You go to `/dashboard/unknown`
    
    - Layout still renders header/nav.
        
    - `<Outlet />` shows nested 404.
        
5. You go to `/unknown`
    
    - Global 404 shows.
        

---

## ✅ **Key Concepts You Practiced**

|Concept|What it Means|
|---|---|
|**Nested Routes**|`/dashboard` is parent, pages inside are children. Use `Outlet` to render them.|
|**Index Route**|Default child page for `/dashboard`.|
|**Loader**|Fetch data **before page renders**. Use `useLoaderData()` to access it.|
|**404 Handling**|Nested 404 for `/dashboard/*` and global 404 for `*`.|
|**Layout**|Shared UI (nav, header) that wraps child pages.|
|**Clean App.jsx**|Keep routing logic in `router.jsx` only.|

---

💡 **Tips for beginners:**

- Always use `Outlet` in **layout components** for nested routes.
    
- `NavLink` helps to **highlight active page**.
    
- `loader` is useful for **preloading data**, instead of fetching inside `useEffect`.
    
- Keep App.jsx **clean**, only `RouterProvider`.