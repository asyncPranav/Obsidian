
---

## **Think of `useRef` as a small “box” you keep on your desk**

- Inside the box, you can put anything you want — a number, a text, or even a DOM element (like an `<input>`).
    
- The box stays there **even if your component refreshes**.
    
- When you change what’s inside the box, **React won’t re-render** your component.
    
- The only way React updates the UI is when `useState` changes — `useRef` changes are silent.
    

---

### **Two main things `useRef` is used for**

#### 1️⃣ **To directly control a DOM element**

- Example: You can make an `<input>` automatically get the cursor when the page loads.
    
- Without `useRef`, you’d have no direct way to “point” to that exact input.
    

#### 2️⃣ **To store data without causing re-renders**

- Example: You want to store how many times a button was clicked,  
    but you don’t want the page to re-render every time.
    
- You can keep that count inside `useRef` instead of `useState`.
    

---

### **How `useRef` works**

```jsx
const myBox = useRef("hello");
```

Here’s what happens:

- `myBox` → an object like `{ current: "hello" }`
    
- You can **read** the value: `myBox.current` → `"hello"`
    
- You can **change** the value: `myBox.current = "world"`
    
- Changing `myBox.current` won’t make React redraw your page.
    

---

### **Why not just use variables?**

Because normal variables **reset every time the component re-renders**.  
`useRef` values **stay the same** across re-renders.

---

💡 **Analogy**:

- `useState` is like a whiteboard where changing it makes the whole class look up because something changed.
    
- `useRef` is like your secret notebook — you can scribble in it, but no one knows unless you tell them.