
---
# 📘 Chapter 9: Forms in React

## Controlled & Uncontrolled Components

---

## 1️⃣ What Are Forms in React?

Forms are used to:

- Collect user input
    
- Submit data
    
- Handle user interaction
    

Examples:

- Login form
    
- Signup form
    
- Search box
    
- Feedback form
    

React handles forms **differently than plain HTML**.

---

## 1️⃣ How Input Works in Normal HTML (Before React)

### Example (Plain HTML)

```html
<input type="text" />
```

➡ User types  
➡ Browser automatically stores the value  
➡ JavaScript is NOT aware unless we ask

In HTML:

- Browser controls the input
    
- JavaScript has no control by default
    

---

## 2️⃣ Problem React Solves

React wants:  
✔ UI  
✔ Data  
✔ Logic

to be **in sync**

React says:

> “I want to control the input value using JavaScript (state)”

That is why **events like `onChange` exist**

---

## 3️⃣ What is `onChange`? (Very Important)

### Definition (Simple Language)

> `onChange` is a **React event** that runs **every time the input value changes**

Whenever user:

- Types a letter
    
- Deletes a letter
    
- Pastes text
    

👉 `onChange` runs automatically

---

## 4️⃣ Why `onChange` is Needed

React does NOT automatically know what user typed.

So we must:

1. Listen to input changes → `onChange`
    
2. Read the typed value
    
3. Store it in state
    

---

## 5️⃣ What Happens When User Types? (Flow)

```js
User types → onChange event triggers
          → event object created
          → e.target.value contains typed text
          → setState updates state
          → component re-renders
          → input shows updated value
```

---

## 6️⃣ Event Object (`e`) Explained Slowly

```js
function handleChange(e) {
  console.log(e);
}
```

`e` (event object) contains:

- `e.target` → the input element
    
- `e.target.value` → what user typed
    
- `e.target.name` → input name
    

---

## 2️⃣ Controlled vs Uncontrolled Components (CORE CONCEPT ⭐)

---

## 🔹 Controlled Components

### Definition

A **controlled component** is a form element whose value is **controlled by React state**.

> React state = single source of truth

---

### Example: Controlled Text Input

```jsx
import { useState } from "react";

function Form() {
  const [name, setName] = useState("");

  return (
    <input
      type="text"
      value={name}
      onChange={(e) => setName(e.target.value)}
    />
  );
}
```

📌 Input value comes from **state**  
📌 Every change updates state  
📌 Most commonly used in React

---



## 7️⃣ First Basic Controlled Input (STEP BY STEP)

### Step 1: Create State

```jsx
import { useState } from "react";

function App() {
  const [name, setName] = useState("");
}
```

📌 `name` → stores input value  
📌 `setName` → updates value

---

### Step 2: Create Input WITHOUT onChange (Problem)

```jsx
<input type="text" value={name} />
```

❌ Input becomes **read-only**  
❌ User cannot type

WHY?  
Because React controls value, but no way to update it

---

## 8️⃣ Fix: Add `onChange`

### Step 3: Create Handler Function

```jsx
function handleNameChange(event) {
  setName(event.target.value);
}
```

📌 Reads typed value  
📌 Updates state

---

### Step 4: Attach `onChange`

```jsx
<input
  type="text"
  value={name}
  onChange={handleNameChange}
/>
```

✅ Now input works  
✅ React controls input  
✅ This is a **Controlled Component**

---

## 9️⃣ Full Working Example (Beginner Version)

```jsx
import { useState } from "react";

function App() {
  const [name, setName] = useState("");

  function handleNameChange(e) {
    setName(e.target.value);
  }

  return (
    <>
      <h2>Name: {name}</h2>

      <input
        type="text"
        value={name}
        onChange={handleNameChange}
        placeholder="Enter your name"
      />
    </>
  );
}

export default App;
```

---

## 🔟 Why `value` is Required

```jsx
value={name}
```

Because:

- React says → "Input value must come from state"
    
- State becomes **single source of truth**
    

---

