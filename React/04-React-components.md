
---

# 📘 Chapter 3: React Components – Building Reusable UI Blocks

---

## 1️⃣ What is a Component?

### Definition (Simple)

A **component** is a **reusable piece of UI** written using JavaScript and JSX.

👉 In React, **everything is a component**.

### Example (Real Life)

Think of a website like a building:

- Button = component
    
- Header = component
    
- Footer = component
    
- Card = component
    

Instead of building everything again and again, we **reuse components**.

---

## 2️⃣ Why Components Are Needed?

Without components ❌:

- Code becomes very large
    
- Hard to read & maintain
    
- Repetition everywhere
    

With components ✅:

- Reusable code
    
- Easy to maintain
    
- Clean structure
    
- Easy teamwork
    

📌 **React’s biggest strength = component-based architecture**

---

## 3️⃣ Functional Components

### What are Functional Components?

- Simple **JavaScript functions**
    
- Return **JSX**
    
- Most commonly used today
    

### Syntax

```
```

OR (Arrow function)

`const Hello = () => {   return <h1>Hello React</h1>; };`

📌 A functional component:

- Must return JSX
    
- Must start with a **capital letter**
    

---

## 4️⃣ Component Naming Rules (VERY IMPORTANT)

### Rules:

1. Component name **must start with a capital letter**
    
2. Use **PascalCase**
    
3. File name usually same as component name
    

### Examples

❌ Wrong:

`function header() {}`

✅ Correct:

`function Header() {}`

📌 Reason:

> React treats lowercase tags as HTML elements.

---

## 5️⃣ Returning JSX from a Component

### Basic Example

`function App() {   return (     <div>       <h1>Hello</h1>       <p>Welcome to React</p>     </div>   ); }`

### Important Rules:

- Component must return **one parent element**
    
- JSX must be **valid**
    
- Can return `null` if nothing to render
    

---

## 6️⃣ Rendering Components

### Rendering to DOM

`const root = ReactDOM.createRoot(   document.getElementById("root") );  root.render(<App />);`

📌 Note:

- `<App />` is **component usage**
    
- `App()` is **not used directly**
    

---

## 7️⃣ Reusing Components

### Example

`function Button() {   return <button>Click Me</button>; }  function App() {   return (     <div>       <Button />       <Button />       <Button />     </div>   ); }`

✔ Same component used multiple times  
✔ No duplication of code

---

## 8️⃣ Nesting Components (Parent → Child)

### Example

`function Header() {   return <h1>Header</h1>; }  function Footer() {   return <h1>Footer</h1>; }  function App() {   return (     <div>       <Header />       <Footer />     </div>   ); }`

📌 Components can contain **other components**.

---

## 9️⃣ Component Composition (IMPORTANT ⭐)

### What is Component Composition?

Component composition means:

> **Building complex UI by combining small components together**

Instead of inheritance, React uses **composition**.

### Example

`function Card({ children }) {   return <div className="card">{children}</div>; }  function App() {   return (     <Card>       <h2>Title</h2>       <p>Description</p>     </Card>   ); }`

📌 `children` allows flexible UI content.

---

## 🔟 Component vs HTML Element

|Feature|Component|HTML Element|
|---|---|---|
|Naming|Capitalized|Lowercase|
|Reusability|Yes|No|
|Logic|Can contain JS|No|
|Props|Yes|No|

Example:

`<Header />   // Component <h1>Title</h1> // HTML`

---

## 1️⃣1️⃣ Common Beginner Mistakes ⚠️

1. Using lowercase component name
    
2. Forgetting `return`
    
3. Returning multiple elements without wrapper
    
4. Using `App()` instead of `<App />`
    
5. Writing logic outside component
    
6. Not closing JSX tags
    

---

## 1️⃣2️⃣ How Components Work Internally (Basic Idea)

1. React calls the component function
    
2. Component returns JSX
    
3. JSX converts to React elements
    
4. React updates Virtual DOM
    
5. Browser DOM updates
    

---

## ✅ Chapter 3 Summary

✔ Components are reusable UI blocks  
✔ Functional components are preferred  
✔ Components must start with capital letters  
✔ JSX must return one parent  
✔ Components can be nested  
✔ Composition builds complex UI  
✔ Reuse improves maintainability