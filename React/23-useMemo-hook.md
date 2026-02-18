# 📘 Chapter: `useMemo` Hook – Detailed Notes (Beginner to Advanced)

---

# 1️⃣ What is `useMemo`?

`useMemo` is a **performance optimization hook** in React.

It is used to:

- **Memoize (cache) expensive calculations**
    
- Prevent unnecessary recalculations
    
- Improve performance in large applications
    

---

## 🔹 Simple Definition

> `useMemo` remembers the result of a calculation and only recalculates it when dependencies change.

---

# 2️⃣ Why Do We Need `useMemo`?

In React:

- Every time a component re-renders,
    
- All functions and calculations inside it run again.
    

Sometimes this is fine.  
But if the calculation is:

- Heavy (large loops, filtering big arrays)
    
- Expensive (sorting 10,000 items)
    
- Complex mathematical operations
    

Then unnecessary recalculations can slow down the app.

👉 That’s where `useMemo` helps.

---

# 3️⃣ Syntax of useMemo

```js
const memoizedValue = useMemo(() => {   return someExpensiveCalculation(); }, [dependencies]);
```

### Parameters:

1. Callback function → returns value
    
2. Dependency array → when to recompute
    

---

# 4️⃣ Basic Example (Without useMemo)

```js
function Example() {   const [count, setCount] = useState(0);   const [text, setText] = useState("");    const expensiveCalculation = () => {     console.log("Calculating...");     return count * 2;   };    const result = expensiveCalculation();    return (     <>       <h2>{result}</h2>       <button onClick={() => setCount(count + 1)}>Increment</button>       <input value={text} onChange={(e) => setText(e.target.value)} />     </>   ); }
```

❌ Problem:

- Even when typing in input,
    
- "Calculating..." runs again
    
- Unnecessary computation
    

---

# 5️⃣ Same Example Using useMemo

`import { useMemo } from "react";  const result = useMemo(() => {   console.log("Calculating...");   return count * 2; }, [count]);`

✅ Now:

- Calculation runs only when `count` changes
    
- Not when `text` changes
    

---

# 6️⃣ How useMemo Works Internally

Step-by-step:

1. React stores the calculated value.
    
2. On next render:
    
    - If dependencies didn’t change → return cached value.
        
    - If dependencies changed → recalculate and store new value.
        

---

# 7️⃣ When to Use useMemo?

Use it when:

✔ Heavy calculations  
✔ Filtering large lists  
✔ Sorting big arrays  
✔ Expensive computations  
✔ Derived state

Avoid it when:

❌ Simple calculations (like `count + 1`)  
❌ Premature optimization

---

# 8️⃣ Real World Example – Filtering Large List

`function ProductList({ products, search }) {    const filteredProducts = useMemo(() => {     return products.filter(product =>       product.name.toLowerCase().includes(search.toLowerCase())     );   }, [products, search]);    return (     <ul>       {filteredProducts.map(product => (         <li key={product.id}>{product.name}</li>       ))}     </ul>   ); }`

✅ Filters only when:

- products change
    
- search changes
    

Not on every re-render.

---

# 9️⃣ useMemo vs useCallback

|useMemo|useCallback|
|---|---|
|Memoizes VALUE|Memoizes FUNCTION|
|Returns computed result|Returns memoized function|
|Used for heavy calculations|Used for preventing re-creation of functions|

Example:

`const memoizedValue = useMemo(() => compute(), []); const memoizedFunction = useCallback(() => doSomething(), []);`

---

# 🔟 useMemo with Object Dependencies

Important concept ⚠

`const obj = { name: "John" };  useMemo(() => {   console.log("Runs every time!"); }, [obj]);`

❌ This runs every render because:

- Objects are recreated every time
    
- Reference changes
    

Correct way:

`const obj = useMemo(() => ({ name: "John" }), []);`

---

# 1️⃣1️⃣ Expensive Example (Large Loop)

`const expensiveCalculation = (num) => {   console.log("Running heavy task...");   for (let i = 0; i < 1000000000; i++) {}   return num * 2; };  const result = useMemo(() => {   return expensiveCalculation(count); }, [count]);`

Now it runs only when `count` changes.

---

# 1️⃣2️⃣ Dependency Array Explained

`useMemo(() => {   return calculation(); }, [a, b]);`

Re-runs only when:

- `a` changes
    
- `b` changes
    

If empty:

`useMemo(() => calculation(), []);`

Runs only once (on mount)

---

# 1️⃣3️⃣ Common Mistakes

❌ Using useMemo everywhere  
❌ Forgetting dependencies  
❌ Using it for simple logic  
❌ Thinking it prevents re-render (it doesn’t!)

Important:

> useMemo does NOT stop component from re-rendering.  
> It only memoizes values.

---

# 1️⃣4️⃣ When NOT to Use useMemo

- Small components
    
- Simple apps
    
- Light calculations
    
- When optimization is unnecessary
    

Remember:

> Premature optimization can make code harder to read.

---

# 1️⃣5️⃣ Advanced Concept – Derived State Optimization

Instead of storing derived state:

❌ Bad practice:

`const [total, setTotal] = useState(0);`

Better:

`const total = useMemo(() => {   return items.reduce((sum, item) => sum + item.price, 0); }, [items]);`

Now total updates automatically when items change.

---

# 1️⃣6️⃣ Performance Rule of Thumb

Ask yourself:

1. Is the calculation expensive?
    
2. Does it run unnecessarily?
    
3. Is performance actually affected?
    

If yes → useMemo  
If no → avoid

---

# 1️⃣7️⃣ useMemo vs React.memo

|useMemo|React.memo|
|---|---|
|Memoizes value inside component|Memoizes whole component|
|Used for calculations|Used for preventing child re-render|
|Hook|Higher Order Component|

---

# 1️⃣8️⃣ Practical Mini Project Example

Imagine Expense App:

`const totalExpense = useMemo(() => {   return expenses.reduce((sum, exp) => sum + exp.amount, 0); }, [expenses]);`

- Only recalculates when expenses change
    
- Not when input fields change
    

---

# 1️⃣9️⃣ Visual Flow

`Component Re-renders         ↓ Check Dependencies         ↓ Changed? → YES → Recalculate         ↓ Changed? → NO  → Return Cached Value`

---

# 2️⃣0️⃣ Summary

- `useMemo` optimizes expensive calculations
    
- Memoizes returned value
    
- Recalculates only when dependencies change
    
- Useful for filtering, sorting, totals, heavy loops
    
- Do not overuse
    
- Does not prevent re-renders
    

---

# 💡 Interview Questions

1. What is useMemo?
    
2. Difference between useMemo and useCallback?
    
3. Does useMemo prevent re-render?
    
4. When should we use useMemo?
    
5. What happens if dependency array is empty?
6. 