## 1️⃣1️⃣ Inline `onChange` (AFTER Understanding Basics)

⚠️ Beginners should use function first  
Later, you can write:

```jsx
<input
  value={name}
  onChange={(e) => setName(e.target.value)}
/>
```

⚠️ This is **short syntax**, not magic  
It does **same thing**

---

## 1️⃣2️⃣ Checkbox Handling (Explained Slowly)

### Step 1: State

```jsx
const [isChecked, setIsChecked] = useState(false);
```

---

### Step 2: Handler

```jsx
function handleCheckboxChange(e) {
  setIsChecked(e.target.checked);
}
```

📌 `checked` is boolean (true/false)

---

### Step 3: Input

```jsx
<input
  type="checkbox"
  checked={isChecked}
  onChange={handleCheckboxChange}
/>
```

---

## 1️⃣3️⃣ Form Submission Explained

### HTML Problem

```jsx
<form>
  <button type="submit">Submit</button>
</form>
```

➡ Page reloads  
➡ React app breaks

---

### React Solution

```jsx
function handleSubmit(e) {
  e.preventDefault(); // stops reload
  console.log("Form submitted");
}
```

```jsx
<form onSubmit={handleSubmit}>
  <button type="submit">Submit</button>
</form>
```

---

## 1️⃣4️⃣ Controlled vs Uncontrolled (Clear Difference)

### Controlled

- Uses `value`
    
- Uses `onChange`
    
- Uses state
    

### Uncontrolled

- Uses DOM
    
- Uses `ref`
    
- No state control
    

---

## 1️⃣5️⃣ Beginner Mistakes ❌

1. Using `value` without `onChange`
    
2. Forgetting `e.target.value`
    
3. Using `value` instead of `checked` for checkbox
    
4. Thinking React auto-reads input
    
5. Skipping handler function explanation
    

---

## 🧠 Golden Rule (Remember This)

> **If input has `value`, it MUST have `onChange`**

---


---

## 🔹 Uncontrolled Components

### Definition

An **uncontrolled component** stores its value **in the DOM**, not in React state.

React accesses it using **refs**.

---

### Example: Uncontrolled Input

```jsx
import { useRef } from "react";

function Form() {
  const inputRef = useRef();

  function handleSubmit() {
    console.log(inputRef.current.value);
  }

  return (
    <>
      <input type="text" ref={inputRef} />
      <button onClick={handleSubmit}>Submit</button>
    </>
  );
}
```

📌 DOM controls the input  
📌 Less React control  
📌 Used rarely (file inputs, legacy code)

---

### Controlled vs Uncontrolled (Comparison)

|Controlled|Uncontrolled|
|---|---|
|State-driven|DOM-driven|
|Easy validation|Hard validation|
|Recommended|Limited use|
|More code|Less code|

---

## 3️⃣ Handling Text Inputs

```jsx
const [text, setText] = useState("");

<input
  type="text"
  value={text}
  onChange={(e) => setText(e.target.value)}
/>
```

📌 `onChange` fires on every keystroke  
📌 Always bind `value`

---

## 4️⃣ Handling Textarea

```jsx
const [message, setMessage] = useState("");

<textarea
  value={message}
  onChange={(e) => setMessage(e.target.value)}
/>
```

📌 Same as text input  
📌 No special syntax

---

## 5️⃣ Handling Checkbox

### Single Checkbox

```jsx
const [isChecked, setIsChecked] = useState(false);

<input
  type="checkbox"
  checked={isChecked}
  onChange={(e) => setIsChecked(e.target.checked)}
/>
```

📌 Use `checked`, NOT `value`

---

## 6️⃣ Handling Radio Buttons

```jsx
const [gender, setGender] = useState("");

<input
  type="radio"
  value="male"
  checked={gender === "male"}
  onChange={(e) => setGender(e.target.value)}
/> Male

<input
  type="radio"
  value="female"
  checked={gender === "female"}
  onChange={(e) => setGender(e.target.value)}
/> Female
```

📌 Radio buttons share the same state  
📌 Compare value for `checked`

---

