
Here are some **one-liner** solutions to common problems in **C language**:

### **1️⃣ Factorial of a Number**

```c
int fact(int n) { return n == 0 ? 1 : n * fact(n - 1); }
```

### **2️⃣ Count Digits in a Number**

```c
int countDigits(int n) { return n == 0 ? 1 : (int) log10(n) + 1; }
```
(Include `<math.h>` for `log10`)

### **3️⃣ Reverse a Number**

```c
int reverse(int n) { int rev = 0; while (n) rev = rev * 10 + n % 10, n /= 10; return rev; }
```

### **4️⃣ Check if a Number is Palindrome**

```c
int isPalindrome(int n) { return n == reverse(n); }
```

### **5️⃣ Check if a Number is Prime**

```c
int sumDigits(int n) { return n == 0 ? 0 : n % 10 + sumDigits(n / 10); }
```

### **6️⃣ Sum of Digits of a Number**

```c

```

### **7️⃣ Check if a Year is a Leap Year**

```c

```

### **8️⃣ Swap Two Numbers Without Using a Third Variable**

```c

```

Let me know if you need more! 🚀

4o