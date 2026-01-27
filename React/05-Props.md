
---
## 1️⃣ What are Props?

### Definition (Very Simple)

**Props** stands for **properties**.

👉 Props are **inputs given to a React component** from its parent.

Think of props like:

- **Function arguments**
    
- **Attributes of a component**
    

### Example (Real Life)

```jsx
<Car color="red" brand="BMW" />
```

Here:

- `color` and `brand` are **props**
    
- `Car` is the component
    

---

## 2️⃣ Why Props Are Needed?

Without props ❌:

- Components cannot receive data
    
- Same UI always shows same content
    
- Reusability is limited
    

With props ✅:

- Components become **dynamic**
    
- Same component can show **different data**
    
- Data flows cleanly from parent to child
    

📌 **Props make components reusable and flexible**

---

## 3️⃣ Passing Props (Parent Component)

Props are passed like **HTML attributes**.

### Example

```js
function App() {
  return <User name="Rahul" age={22} />;
}
```

📌 Notes:

- Strings → `"Rahul"`
    
- Numbers / variables → `{}`
    

---

## 4️⃣ Receiving Props (Child Component)

### Method 1: Using `props` object

```js
function User(props) {
  return (
    <h1>
      Name: {props.name}, Age: {props.age}
    </h1>
  );
}
```

Here:

- `props` is an **object**
    
- Contains all values passed from parent
    

---

## 5️⃣ Props Object

### What is `props`?

- A **plain JavaScript object**
    
- Created by React
    
- Contains **key-value pairs**
    

Example:

```js
props = {
  name: "Rahul",
  age: 22
}
```

📌 You should **never modify** this object.

---

## 6️⃣ Destructuring Props (Recommended Way)

### Why destructuring?

- Cleaner code
    
- Easier to read
    
- Professional practice
    

### Example

```js
function User({ name, age }) {
  return <h1>{name} is {age} years old</h1>;
}
```

Equivalent to:

```js
const name = props.name;
const age = props.age;
```

---

## 7️⃣ Passing Different Data Types as Props

### String

```js
<User name="Rahul" />
```

### Number

```js
<User age={25} />
```

### Boolean

```js
<User isAdmin={true} />
```

### Array

```js
<User skills={["JS", "React"]} />
```

### Object

```js
<User info={{ city: "Delhi", country: "India" }} />
```

### Function (IMPORTANT ⭐)

```js
<User onClick={handleClick} />
```

---

## 8️⃣ Default Props

### Why Default Props?

- Prevent `undefined`
    
- Provide fallback values
    

### Example

```js
function User({ name = "Guest" }) {
  return <h1>Hello {name}</h1>;
}
```

If parent does not pass `name`, default is used.

📌 Modern React uses **default parameters** instead of `defaultProps`.

---

## 9️⃣ Props Are Read-Only (VERY IMPORTANT ⚠️)

❌ Wrong:

```js
props.name = "Amit";
```

✅ Correct:

- Props can only be **read**
    
- Cannot be modified inside child
    

📌 Reason:

> React follows **unidirectional data flow**

---

## 🔟 Parent → Child Data Flow

### Rule:

> Data flows **only one way** in React:  
> **Parent → Child**

Example:

```js
function Parent() {
  return <Child value="Hello" />;
}
```

Child cannot directly change parent’s data.

📌 To change parent data, child must use **callback props** (next chapter topic).

---

## 1️⃣1️⃣ Props vs Variables

|Feature|Props|Variables|
|---|---|---|
|Source|Parent component|Same component|
|Mutability|Read-only|Mutable|
|Purpose|Pass data|Store local data|
|Scope|Component input|Component logic|

---

## 1️⃣2️⃣ Children Prop (IMPORTANT ⭐)

### What is `children`?

`children` is a **special prop** used in component composition.

### Example

```js
function Card({ children }) {
  return <div className="card">{children}</div>;
}
```

Usage:

```js

```

📌 `children` allows **flexible UI composition**

---

## 1️⃣3️⃣ Conditional Rendering Using Props

`function User({ isLoggedIn }) {   return (     <h1>{isLoggedIn ? "Welcome" : "Please Login"}</h1>   ); }`

---

## 1️⃣4️⃣ Common Beginner Mistakes ⚠️

1. Trying to modify props
    
2. Forgetting `{}` for non-string values
    
3. Misspelling prop names
    
4. Passing props but not using them
    
5. Confusing props with state
    

---

## 1️⃣5️⃣ Interview Important Points ⭐

- Props are immutable
    
- Props flow parent → child
    
- Props are like function arguments
    
- `children` is a special prop
    
- Destructuring props is best practice
    

---

## ✅ Chapter 4 Summary

✔ Props pass data to components  
✔ Props make components dynamic  
✔ Props are read-only  
✔ Data flows one way  
✔ Destructuring improves readability  
✔ `children` enables composition