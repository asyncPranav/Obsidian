
# **📌 Exception Handling in Java**

## **1️⃣ What is an Exception?**

- An **exception** is an **unexpected event** that occurs during the execution of a program, disrupting its normal flow.
    
- It is an **object** in Java that represents an error condition.
    
- **Example:** Division by zero, accessing an invalid array index, invalid input, etc.
    

### **Example of an Exception**

```java
public class ExceptionExample {
    public static void main(String[] args) {
        int a = 5, b = 0;
        int c = a / b;  // ❌ This will cause ArithmeticException
        System.out.println("Result: " + c);
    }
}
```

🔴 **Error:** `java.lang.ArithmeticException: / by zero`

---

## **2️⃣ Types of Exceptions in Java**

Java exceptions are categorized into **two types**:

1. **Checked Exceptions** (Compile-time exceptions)
    
2. **Unchecked Exceptions** (Runtime exceptions)
    

### **📌 (A) Checked Exceptions (Compile-time Exceptions)**

- These exceptions are **checked at compile-time** by the Java compiler.
    
- If not handled, the compiler gives an error.
    
- **Example:**
    
    - `IOException`
        
    - `SQLException`
        
    - `ClassNotFoundException`
        

### **Example of Checked Exception**

```java
import java.io.File;
import java.io.FileReader;

public class CheckedExceptionExample {
    public static void main(String[] args) {
        File file = new File("test.txt");
        FileReader fr = new FileReader(file); // ❌ Compilation Error: Unhandled exception
    }
}
```

✔ **Solution:** Handle it using `try-catch` or `throws`.

---

### **📌 (B) Unchecked Exceptions (Runtime Exceptions)**

- These exceptions occur **during execution** and are **not checked** by the compiler.
    
- If not handled, the program crashes at runtime.
    
- **Example:**
    
    - `NullPointerException`
        
    - `ArrayIndexOutOfBoundsException`
        
    - `ArithmeticException`
        

### **Example of Unchecked Exception**

```java
public class UncheckedExceptionExample {
    public static void main(String[] args) {
        int[] arr = new int[3];
        System.out.println(arr[5]);  // ❌ ArrayIndexOutOfBoundsException
    }
}
```

---

## **3️⃣ Exception Handling Keywords in Java**

Java provides **five** key mechanisms to handle exceptions:

|Keyword|Description|
|---|---|
|`try`|Defines a block of code that may cause an exception.|
|`catch`|Handles the exception that occurs in the `try` block.|
|`finally`|A block that always executes (even if an exception occurs).|
|`throw`|Used to explicitly throw an exception.|
|`throws`|Declares exceptions in a method signature.|

---

## **4️⃣ Handling Exceptions Using `try-catch`**

The `try-catch` block is used to catch exceptions and prevent program termination.

### **Syntax**

```java
try {
    // Code that may cause an exception
} catch (ExceptionType e) {
    // Code to handle the exception
}
```

### **Example**

```java
public class TryCatchExample {
    public static void main(String[] args) {
        try {
            int num = 10 / 0; // ❌ ArithmeticException
        } catch (ArithmeticException e) {
            System.out.println("Exception caught: " + e);
        }
    }
}
```

✔ **Output:** `Exception caught: java.lang.ArithmeticException: / by zero`

---

## **5️⃣ Using `finally` Block**

- The `finally` block **always executes**, regardless of whether an exception occurs or not.
    
- It is mainly used for **clean-up operations** like closing files, releasing resources, etc.
    

### **Example**

```java
public class FinallyExample {
    public static void main(String[] args) {
        try {
            int num = 10 / 0;
        } catch (ArithmeticException e) {
            System.out.println("Exception caught: " + e);
        } finally {
            System.out.println("This will always execute.");
        }
    }
}
```

✔ **Output:**

```sh
Exception caught: java.lang.ArithmeticException: / by zero
This will always execute.
```

---

## **6️⃣ `throw` Keyword (Manually Throwing an Exception)**

- The `throw` keyword is used to **explicitly throw an exception** inside a method.
    

### **Example**

```java
public class ThrowExample {
    public static void checkAge(int age) {
        if (age < 18) {
            throw new ArithmeticException("Access Denied – You must be 18+.");
        } else {
            System.out.println("Access Granted.");
        }
    }
	
    public static void main(String[] args) {
        checkAge(15);  // ❌ Throws exception
    }
}
```
✔ **Output:** `Exception in thread "main" java.lang.ArithmeticException: Access Denied – You must be 18+.`

---

## **7️⃣ `throws` Keyword (Declaring Exceptions)**

- The `throws` keyword is used to declare an exception in the **method signature**.
    
- It does **not handle** the exception but **informs** the caller that an exception may occur.
    

