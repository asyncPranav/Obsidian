
---


## ✅ Java Exception Propagation and Handling – Complete Notes

### 📌 1. Exception Propagation Example (Your Original Code)

```java
public class _12_ExceptionPassingAndHandling {
    public static int meth1(){
        return 10 / 0; // Causes ArithmeticException (unchecked)
    }
	
    public static void meth2(){
        int r = meth1(); // Exception propagates to meth2
        System.out.println("Result of division is " + r); // Not executed
    }
	
    public static void meth3(){
        meth2(); // Exception propagates to meth3
    }
	
    public static void main(String[] args) {
        System.out.println("Start of the program");
        try {
            meth3(); // Exception caught here
        }
        catch (Exception e) {
            System.out.println("Exception caught: " + e);
        }
        System.out.println("End of the program");
    }
}

```

### 🔹 Key Points:

- `ArithmeticException` is an **unchecked exception** (a subclass of `RuntimeException`).
    
- Unchecked exceptions **do not need `throws`** in method signatures.
    
- Exception is propagated automatically up the call stack.
    
- Exception is handled with `try-catch` in `main()`.
    

---

### 📌 2. Using Custom Exceptions Instead of ArithmeticException

#### ➤ Two types of custom exceptions:

|Custom Exception Type|Extends|Throws Required?|Checked?|
|---|---|---|---|
|`class MyEx extends Exception`|`Exception`|✅ Yes|✅ Checked|
|`class MyEx extends RuntimeException`|`RuntimeException`|❌ No|❌ Unchecked|

---

### 📌 3. Example: Custom Checked Exception (Needs `throws`)

```java
class MyCheckedException extends Exception {
    public MyCheckedException(String message) {
        super(message);
    }
}

public class Test {
    public static int meth1() throws MyCheckedException {
        throw new MyCheckedException("Something went wrong");
    }
	
    public static void meth2() throws MyCheckedException {
        int r = meth1();
    }
	
    public static void meth3() throws MyCheckedException {
        meth2();
    }
	
    public static void main(String[] args) {
        try {
            meth3();
        } catch (MyCheckedException e) {
            System.out.println("Caught custom exception: " + e.getMessage());
        }
    }
}
```

✅ Every method that **calls a method which throws a checked exception** must either:

- Declare it using `throws`, or
    
- Catch it using `try-catch`.
    

---

### 📌 4. What if You Don't Use `throws` in Method That Throws Checked Exception?

#### ❌ Example:

java

Copy code

`public static void meth1() {     throw new MyCheckedException("Error"); // ❌ Compile-time error }`

#### ❗ Java Compiler Error:

> `unreported exception MyCheckedException; must be caught or declared to be thrown`

🛑 Java does **not allow** throwing a checked exception without either:

- Declaring `throws` in the method signature, or
    
- Catching it inside the same method.
    

Even if you **handle it in the calling method**, Java still **requires** the method that throws it to declare `throws`.

---

### 📌 5. Correct Ways to Handle Checked Exceptions

#### ✅ Option 1: Declare `throws` and Catch Later

java

Copy code

`public static void meth1() throws MyCheckedException {     throw new MyCheckedException("Error"); }  public static void main(String[] args) {     try {         meth1(); // Catching it here     } catch (MyCheckedException e) {         System.out.println("Caught: " + e.getMessage());     } }`

#### ✅ Option 2: Catch Exception in the Same Method

java

Copy code

`public static void meth1() {     try {         throw new MyCheckedException("Error");     } catch (MyCheckedException e) {         System.out.println("Handled inside meth1");     } }`

---

### ✅ Summary Table

|Case|Exception Type|Method Must Use `throws`?|Can Be Caught|
|---|---|---|---|
|`throw new ArithmeticException()`|Unchecked (`RuntimeException`)|❌ No|✅ Yes|
|`throw new MyCheckedException()` (extends `Exception`)|Checked|✅ Yes|✅ Yes|
|Method throws checked exception but no `throws` or `try-catch`|❌ Compilation Error|❌|❌|