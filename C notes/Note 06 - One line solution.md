
Here are some **one-liner** solutions to common problems in **C language**:

### **1️⃣ Factorial of a Number**

```c
int fact(int n) { return n == 0 ? 1 : n * fact(n - 1); }
```

**Logic:**

- Uses **recursion**: `fact(n) = n * fact(n-1)`.
- Base case: If `n == 0`, return `1` (since `0! = 1`).


### **2️⃣ Count Digits in a Number**

```c
int countDigits(int n) { return n == 0 ? 1 : (int) log10(n) + 1; }
```

**Logic:**

- Uses the **logarithm property**: `log10(n) + 1` gives the number of digits.
- `log10(100) = 2`, so `100` has `3` digits.
- If `n == 0`, return `1` (since `0` has 1 digit).
- Requires `<math.h>` for `log10()` function.


### **3️⃣ Reverse a Number**

```c
int reverse(int n) { int rev = 0; while (n) rev = rev * 10 + n % 10, n /= 10; return rev; }
```

**Logic:**

- Extracts digits from `n` using `% 10` and builds `rev` by shifting left (`rev * 10`).
- Example: `n = 123` →
    1. `rev = 0 * 10 + 3 = 3`, `n = 12`
    2. `rev = 3 * 10 + 2 = 32`, `n = 1`
    3. `rev = 32 * 10 + 1 = 321`, `n = 0`


### **4️⃣ Check if a Number is Palindrome**

```c
int isPalindrome(int n) { return n == reverse(n); }
```

**Logic:**

- A palindrome number reads the same forward and backward.
- Compares `n` with its reversed version.



### **5️⃣ Check if a Number is Prime**

```c
int isPrime(int n) { for (int i = 2; i * i <= n; i++) if (n % i == 0) return 0; return n > 1; }
```

**Logic:**

- Loops from `2` to `sqrt(n)`, checking if `n` is divisible by `i`.
- If any `i` divides `n`, return `0` (not prime).
- If loop completes, return `1` (prime).



### **6️⃣ Sum of Digits of a Number**

```c
int sumDigits(int n) { return n == 0 ? 0 : n % 10 + sumDigits(n / 10); }
```

**Logic:**

- Uses **recursion** to extract digits.
- Adds last digit (`n % 10`) to the sum of remaining digits.



### **7️⃣ Check if a Year is a Leap Year**

```c
int isLeap(int year) { return (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0); }
```

**Logic:**

- Leap year rules:
    1. Divisible by `4` **AND** not by `100` (e.g., 2024).
    2. OR divisible by `400` (e.g., 2000 but not 1900).



### **8️⃣ Swap Two Numbers Without Using a Third Variable**

```c
void swap(int *a, int *b) { *a = *a + *b - (*b = *a); }
```

**Logic:**

- `*b = *a` stores `*a` in `*b`,
- `*a = *a + *b - *b` effectively swaps values.



### **🔟 Swap Two Numbers Using XOR**

```c
int x = 5, y = 7;
x ^= y ^= x ^= y;
```

**Logic:**

- Uses **bitwise XOR** to swap numbers efficiently.
- XOR properties:
    - `a ^ a = 0`
    - `a ^ 0 = a`
- This swaps without using extra space.



### **1️⃣1️⃣ Convert Lowercase to Uppercase (char c)**

```c
char c = 'b', upper = (c >= 'a' && c <= 'z') ? c - 32 : c;
```

**Logic:**

- ASCII value of `'a'` is `97`, `'A'` is `65` (difference = `32`).
- If `c` is between `'a'` and `'z'`, subtract `32` to convert.

### **1️⃣2️⃣ Convert uppercase to lowercase (char c)**

```c
char c = 'B', lower = (c >= 'A' && c <= 'Z') ? c + 32 : c;
```

**Logic:**

- If `c` is between `'A'` and `'Z'`, add `32` to convert to lowercase.
