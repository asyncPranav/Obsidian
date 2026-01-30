
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

## 2️⃣ Controlled vs Uncontrolled Components (CORE CONCEPT ⭐)

---

## 🔹 Controlled Components

### Definition

A **controlled component** is a form element whose value is **controlled by React state**.

> React state = single source of truth

---

### Example: Controlled Text Input

`import { useState } from "react";  function Form() {   const [name, setName] = useState("");    return (     <input       type="text"       value={name}       onChange={(e) => setName(e.target.value)}     />   ); }`

📌 Input value comes from **state**  
📌 Every change updates state  
📌 Most commonly used in React

---

## 🔹 Uncontrolled Components

### Definition

An **uncontrolled component** stores its value **in the DOM**, not in React state.

React accesses it using **refs**.

---

### Example: Uncontrolled Input

`import { useRef } from "react";  function Form() {   const inputRef = useRef();    function handleSubmit() {     console.log(inputRef.current.value);   }    return (     <>       <input type="text" ref={inputRef} />       <button onClick={handleSubmit}>Submit</button>     </>   ); }`

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

`const [text, setText] = useState("");  <input   type="text"   value={text}   onChange={(e) => setText(e.target.value)} />`

📌 `onChange` fires on every keystroke  
📌 Always bind `value`

---

## 4️⃣ Handling Textarea

`const [message, setMessage] = useState("");  <textarea   value={message}   onChange={(e) => setMessage(e.target.value)} />`

📌 Same as text input  
📌 No special syntax

---

## 5️⃣ Handling Checkbox

### Single Checkbox

`const [isChecked, setIsChecked] = useState(false);  <input   type="checkbox"   checked={isChecked}   onChange={(e) => setIsChecked(e.target.checked)} />`

📌 Use `checked`, NOT `value`

---

## 6️⃣ Handling Radio Buttons

`const [gender, setGender] = useState("");  <input   type="radio"   value="male"   checked={gender === "male"}   onChange={(e) => setGender(e.target.value)} /> Male  <input   type="radio"   value="female"   checked={gender === "female"}   onChange={(e) => setGender(e.target.value)} /> Female`

📌 Radio buttons share the same state  
📌 Compare value for `checked`

---

## 7️⃣ Handling Select Dropdown

`const [city, setCity] = useState("");  <select value={city} onChange={(e) => setCity(e.target.value)}>   <option value="">Select city</option>   <option value="Delhi">Delhi</option>   <option value="Mumbai">Mumbai</option> </select>`

📌 Controlled dropdown  
📌 `value` + `onChange`

---

## 8️⃣ Handling Multiple Inputs (IMPORTANT ⭐)

### Best Pattern: Single State Object

`const [formData, setFormData] = useState({   name: "",   email: "", });`

`function handleChange(e) {   const { name, value } = e.target;    setFormData({     ...formData,     [name]: value,   }); }`

`<input   name="name"   value={formData.name}   onChange={handleChange} />  <input   name="email"   value={formData.email}   onChange={handleChange} />`

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

`function handleSubmit(e) {   e.preventDefault();   console.log("Form submitted"); }`

`<form onSubmit={handleSubmit}>   <button type="submit">Submit</button> </form>`

📌 `e.preventDefault()` is mandatory

---

## 🔟 Basic Form Validation

### Example: Required Field Validation

`function handleSubmit(e) {   e.preventDefault();    if (!formData.email) {     alert("Email is required");     return;   }    console.log(formData); }`

📌 Validation logic runs **before submission**

---

## 1️⃣1️⃣ Controlled Validation with State

`const [error, setError] = useState("");  if (!email) {   setError("Email is required"); }`

`{error && <p style={{ color: "red" }}>{error}</p>}`

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