### **Example**

```java
import java.io.*;

public class ThrowsExample {
    public static void readFile() throws IOException {
        FileReader file = new FileReader("test.txt");  // May cause IOException
    }
	
    public static void main(String[] args) {
        try {
            readFile();
        } catch (IOException e) {
            System.out.println("Exception caught: " + e);
        }
    }
}
```

---

## **8️⃣ Multiple `catch` Blocks**

- We can catch **multiple exceptions** by using multiple `catch` blocks.
    

### **Example**

```java
public class MultipleCatchExample {
    public static void main(String[] args) {
        try {
            int[] arr = new int[3];
            System.out.println(arr[5]);  // ❌ ArrayIndexOutOfBoundsException
        } catch (ArithmeticException e) {
            System.out.println("Arithmetic Exception caught.");
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("ArrayIndexOutOfBoundsException caught.");
        }
    }
}
```

✔ **Output:** `ArrayIndexOutOfBoundsException caught.`

---

## **9️⃣ Nested `try-catch`**

- A `try` block inside another `try` block is called **nested try-catch**.
    

### **Example**

```java
public class NestedTryCatchExample {
    public static void main(String[] args) {
        try {
            try {
                int num = 10 / 0;
            } catch (ArithmeticException e) {
                System.out.println("Inner catch: " + e);
            }
			
            int[] arr = new int[3];
            System.out.println(arr[5]);  // ❌ ArrayIndexOutOfBoundsException
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Outer catch: " + e);
        }
    }
}
```

---

## **🔟 Custom Exceptions**

- We can create our own **custom exception classes** by extending `Exception` or `RuntimeException`.
    

### **Example**

```java
class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}

public class CustomExceptionExample {
    public static void main(String[] args) {
        try {
            throw new CustomException("This is a custom exception!");
        } catch (CustomException e) {
            System.out.println(e.getMessage());
        }
    }
}
```

✔ **Output:** `This is a custom exception!`

---

## **📌 Summary**

|Feature|Description|
|---|---|
|`try`|Defines a block that may cause an exception.|
|`catch`|Handles the exception.|
|`finally`|Always executes, used for cleanup.|
|`throw`|Manually throws an exception.|
|`throws`|Declares exceptions in method signature.|
|`Custom Exception`|User-defined exception class.|

---

# **📌 Advanced Exception Handling in Java**



## **1️⃣ Multiple Exceptions in a Single `catch` Block (Java 7+)**

In **Java 7 and later**, we can catch multiple exceptions in a **single catch block** using `|` (OR operator).

### **Example**

```java
public class MultiExceptionExample {
    public static void main(String[] args) {
        try {
            int[] arr = new int[3];
            System.out.println(arr[5]);  // ❌ ArrayIndexOutOfBoundsException
        } catch (ArithmeticException | ArrayIndexOutOfBoundsException e) {
            System.out.println("Exception caught: " + e);
        }
    }
}
```

✔ **Output:**  
`Exception caught: java.lang.ArrayIndexOutOfBoundsException: Index 5 out of bounds for length 3`

---

## **2️⃣ Re-throwing Exceptions**

- We can **re-throw** an exception after catching it, using `throw`.
    

### **Example**

```java
public class RethrowExample {
    public static void main(String[] args) {
        try {
            divide(10, 0);
        } catch (ArithmeticException e) {
            System.out.println("Handled exception: " + e);
        }
    }
	
    public static void divide(int a, int b) throws ArithmeticException {
        try {
            int result = a / b;
        } catch (ArithmeticException e) {
            System.out.println("Exception caught, rethrowing...");
            throw e;  // Re-throwing the exception
        }
    }
}
```

✔ **Output:**

csharp

Copy code

`Exception caught, rethrowing... Handled exception: java.lang.ArithmeticException: / by zero`

---

## **3️⃣ Exception Propagation**

- If a method **does not handle** an exception, it **propagates** to the caller method.
    

### **Example**

```java
public class ExceptionPropagationExample {
    public static void main(String[] args) {
        method1(); // Calls method2()
    }
	
    public static void method1() {
        method2(); // Calls method3()
    }
	
    public static void method2() {
        method3(); // Throws exception
    }
	
    public static void method3() {
        int num = 10 / 0;  // ❌ ArithmeticException occurs
    }
}
```

✔ **Output:**

```sh
Exception in thread "main" java.lang.ArithmeticException: / by zero
```

🔹 Since **method3()** does not handle the exception, it propagates to **method2()**, then to **method1()**, and finally to `main()` where the program crashes.

✔ **Fix:** Add `try-catch` in `method3()` or `method1()`.

---

## **4️⃣ Using `try-with-resources` (Java 7+)**

