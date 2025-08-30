
---

# **Bitwise Left Shift and MSB Discarding – Detailed Notes**

---

## ✅ **1. What is Left Shift (`<<`) in Java?**

- The **left shift operator (`<<`)** shifts all bits of a number **to the left** by the specified number of positions.
    
```java
number << n

```
    
Means: Move each bit `n` positions left and **append 0s on the right**.


---

### ✅ **2. Rules of Left Shift**

1. Each shift **multiplies the number by 2** (for positive integers).
    
2. **LSB (Least Significant Bit)** becomes `0` after each shift.
    
3. **MSB (Most Significant Bit)** is **discarded only if it goes beyond the fixed size of the data type**.
    
4. In Java:
    
    - `int` = 32 bits
        
    - `long` = 64 bits
        
    - So, overflow happens only when shifted beyond these bit limits.
        

---

## ✅ **3. Why didn’t we discard MSB in `1111 << 1`?**

- You saw:
    
```sh
1111 << 1 → 11110
```
    
- Here, we **did not discard MSB** because we **did not define a fixed width** in the example.
	
- But in Right Shift we discard left bit 
	
```sh
1111 >> 1 → 0111
```
    
- In **Java**, `int` is **32 bits**, so the real representation is:
	
```sh
00000000 00000000 00000000 00001111  (15)
After << 1:
00000000 00000000 00000000 00011110  (30)
```
    
- No MSB was lost because there was **room to shift inside 32 bits**. But in Right shift there was not any vacant bit for storing right shifted bit that's why it get discarded.


---

### ✅ **When do we discard MSB?**

- If you keep shifting and the `1` reaches the left edge (bit 31 for int), the next shift discards it:
    
```sh
Before shift:
10000000 00000000 00000000 00000000  (MSB set)
After << 1:
00000000 00000000 00000000 00000000  (MSB lost)
```

---

## ✅ **4. Difference Between Conceptual Example & Java Implementation**

- **Conceptual (simplified)**: `1111 << 1` = `11110` (we add zero on right, no fixed limit).
    
- **Java (32-bit)**: Always in 32 bits → overflow occurs only after 32 shifts.
    

---

## ✅ **5. Key Facts**

✔ Left shift (`<<`) = Multiply by 2 (positive numbers).  
✔ Adds `0` on right (LSB).  
✔ MSB is discarded **only when overflow happens beyond the data type width**.  
✔ For signed integers:

- Overflow **does not throw an error**; it wraps around using **2’s complement**.
    

---

## ✅ **6. Visual Example**

Example: `15 << 1`

```sh
Binary: 00000000 00000000 00000000 00001111  (15)
Shift:  00000000 00000000 00000000 00011110  (30)
```

After multiple shifts:

```sh
15 << 27 → 1111000000000000000000000000000
15 << 28 → 11110000000000000000000000000000
15 << 29 → overflow, MSB gone
```

---

## ✅ **7. Quick Comparison**

|**Operation**|**What Happens**|
|---|---|
|`n << 1`|Multiply by 2 (add zero at right)|
|MSB loss?|Only when shifting beyond width (32)|

---

### **Extra Tip:**

In **right shift (`>>`)**, we lose the **LSB** and add either **0 (logical shift)** or **sign bit (arithmetic shift)** on the left.

---

🔥 **Summary in One Line:**  
MSB in left shift is discarded **only when the bit moves out of the fixed-size boundary (32 bits in int)**, which doesn’t happen in small examples like `1111 << 1`.