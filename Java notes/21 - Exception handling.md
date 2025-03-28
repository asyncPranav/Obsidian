
# **📌 Exception Handling in Java**

## **1️⃣ What is an Exception?**

- An **exception** is an **unexpected event** that occurs during the execution of a program, disrupting its normal flow.
    
- It is an **object** in Java that represents an error condition.
    
- **Example:** Division by zero, accessing an invalid array index, invalid input, etc.
    

### **Example of an Exception**

```java
v
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

java

Copy code

`import java.io.File; import java.io.FileReader;  public class CheckedExceptionExample {     public static void main(String[] args) {         File file = new File("test.txt");         FileReader fr = new FileReader(file); // ❌ Compilation Error: Unhandled exception     } }`

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

java

Copy code

`public class UncheckedExceptionExample {     public static void main(String[] args) {         int[] arr = new int[3];         System.out.println(arr[5]);  // ❌ ArrayIndexOutOfBoundsException     } }`

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

java

Copy code

`try {     // Code that may cause an exception } catch (ExceptionType e) {     // Code to handle the exception }`

### **Example**

java

Copy code

`public class TryCatchExample {     public static void main(String[] args) {         try {             int num = 10 / 0; // ❌ ArithmeticException         } catch (ArithmeticException e) {             System.out.println("Exception caught: " + e);         }     } }`

✔ **Output:** `Exception caught: java.lang.ArithmeticException: / by zero`

---

## **5️⃣ Using `finally` Block**

- The `finally` block **always executes**, regardless of whether an exception occurs or not.
    
- It is mainly used for **clean-up operations** like closing files, releasing resources, etc.
    

### **Example**

java

Copy code

`public class FinallyExample {     public static void main(String[] args) {         try {             int num = 10 / 0;         } catch (ArithmeticException e) {             System.out.println("Exception caught: " + e);         } finally {             System.out.println("This will always execute.");         }     } }`

✔ **Output:**

pgsql

Copy code

`Exception caught: java.lang.ArithmeticException: / by zero This will always execute.`

---

## **6️⃣ `throw` Keyword (Manually Throwing an Exception)**

- The `throw` keyword is used to **explicitly throw an exception** inside a method.
    

### **Example**

java

Copy code

`public class ThrowExample {     public static void checkAge(int age) {         if (age < 18) {             throw new ArithmeticException("Access Denied – You must be 18+.");         } else {             System.out.println("Access Granted.");         }     }      public static void main(String[] args) {         checkAge(15);  // ❌ Throws exception     } }`

✔ **Output:** `Exception in thread "main" java.lang.ArithmeticException: Access Denied – You must be 18+.`

---

## **7️⃣ `throws` Keyword (Declaring Exceptions)**

- The `throws` keyword is used to declare an exception in the **method signature**.
    
- It does **not handle** the exception but **informs** the caller that an exception may occur.
    

### **Example**

java

Copy code

`import java.io.*;  public class ThrowsExample {     public static void readFile() throws IOException {         FileReader file = new FileReader("test.txt");  // May cause IOException     }      public static void main(String[] args) {         try {             readFile();         } catch (IOException e) {             System.out.println("Exception caught: " + e);         }     } }`

---

## **8️⃣ Multiple `catch` Blocks**

- We can catch **multiple exceptions** by using multiple `catch` blocks.
    

### **Example**

java

Copy code

`public class MultipleCatchExample {     public static void main(String[] args) {         try {             int[] arr = new int[3];             System.out.println(arr[5]);  // ❌ ArrayIndexOutOfBoundsException         } catch (ArithmeticException e) {             System.out.println("Arithmetic Exception caught.");         } catch (ArrayIndexOutOfBoundsException e) {             System.out.println("ArrayIndexOutOfBoundsException caught.");         }     } }`

✔ **Output:** `ArrayIndexOutOfBoundsException caught.`

---

## **9️⃣ Nested `try-catch`**

- A `try` block inside another `try` block is called **nested try-catch**.
    

### **Example**

java

Copy code

`public class NestedTryCatchExample {     public static void main(String[] args) {         try {             try {                 int num = 10 / 0;             } catch (ArithmeticException e) {                 System.out.println("Inner catch: " + e);             }              int[] arr = new int[3];             System.out.println(arr[5]);  // ❌ ArrayIndexOutOfBoundsException         } catch (ArrayIndexOutOfBoundsException e) {             System.out.println("Outer catch: " + e);         }     } }`

---

## **🔟 Custom Exceptions**

- We can create our own **custom exception classes** by extending `Exception` or `RuntimeException`.
    

### **Example**

java

Copy code

`class CustomException extends Exception {     public CustomException(String message) {         super(message);     } }  public class CustomExceptionExample {     public static void main(String[] args) {         try {             throw new CustomException("This is a custom exception!");         } catch (CustomException e) {             System.out.println(e.getMessage());         }     } }`

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

🔹 Let me know if you need **more examples or explanations**! 🚀😊