

# 📘 Chapter: Lifting State Up (Detailed Notes)

---

## 1️⃣ First, What is “State”?

State is:

- Data that **belongs to a component**
    
- Can change over time
    
- Causes **re-render** when updated
    

Example:

```jsx
const [count, setCount] = useState(0);
```

📌 State lives where it is **defined**

---

## 2️⃣ The Core Problem Lifting State Solves

### ❓ Problem Scenario

Two components need to:

- **Share the same data**
    
- Or **affect each other**
    

But:

- State exists in only one component
    
- Siblings cannot access each other’s state directly
    

---

### 🔴 Example Problem (Before Lifting)

```jsx
function InputBox() {
  const [text, setText] = useState("");
  return <input onChange={(e) => setText(e.target.value)} />;
}

function Display() {
  return <h2>Text here</h2>;
}
```

❌ `Display` cannot access `text`  
❌ State is trapped inside `InputBox`

---

## 3️⃣ What is Lifting State Up?

### 🔹 Simple Definition

**Lifting State Up** means:

> Moving state from a child component to the **nearest common parent**, so multiple children can share it.

---

### 🔹 Mental Model

```jsx
Before:
InputBox (state)
Display   (no access)

After:
Parent (state)
   ↓        ↓
InputBox  Display
```

---

## 4️⃣ Why “Nearest Common Parent”?

React rule:

- State should live **as close as possible** to where it’s needed
    
- But **not lower than all consumers**
    

---

## 5️⃣ Basic Lifting State Example (Step-by-Step)

---

### 🟢 Step 1: Parent Holds State

```jsx
function App() {
  const [text, setText] = useState("");

  return (
    <>
      <InputBox text={text} setText={setText} />
      <Display text={text} />
    </>
  );
}
```

---

### 🟢 Step 2: Child Updates Parent State

`function InputBox({ text, setText }) {   return (     <input       value={text}       onChange={(e) => setText(e.target.value)}     />   ); }`

---

### 🟢 Step 3: Other Child Uses Same State

`function Display({ text }) {   return <h2>{text}</h2>; }`

---

### ✅ Result

- One source of truth
    
- Both components stay in sync
    

---

## 6️⃣ Why Lifting State Works

Because:

- Parent owns the data
    
- Children communicate via props
    
- Data flow remains **one-way**
    

📌 React philosophy stays intact

---

## 7️⃣ Lifting State with Functions (Very Important)

### 🔹 Child Triggers Parent Action

`function App() {   const [count, setCount] = useState(0);    return <Counter onIncrement={() => setCount(count + 1)} />; }  function Counter({ onIncrement }) {   return <button onClick={onIncrement}>+</button>; }`

📌 Child doesn’t own state  
📌 Child requests change via function

---

## 8️⃣ Lifting State Up Between Siblings

### 🔹 Common Use Case

- Form + Preview
    
- Filters + List
    
- Tabs + Content
    

---

### Example: Filtered List

`function App() {   const [filter, setFilter] = useState("");    return (     <>       <FilterInput setFilter={setFilter} />       <ItemList filter={filter} />     </>   ); }`

---

## 9️⃣ Lifting State vs Prop Drilling

|Concept|Purpose|
|---|---|
|Lifting State|Share state between siblings|
|Prop Drilling|Side-effect of lifting too high|

📌 Lifting state **can cause prop drilling**  
📌 Context is used when lifting goes too high

---

## 🔟 When to Lift State Up

✔ Two or more components need same data  
✔ Sibling communication needed  
✔ Data consistency required

---

## 1️⃣1️⃣ When NOT to Lift State

❌ Only one component needs data  
❌ State is very local  
❌ Causes unnecessary prop drilling

---

## 1️⃣2️⃣ Common Beginner Mistakes

❌ Keeping duplicate state  
❌ Lifting state too high  
❌ Passing too many setters  
❌ Mutating state inside children

---

## 🧠 Golden Rule (Very Important)

> **One source of truth**

If multiple components show same data → state should exist once.

---

## 1️⃣3️⃣ Real-World Example (Form)

`function App() {   const [formData, setFormData] = useState({ name: "", email: "" });    return (     <>       <Form setFormData={setFormData} />       <Preview data={formData} />     </>   ); }`

📌 Live preview without duplication

---

## ✅ Summary

- Lifting state = move state to common parent
    
- Enables sibling communication
    
- Maintains one-way data flow
    
- Can lead to prop drilling if overused
    
- Context API solves deep lifting