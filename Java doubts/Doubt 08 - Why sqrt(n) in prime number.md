
```java
import java.util.Scanner;

public class PrimeCheck {
    public static boolean isPrime(int n) {
        if (n < 2) return false;
        for (int i = 2; i * i <= n; i++) {
            if (n % i == 0) return false;
        }
        return true;
    }
	
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter a number: ");
        int num = scanner.nextInt();
        scanner.close();
		
        if (isPrime(num)) {
            System.out.println(num + " is a Prime Number.");
        } else {
            System.out.println(num + " is NOT a Prime Number.");
        }
    }
}

```



When checking whether a number n is prime, we use n\sqrt{n}n​ as an optimization to reduce the number of checks. The logic behind this is based on the **properties of factors**:

### **Key Idea: Factors Appear in Pairs**

Every composite number n can be expressed as the product of two factors:

$$
n=a×b
$$

If both a and b were greater than sqrt(n) , their product would be greater than n, which is a contradiction. Therefore, **at least one of the factors must be ≤ sqrt(n) .

#DOUBT **- what is Factor pair ??**

A factor pair of a number n is **two numbers that multiply together to give n**.

For example, let's take n=36.  

The factor pairs of 36 are:
	1×36=36 
	2×18=36
	3×6=36
	4×9=36
	6×6=36
Once we reach **6 × 6**, the factors **start repeating** in reverse:
	9×4=36
	12×3=36
	18×2=36
	36×1=36

So, after we reach 6, we don’t need to check **9, 12, 18, 36**, because they already appeared before (just reversed).

That means:  
👉 **If we don’t find a factor before sqrt(36) = 6, we won’t find one later either!**

---

### **Why Does This Matter for Prime Numbers?**

A prime number **has no factors** except **1 and itself**.

#### **Example: Checking if 37 is Prime**

We want to check if 37 is prime.  
Instead of checking all numbers from **2 to 36**, we **only check up to**
$$
\sqrt{37} ≈6
$$
So, we check: **2, 3, 4, 5, 6**

- None of them divide 37.
- That means **no larger number will divide 37 either** (because its pair would be smaller than 6).
- Since we found **no factors**, **37 is prime**.

---

### **Final Answer: Why Use sqrt{n} ​?**

✅ **If a number n has a factor larger than sqrt(n), then its pair factor must be smaller than sqrt(n).**  
✅ **If no number ≤ sqrt(n)​ divides n, then no number will.**  
✅ **This makes checking for primes much faster!**