## 7️⃣ Handling Select Dropdown

```jsx
const [city, setCity] = useState("");

<select value={city} onChange={(e) => setCity(e.target.value)}>
  <option value="">Select city</option>
  <option value="Delhi">Delhi</option>
  <option value="Mumbai">Mumbai</option>
</select>
```

📌 Controlled dropdown  
📌 `value` + `onChange`

---

## 8️⃣ Handling Multiple Inputs (IMPORTANT ⭐)

### Best Pattern: Single State Object

```jsx
const [formData, setFormData] = useState({
  name: "",
  email: "",
});
```

```jsx
function handleChange(e) {
  const { name, value } = e.target;

  setFormData({
    ...formData,
    [name]: value,
  });
}
```

```jsx
<input
  name="name"
  value={formData.name}
  onChange={handleChange}
/>

<input
  name="email"
  value={formData.email}
  onChange={handleChange}
/>
```

📌 Clean  
📌 Scalable  
📌 Industry standard

---

## 9️⃣ Form Submission & Prevent Default

### Why prevent default?

HTML forms **reload the page** by default.

React apps are **single-page apps**, so we stop that.

---

### Example

```jsx
function handleSubmit(e) {
  e.preventDefault();
  console.log("Form submitted");
}
```

```jsx
<form onSubmit={handleSubmit}>
  <button type="submit">Submit</button>
</form>
```

📌 `e.preventDefault()` is mandatory

---

## 🔟 Basic Form Validation

### Example: Required Field Validation

```jsx
function handleSubmit(e) {
  e.preventDefault();

  if (!formData.email) {
    alert("Email is required");
    return;
  }

  console.log(formData);
}
```

📌 Validation logic runs **before submission**

---

## 1️⃣1️⃣ Controlled Validation with State

```jsx
const [error, setError] = useState("");

if (!email) {
  setError("Email is required");
}
```

```jsx
{error && <p style={{ color: "red" }}>{error}</p>}
```

---

## 1️⃣2️⃣ Common Mistakes ❌

1. Missing `value` in controlled input
    
2. Forgetting `onChange`
    
3. Using `value` instead of `checked` for checkbox
    
4. Not calling `preventDefault`
    
5. Mutating state directly
    
6. Using uncontrolled inputs everywhere
    

---

## 1️⃣3️⃣ When to Use Uncontrolled Components (Extra ⭐)

Use uncontrolled when:

- File uploads (`<input type="file">`)
    
- Very simple forms
    
- Performance-critical edge cases
    

---

## 1️⃣4️⃣ Interview Points ⭐

- Controlled vs uncontrolled difference
    
- Why controlled components are preferred
    
- How to handle multiple inputs
    
- How to validate forms
    
- Why preventDefault is needed
    

---

## 🧠 Mental Model

> **User Input → Event → State → UI**

---

## ✅ Chapter 9 Summary

✔ Controlled components = React state controlled  
✔ Uncontrolled components = DOM controlled  
✔ All input types handled via state  
✔ Forms require `preventDefault()`  
✔ Validation is done before submission


---


# 📘 Uncontrolled Components & `useRef` (Beginner-Friendly Detailed Notes)

---

## 1️⃣ First Understand the Problem

### How inputs work normally (without React)

In plain HTML:

```jsx
<input type="text" />
```

- Browser stores the value
    
- JavaScript does NOT control it
    
- Value lives inside the DOM
    

React usually wants **state control**, but **sometimes we don’t want React to control everything**.

👉 That’s where **Uncontrolled Components** come in.

---

## 2️⃣ What is an Uncontrolled Component?

### Definition (Simple Language)

> An **uncontrolled component** is a form element whose value is **controlled by the DOM itself**, not by React state.

- No `value` prop
    
- No `onChange` for storing state
    
- React reads the value **only when needed**
    

---

## 3️⃣ Why Do We Need Uncontrolled Components?

React controlled components:

- More code
    
- More re-renders
    
- Not always necessary
    

Uncontrolled components are useful when:

- Simple forms
    
- File inputs
    
