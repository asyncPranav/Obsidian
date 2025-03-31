

```java
package $18_Exception_Handling;  
  
import java.util.Scanner;  
  
public class _21_Calculator {  
    public static void main(String[] args) {  
        Scanner sc = new Scanner(System.in);  
        System.out.print("Enter first number : ");  
        float a = sc.nextFloat();  
        System.out.print("Enter operator : ");  
        String op = sc.next();  
        System.out.print("Enter first number : ");  
        float b = sc.nextFloat();  
        float result=0f;  
        switch (op){  
            case "+" -> result = a+b;  
            case "-" -> result = a-b;  
            case "*" -> result = a*b;  
            case "/" -> result = a/b;  
            default -> System.out.println("Invalid operator");  
        }  
        System.out.println("Result : "+result);   
    }  
}
```

Your code **does not throw a runtime exception** when dividing by zero because of how **floating-point division** (`float` or `double`) works in Java.

### **Why No Exception?**

In Java:

- **Integer division (`int / int`)** throws `ArithmeticException: / by zero`.
    
- **Floating-point division (`float / float` or `double / double`)** follows IEEE 754 standards, where:
    
    - `x / 0.0` results in **Infinity** (`∞`) if `x > 0`.
        
    - `x / 0.0` results in **-Infinity** (`-∞`) if `x < 0`.
        
    - `0.0 / 0.0` results in **NaN** (Not a Number).
        

### **Example Output for `a = 5` and `b = 0`**

```sh
Enter first number : 5
Enter operator : /
Enter second number : 0
Result : Infinity
```

Since `5 / 0.0` produces **Infinity**, no exception is thrown.

---

### **How to Handle Division by Zero?**

If you want to **manually throw an exception** for division by zero, modify your code like this:

```java
case "/" -> {
    if (b == 0) {
        throw new ArithmeticException("Cannot divide by zero");
    }
    result = a / b;
}
```

Now, if `b = 0`, it will throw:

```sh
Exception in thread "main" java.lang.ArithmeticException: Cannot divide by zero
```
