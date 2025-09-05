
---

## ✅ **Method 01: Basic Method (Brute Force)**

`static boolean isPrime1(int n) {     if (n <= 1) return false;     for (int i = 2; i < n; i++) {         if (n % i == 0) return false;     }     return true; }`

### **Time Complexity:** `O(n)`

- Checks every number from 2 to n-1.
    
- Very slow for large `n`.
    

---

## ✅ **Method 02: Square Root Optimization**

`static boolean isPrime2(int n) {     if (n <= 1) return false;     for (int i = 2; i <= Math.sqrt(n); i++) {         if (n % i == 0) return false;     }     return true; }`

### **Time Complexity:** `O(√n)`

- Only checks up to √n because:
    
    `If n = a × b, then one of a or b ≤ √n`
    

---

## ✅ **Method 03: 6k ± 1 Optimization**

We know:

- All primes greater than 3 are in the form **6k ± 1** (except 2 and 3).
    
- So, check 2 and 3 first, then skip multiples of 2 and 3.
    

`static boolean isPrime3(int n) {     if (n <= 1) return false;     if (n <= 3) return true;     if (n % 2 == 0 || n % 3 == 0) return false;          for (int i = 5; i * i <= n; i += 6) {         if (n % i == 0 || n % (i + 2) == 0) return false;     }     return true; }`

### **Time Complexity:** `O(√n / 6)` (still `O(√n)` but 3x faster)

- Checks fewer numbers by skipping unnecessary checks.
    

---

## ✅ **Method 04: Precompute using Sieve of Eratosthenes**

If you need to check primes **many times**, use the **Sieve of Eratosthenes**:

`static boolean[] sieve(int n) {     boolean[] isPrime = new boolean[n + 1];     Arrays.fill(isPrime, true);     isPrime[0] = isPrime[1] = false;      for (int i = 2; i * i <= n; i++) {         if (isPrime[i]) {             for (int j = i * i; j <= n; j += i) {                 isPrime[j] = false;             }         }     }     return isPrime; }`

### **Time Complexity:** `O(n log log n)`

- Best when you need primes up to `n` multiple times.
    

---

### ✅ **Summary Table**

|Method|Time Complexity|When to Use|
|---|---|---|
|Brute Force|O(n)|Very small numbers only|
|√n Optimization|O(√n)|Single check, large numbers|
|6k ± 1 Optimization|O(√n)|Faster single checks|
|Sieve|O(n log log n)|Many checks up to n|