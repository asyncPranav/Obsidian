
---
## **🔑 Properties of Modulo**

Let’s assume:

- `a`, `b`, `c` are integers
    
- `m` is a positive integer (modulus)
    

---

### 1. **Basic definition**

```sh
a % m = remainder when a is divided by m
```

Example: `17 % 5 = 2` (since 17 = 5×3 + 2).

---

### 2. **Range of modulo**

- Result is always in range `[0, m-1]` if `a ≥ 0`.
    
- If `a` is negative, in many languages result can be negative → often we normalize by:
    

```sh
(a % m + m) % m
```

Example: `(-7 % 5 + 5) % 5 = 3`.

---

### 3. **Addition property**

```sh
(a + b) % m = ((a % m) + (b % m)) % m
```

✅ Useful to avoid overflow in big sums.

Example: `(7 + 9) % 5 = 16 % 5 = 1`  
Check: `(7 % 5 + 9 % 5) % 5 = (2 + 4) % 5 = 6 % 5 = 1`.

---

### 4. **Subtraction property**

```sh
(a - b) % m = ((a % m) - (b % m) + m) % m
```

(We add `+m` to avoid negative values.)

Example: `(7 - 9) % 5 = -2 % 5 = -2 → normalized → 3`  
Check: `(7 % 5 - 9 % 5 + 5) % 5 = (2 - 4 + 5) % 5 = 3`.

---

### 5. **Multiplication property**

```sh
(a × b) % m = ((a % m) × (b % m)) % m
```

✅ Very common in DSA (modular exponentiation, hashing, combinatorics).

Example: `(7 × 9) % 5 = 63 % 5 = 3`  
Check: `(7 % 5 × 9 % 5) % 5 = (2 × 4) % 5 = 8 % 5 = 3`.

---

### 6. **Exponentiation property**

```sh
(a ^ b) % m = ((a % m) ^ b) % m
```

(where `^` means exponentiation, not XOR).  
This is the basis of **modular exponentiation (fast power)** in O(log b).

Example: `(7^3) % 5 = 343 % 5 = 3`  
Check: `(7 % 5)^3 % 5 = (2^3) % 5 = 8 % 5 = 3`.

---

### 7. **Division (⚠️ tricky!)**

- Modular division is **not straightforward**.
    
- You cannot just do `(a / b) % m`.
    
- Proper way: multiply by modular multiplicative inverse:
    

```sh
(a / b) % m = (a × b^(-1)) % m
```

(b^(-1) means modular inverse of b modulo m).  
Works when `m` is prime and `gcd(b, m) = 1`.

---

## 🔥 In DSA, most used:

1. **(a + b) % m**
    
2. **(a × b) % m**
    
3. **(a ^ b) % m** (using fast exponentiation)
    
4. **Normalization for negatives** → `(a % m + m) % m`