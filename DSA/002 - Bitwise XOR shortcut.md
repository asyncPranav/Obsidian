
---

### **Fast mental trick (using powers of 2)**

Think of **bit positions**:

- 5 = 4 + 1 → (2² + 2⁰)
    
- 3 = 2 + 1 → (2¹ + 2⁰)
    
- XOR removes common powers (2⁰ appears in both → gone):
    
```sh
{2², 2⁰}  XOR {2¹, 2⁰}  → remove common (2⁰)
Left: {2², 2¹} → 4 + 2 = 6
```


---


Great question! Computing XOR of a sequence like:

`0 ^ 1 ^ 2 ^ 3 ^ 4`

can be **done on paper quickly** using a pattern (no need to XOR one by one).

---

### ✅ XOR Properties (needed for shortcut)

1. `a ^ a = 0` (same numbers cancel out)
    
2. `a ^ 0 = a`
    
3. XOR is **associative and commutative** → order doesn’t matter.
    
4. There is a **pattern for XOR from 0 to n**:
    


XOR (0 to n) = 

| n   | if n mod 4 = 0 |
| --- | -------------- |
| 1   | if n mod 4 = 1 |
| n+1 | if n mod 4 = 2 |
| 0   | if n mod 4 = 3 |


---

### ✅ Why this pattern?

If you write XOR results for `n = 0, 1, 2, 3, 4, 5, ...`:

```
n:      0   1   2   3   4   5   6   7   8
XOR:    0   1   3   0   4   1   7   0   8

```

Pattern repeats every 4 numbers:

`0, 1, n+1, 0, n, 1, n+1, 0, ...`

---

### ✅ Example: `0 ^ 1 ^ 2 ^ 3 ^ 4`

- Here `n = 4`
    
- `4 mod 4 = 0`
    
- So XOR from 0 to 4 = **n = 4**
    

---

### ✅ Another example: `0 ^ 1 ^ 2 ^ 3 ^ 4 ^ 5 ^ 6`

- `n = 6`
    
- `6 mod 4 = 2`
    
- So XOR = `n + 1 = 6 + 1 = 7`
    

---

### ✅ On Paper Shortcut Steps:

1. Take last number (`n`)
    
2. Find `n mod 4`
    
3. Apply rule:
    
    - If mod 4 = 0 → answer = `n`
        
    - If mod 4 = 1 → answer = `1`
        
    - If mod 4 = 2 → answer = `n + 1`
        
    - If mod 4 = 3 → answer = `0`