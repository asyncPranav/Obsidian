
---

### **Floating-Point Representation**

**Definition:**  
Floating-point representation is a method of representing real numbers in a computer in the form of a **mantissa and exponent**, allowing both very large and very small numbers to be expressed efficiently.

---

**General Form:**

±m×be\boxed{\pm m \times b^e}±m×be​

Where:

- mmm = **Mantissa (or significand)** – represents the significant digits of the number.
    
- bbb = **Base** (commonly 2 in binary systems).
    
- eee = **Exponent** – determines the scaling or magnitude of the number.
    
- ±\pm± = Sign of the number (positive or negative).
    

---

**Explanation of Terms:**

1. **Mantissa (m):**
    
    - Contains the actual digits of the number.
        
    - Typically normalized so that the first digit is non-zero (e.g., 1.2341.2341.234 in binary normalized form).
        
2. **Exponent (e):**
    
    - Indicates the power of the base by which mantissa is multiplied.
        
    - Allows the number to “float” across a wide range of values.
        

---

**Why Floating-Point Representation is Used:**

1. **Large Range:**
    
    - Can represent extremely large or extremely small numbers which cannot be stored in fixed-point form.
        
2. **Precision:**
    
    - Maintains significant digits accurately.
        
    - Reduces loss of information compared to simple integer or fixed-point storage.
        
3. **Flexibility:**
    
    - Useful in scientific calculations, simulations, and engineering computations where numbers vary widely in magnitude.
        

---

**Example:**  
Decimal number 0.000560.000560.00056 in floating-point binary form:

0.00056=5.6×10−40.00056 = 5.6 \times 10^{-4} 0.00056=5.6×10−4

Here, 5.65.65.6 is the **mantissa**, and −4-4−4 is the **exponent**.

---

**Key Point to Remember for Exam:**

- Normalization ensures **mantissa starts with non-zero digit**.
    
- Floating-point gives **both range and precision**, which is **impossible with simple integer representation**.




---

### **Normalized Floating-Point Numbers**

**1. Definition of Normalization:**  
Normalization is the process of adjusting the **mantissa** and **exponent** in a floating-point number so that the mantissa falls within a **specific range**, usually making the first digit non-zero.

**Purpose:** To **maximize precision** and maintain a **unique representation**.

---

**2. Rule of Normalization:**

- In **binary systems:** The normalized form requires the mantissa to satisfy
    

1≤∣m∣<21 \le |m| < 21≤∣m∣<2

- In **decimal systems:**
    

1≤∣m∣<101 \le |m| < 101≤∣m∣<10

- The exponent is adjusted accordingly to keep the value of the number unchanged.
    

---

**3. Conversion of a Number into Normalized Form:**

**Example 1 (Decimal):**  
Convert 0.00560.00560.0056 to normalized form:

0.0056=5.6×10−30.0056 = 5.6 \times 10^{-3} 0.0056=5.6×10−3

- Mantissa: 5.65.65.6 (between 1 and 10)
    
- Exponent: −3-3−3
    

**Example 2 (Binary):**  
Convert 0.101120.1011_20.10112​ to normalized form:

0.10112=1.011×2−20.1011_2 = 1.011 \times 2^{-2} 0.10112​=1.011×2−2

- Mantissa: 1.0111.0111.011
    
- Exponent: −2-2−2
    

---

**4. Consequences of Normalization:**

1. **Increased Precision:**
    
    - All significant digits are preserved in mantissa, reducing loss of information.
        
2. **Reduced Rounding Error:**
    
    - Standardized mantissa minimizes errors when performing arithmetic operations.
        
3. **Uniform Representation:**
    
    - Every number has a **unique normalized form**, simplifying comparisons and calculations in computers.
        

---

**Key Point for Exam:**

- Always remember: **Normalized → Mantissa starts with non-zero digit**.
    
- This ensures **maximum precision** and **consistent storage**.


---


### **Significant Digits**

**1. Definition:**  
Significant digits (or significant figures) of a number are the **meaningful digits** that contribute to its **accuracy**.

- They include all non-zero digits, zeros between non-zero digits, and trailing zeros **after a decimal point**.
    
- Leading zeros are **not significant**.
    

---

**2. Rules for Counting Significant Digits:**

1. **All non-zero digits** are significant.
    
    - Example: 123.45123.45123.45 → 5 significant digits
        
2. **Zeros between non-zero digits** are significant.
    
    - Example: 100210021002 → 4 significant digits
        
3. **Leading zeros** (before the first non-zero digit) are **not significant**.
    
    - Example: 0.00250.00250.0025 → 2 significant digits
        
4. **Trailing zeros** after a decimal point are significant.
    
    - Example: 25.0025.0025.00 → 4 significant digits
        
5. **Trailing zeros in whole numbers without decimal point** may or may not be significant; use scientific notation to clarify.
    
    - Example: 500500500 → 1, 2, or 3 significant digits depending on context
        

---

**3. Relationship with Accuracy:**

- **More significant digits → Higher accuracy**
    
- The **accuracy of a measurement or computation** is directly related to the number of significant digits it contains.
    
- **Rounding off** reduces significant digits and may decrease precision.
    

**Example:**

- Number: 3.141593.141593.14159 → 6 significant digits → very precise
    
- Number: 3.143.143.14 → 3 significant digits → less precise
    

---

**Key Point for Exam:**

- **Significant digits indicate the precision of a number**, not its magnitude.



---


### **Types of Errors in Numerical Computation**

In numerical computation, **errors** are the differences between the **true value** and the **approximate or computed value**. The main types of errors are:

---

**1. Truncation Error:**

- **Definition:** Error caused when an **infinite process** is approximated by a **finite process**.
    
- **Example:** Using first 3 terms of a series to approximate a function instead of the full infinite series.
    
- **Cause:** Ignoring higher-order terms in a formula or series.
    

---

**2. Rounding Error:**

- **Definition:** Error introduced when a number is **rounded off** to a limited number of significant digits.
    
- **Example:** Approximating 3.141592653.141592653.14159265 as 3.1423.1423.142 for calculations.
    
- **Cause:** Limited precision in computer storage or manual rounding.
    

---

**3. Inherent (or Pre-existing) Error:**

- **Definition:** Error already present in **measured or given data** before computation.
    
- **Example:** Measuring a length with a ruler that has a smallest division of 1 mm → true value has small inherent error.
    
- **Cause:** Limitations of instruments, human errors, or imprecise initial data.
    

---

**Summary Table:**

|Type of Error|Cause|Example|
|---|---|---|
|Truncation Error|Approximating infinite process|Using 3 terms of Taylor series|
|Rounding Error|Limiting number of digits|3.14159265 → 3.142|
|Inherent Error|Measurement/data limitation|Ruler reads ±1 mm|

---

**Key Point for Exam:**

- Understanding these errors is **essential for numerical accuracy**.
    
- **Total error** in computation may be a combination of these three types.




---