- Legacy code
    
- Performance-sensitive scenarios
    

---

## 4️⃣ Problem: How Does React Read DOM Value?

React normally does NOT directly access DOM.

So how do we read input value?

👉 **Answer: `useRef`**

---

## 5️⃣ What is `useRef`?

### Definition

> `useRef` is a React Hook that gives you **direct access to a DOM element**.

It creates a **reference** (pointer) to an element.

---

## 6️⃣ Syntax of `useRef`

`import { useRef } from "react";  const myRef = useRef();`

- `myRef` is an object
    
- Actual DOM element is stored in:
    

`myRef.current`

---

## 7️⃣ Basic Example: Uncontrolled Text Input

### Step-by-Step (IMPORTANT)

### Step 1: Create a ref

`import { useRef } from "react";  function App() {   const inputRef = useRef();`

---

### Step 2: Attach ref to input

`<input type="text" ref={inputRef} />`

📌 React now knows which DOM element to track

---

### Step 3: Read value when needed

`function handleSubmit() {   console.log(inputRef.current.value); }`

---

### Step 4: Full Example

`import { useRef } from "react";  function App() {   const inputRef = useRef();    function handleSubmit() {     alert(inputRef.current.value);   }    return (     <>       <input type="text" ref={inputRef} />       <button onClick={handleSubmit}>Submit</button>     </>   ); }  export default App;`

---

## 8️⃣ Key Points to Understand

✔ No `useState`  
✔ No `onChange`  
✔ DOM controls input  
✔ React reads value only on button click

---

## 9️⃣ Uncontrolled Form Example

`import { useRef } from "react";  function Form() {   const nameRef = useRef();   const emailRef = useRef();    function handleSubmit(e) {     e.preventDefault();      const data = {       name: nameRef.current.value,       email: emailRef.current.value,     };      console.log(data);   }    return (     <form onSubmit={handleSubmit}>       <input type="text" ref={nameRef} />       <input type="email" ref={emailRef} />       <button type="submit">Submit</button>     </form>   ); }`

---

## 🔟 File Input (Best Use Case ⭐)

File input **cannot be controlled** properly using state.

`const fileRef = useRef();  <input type="file" ref={fileRef} />`

`console.log(fileRef.current.files[0]);`

---

## 1️⃣1️⃣ `useRef` Does NOT Cause Re-render

Very important difference:

|useState|useRef|
|---|---|
|Causes re-render|Does NOT re-render|
|UI updates|UI doesn’t update|
|Data-driven|Reference-driven|

---

## 1️⃣2️⃣ When NOT to Use Uncontrolled Components ❌

Avoid uncontrolled when:

- Validation is required
    
- Real-time input tracking needed
    
- Dynamic UI updates
    
- Large forms
    

---

## 1️⃣3️⃣ Controlled vs Uncontrolled (Clear Comparison)

|Feature|Controlled|Uncontrolled|
|---|---|---|
|Data stored in|React state|DOM|
|onChange|Required|Not required|
|Validation|Easy|Hard|
|Re-render|Yes|No|
|Preferred|✅ Yes|⚠ Sometimes|

---

## 1️⃣4️⃣ Common Beginner Mistakes ❌

1. Using `ref` AND `value` together
    
2. Expecting UI update from `useRef`
    
3. Using uncontrolled for complex forms
    
4. Forgetting `.current`
    
5. Mutating DOM manually
    

---

## 1️⃣5️⃣ Mental Model (Very Important)

### Controlled:

`User → Event → State → UI`

### Uncontrolled:

`User → DOM → React reads when needed`

---

## 1️⃣6️⃣ Interview Questions ⭐

- Difference between controlled & uncontrolled components
    
- Why `useRef` doesn’t re-render
    
- When to use uncontrolled components
    
- How file input is handled in React
    

---

## ✅ Final Summary

✔ Uncontrolled components are DOM-driven  
✔ `useRef` accesses DOM elements  
✔ No state, no re-render  
✔ Useful for file inputs & simple forms  
✔ Controlled is preferred for most cases