
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