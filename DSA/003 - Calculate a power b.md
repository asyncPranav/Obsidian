
---

### ✅ What does `5 = (1×2²)+(0×2¹)+(1×2⁰)` mean?

- Every number can be written as a sum of powers of 2.
    
- `5` in binary is `101`.
    
    - The rightmost bit (1) = 2^0= 1
        
    - The middle bit (0) = 2^1= 2
        
    - The leftmost bit (1) = 2^2 = 4
        

So:

5=(1×4)+(0×2)+(1×1)=4+0+1=55 = (1 \times 4) + (0 \times 2) + (1 \times 1) = 4 + 0 + 1 = 55=(1×4)+(0×2)+(1×1)=4+0+1=5



---

### ✅ How does this connect to a5a^5a5?

If `b = 5`, then:

a5=a(4+0+1)a^5 = a^{(4 + 0 + 1)}a5=a(4+0+1)

Now use **exponent rule**:

ax+y=ax×aya^{x+y} = a^x \times a^yax+y=ax×ay

So:

a5=a(4+1)=a4×a1a^5 = a^{(4 + 1)} = a^4 \times a^1a5=a(4+1)=a4×a1

We **skip the zero part** because multiplying by a0=1a^0 = 1a0=1 doesn’t change anything.

---

### ✅ So the binary tells us which powers of 2 to include

- Bit 1 (rightmost): 1 → include a1a^{1}a1
    
- Bit 2: 0 → skip a2a^{2}a2
    
- Bit 3: 1 → include a4a^{4}a4
    

So:

a5=a4×a1a^5 = a^4 \times a^1a5=a4×a1

---

### ✅ Why is this helpful?

Because instead of multiplying `a` five times (slow), we:

- Compute a1a^1a1
    
- Square → a2a^2a2
    
- Square → a4a^4a4
    
- Pick the ones where the binary bit = 1.
    

This is the entire trick!

---

#### **Check with numbers**

Example: a=2a = 2a=2, b=5b = 5b=5

25=24×21=16×2=322^5 = 2^4 \times 2^1 = 16 \times 2 = 3225=24×21=16×2=32

Binary told us: take 4 and 1, skip 2 → works perfectly.