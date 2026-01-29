
---

# 📘 Chapter 7: Conditional Rendering

Conditional rendering means **showing different UI based on a condition**.  
In React (and JSX), conditions are usually based on:

- state
    
- props
    
- API data
    
- user actions
    
- authentication status
    

---

## 🔹 Why Conditional Rendering Is Important

- UI must react to **data changes**
    
- Prevents showing invalid or empty content
    
- Improves UX (loading states, errors, permissions)
    
- Used in almost **every real-world app**
    

Examples:

- Show login button if user is not logged in
    
- Show spinner while data is loading
    
- Show error message if API fails
    

---

## 1️⃣ Using `if` Statements in JSX

### ❗ Important Rule

You **cannot write `if` directly inside JSX**.

❌ Invalid:

```jsx
return (
  <div>
    if (isLoggedIn) {
      <Dashboard />
    }
  </div>
)
```

### ✅ Correct Way: Use `if` _before_ return

```jsx
function App() {
  if (isLoggedIn) {
    return <Dashboard />
  }

  return <Login />
}
```

### ✅ Another Pattern: Variable Assignment

```jsx
let content

if (isLoggedIn) {
  content = <Dashboard />
} else {
  content = <Login />
}

return <div>{content}</div>
```

### 📌 When to Use `if`

- Complex conditions
    
- Multiple branches
    
- Early returns
    

---

## 2️⃣ Ternary Operator (`condition ? A : B`)

The ternary operator is the **most common** way to do conditional rendering in JSX.

### ✅ Syntax

```jsx
condition ? expressionIfTrue : expressionIfFalse
```

### Example

```jsx
{isLoggedIn ? <Dashboard /> : <Login />}
```

### Nested Ternary (Avoid if Possible)

```jsx
{status === "loading"
  ? <Loader />
  : status === "error"
  ? <Error />
  : <Data />}
```

⚠️ **Too many nested ternaries reduce readability**

### 📌 When to Use Ternary

- Simple conditions
    
- Two UI states
    
- Inline rendering
    

---

## 3️⃣ Logical AND (`&&`) for Rendering

### ✅ How It Works

If the condition is `true`, React renders the component.

`condition && <Component />`

### Example

`{isLoggedIn && <Dashboard />}`

If `isLoggedIn` is `false`, nothing is rendered.

### ⚠️ Important Pitfall

Falsy values like `0` will render `0`.

❌ Bad:

`{items.length && <ItemList />}`

If `items.length` is `0`, React renders `0`.

✅ Fix:

`{items.length > 0 && <ItemList />}`

### 📌 When to Use `&&`

- Show something or nothing
    
- No `else` case needed
    

---

## 4️⃣ Showing and Hiding Elements

### Using State

`const [show, setShow] = useState(false)  <button onClick={() => setShow(!show)}>Toggle</button>  {show && <p>Hello World</p>}`

### Using Ternary

`{show ? <Modal /> : null}`

### Using CSS (Visibility Only)

`<div style={{ display: show ? "block" : "none" }}>   Hidden Content </div>`

📌 Prefer conditional rendering over CSS hiding when possible.

---

## 5️⃣ Conditional Components

You can render **entire components conditionally**.

`function App() {   return (     <div>       {isAdmin ? <AdminPanel /> : <UserPanel />}     </div>   ) }`

### Conditional Rendering Based on Props

`function Alert({ type }) {   if (type === "error") return <ErrorAlert />   if (type === "success") return <SuccessAlert />   return null }`

---

## 6️⃣ Conditional Class Names (`className` with state/props)

### Using Ternary

`<button className={isActive ? "active" : "inactive"}>   Click </button>`

### Using Template Literals

``<button className={`btn ${isActive ? "btn-active" : ""}`}>   Click </button>``

### Real Example

`<input   className={hasError ? "input error" : "input"} />`

---

## 7️⃣ Real-World Examples

### 🔐 Authentication

`{user ? <Logout /> : <Login />}`

### ⏳ Loading State

`{loading && <Spinner />}`

### ❌ Error Handling

`{error ? <ErrorMessage /> : <Data />}`

### 📦 Empty State

`{items.length === 0   ? <p>No items found</p>   : <ItemList items={items} />}`

---

## 8️⃣ Best Practices

✅ Keep conditions **simple and readable**  
✅ Use `if` for complex logic  
✅ Use ternary for simple two-state UI  
✅ Avoid deeply nested ternaries  
✅ Prefer conditional rendering over CSS hiding

---

## 🧠 Summary

|Method|Use Case|
|---|---|
|`if` statement|Complex logic, early return|
|Ternary|Two UI states|
|`&&`|Show or hide|
|Conditional classes|Dynamic styling|
|Conditional components|Auth, roles, layout|