- The **`try-with-resources`** statement **automatically closes** resources like files, sockets, or database connections.
    

### **Example**

```java
import java.io.*;

public class TryWithResourcesExample {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("test.txt"))) {
            System.out.println(br.readLine());
        } catch (IOException e) {
            System.out.println("Exception caught: " + e);
        }
    }
}
```

✔ **Why use `try-with-resources`?**

- No need to manually close the resource using `finally`.
    
- Reduces memory leaks.
    

---

## **5️⃣ Custom Exception with Multiple Constructors**

A **custom exception** can have multiple constructors like built-in exceptions.

### **Example**

```java
class AgeException extends Exception {
    public AgeException(String message) {
        super(message);
    }
	
    public AgeException(String message, Throwable cause) {
        super(message, cause);
    }
}

public class CustomExceptionExample {
    public static void main(String[] args) {
        try {
            checkAge(15);
        } catch (AgeException e) {
            System.out.println("Custom Exception caught: " + e.getMessage());
        }
    }
	
    public static void checkAge(int age) throws AgeException {
        if (age < 18) {
            throw new AgeException("Age must be 18 or above.");
        }
    }
}
```

✔ **Output:**  
`Custom Exception caught: Age must be 18 or above.`

---

## **6️⃣ Chained Exceptions (`initCause()` & `getCause()`)**

- **Chained exceptions** provide information about the root cause of an exception.
    

### **Example**

```java
public class ChainedExceptionExample {
    public static void main(String[] args) {
        try {
            throwException();
        } catch (Exception e) {
            System.out.println("Caught Exception: " + e);
            System.out.println("Actual Cause: " + e.getCause());
        }
    }
	
    public static void throwException() throws Exception {
        try {
            int num = 10 / 0;  // ❌ ArithmeticException
        } catch (ArithmeticException e) {
            throw new Exception("Higher-level exception", e); // Chaining exception
        }
    }
}
```

✔ **Output:**

pgsql

Copy code

`Caught Exception: java.lang.Exception: Higher-level exception Actual Cause: java.lang.ArithmeticException: / by zero`

---

## **7️⃣ Difference Between `throw` and `throws`**

|Feature|`throw`|`throws`|
|---|---|---|
|Purpose|Used to **explicitly throw** an exception.|Used to **declare** exceptions in method signature.|
|Usage|Inside a method/block.|In method declaration.|
|Example|`throw new ArithmeticException("Error")`|`void read() throws IOException`|
|Handles Exception?|No, it only throws an exception.|No, it just informs that an exception might occur.|

✔ **Example of `throw`**

java

Copy code

`public class ThrowExample {     public static void main(String[] args) {         throw new RuntimeException("Manually thrown exception");     } }`

✔ **Example of `throws`**

java

Copy code

`public class ThrowsExample {     public static void riskyMethod() throws IOException {         throw new IOException("File error");     }      public static void main(String[] args) {         try {             riskyMethod();         } catch (IOException e) {             System.out.println("Exception caught: " + e);         }     } }`

---

## **8️⃣ Common Java Exceptions**

|Exception|Description|
|---|---|
|`ArithmeticException`|Division by zero (`10/0`).|
|`NullPointerException`|Accessing methods on `null` object.|
|`ArrayIndexOutOfBoundsException`|Accessing array out of its limit.|
|`NumberFormatException`|Converting invalid string to number.|
|`IllegalArgumentException`|Passing illegal arguments to a method.|
|`IOException`|File handling error.|
|`SQLException`|Database access error.|

✔ **Example of `NumberFormatException`**

java

Copy code

`public class NumberFormatExample {     public static void main(String[] args) {         String str = "abc";         int num = Integer.parseInt(str);  // ❌ NumberFormatException     } }`

---

## **9️⃣ Best Practices for Exception Handling**

✔ ✅ **DO:**

- Use **specific exceptions** instead of `Exception` or `Throwable`.
    
- Use `finally` to **close resources**.
    
- Use **logging** instead of `printStackTrace()`.
    
- Use `throw` for **custom exceptions** where needed.
    
- Use **checked exceptions** for recoverable errors (e.g., file not found).
    

❌ **DON'T:**

- Don't **catch generic `Exception`** unless necessary.
    
- Don't use empty `catch` blocks (it hides errors).
    
- Don't overuse exceptions for normal flow (e.g., using `try-catch` instead of `if`).
    

---

### **📌 Summary**

✔ Java provides powerful **exception handling** mechanisms.  
✔ **Use `try-catch`, `finally`, `throw`, and `throws` appropriately**.  
✔ **Understand checked vs unchecked exceptions**.  
✔ **Use best practices** to write maintainable and error-free code.