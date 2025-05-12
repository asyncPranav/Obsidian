

---


### ✅ Ways of Creating **Custom Exceptions** in Java

Custom exceptions allow you to define your own error types, which helps make error handling **more meaningful and specific**.

---

### ✅ Step-by-Step: How to Create a Custom Exception

You can create a custom exception by **extending** either:

1. `Exception` → for **checked exceptions**
    
2. `RuntimeException` → for **unchecked exceptions**
    

---

### ✅ 1. **Checked Custom Exception (Extends Exception)**

```java
// Custom checked exception
class MyCheckedException extends Exception {
    public MyCheckedException(String message) {
        super(message);
    }
}
```

👉 Must be **handled using try-catch** or declared with `throws`.

#### Usage:

```java
public class TestChecked {
    public static void main(String[] args) {
        try {
            throw new MyCheckedException("This is a checked exception");
        } catch (MyCheckedException e) {
            System.out.println(e.getMessage());
        }
    }
}
```

---

### ✅ 2. **Unchecked Custom Exception (Extends RuntimeException)**

```java
// Custom unchecked exception
class MyUncheckedException extends RuntimeException {
    public MyUncheckedException(String message) {
        super(message);
    }
}
```

👉 No need to handle explicitly (optional `try-catch`).

#### Usage:

```java
public class TestUnchecked {
    public static void main(String[] args) {
        throw new MyUncheckedException("This is an unchecked exception");
    }
}
```

---

### ✅ 3. **Overriding `toString()` or `getMessage()` (Optional)**

```java
class CustomException extends Exception {
    public String toString() {
        return "Custom Exception Occurred";
    }
	
    public String getMessage() {
        return "This is a user-defined error message";
    }
}
```

---

### ✅ Summary Table

|Custom Exception Type|Inherits From|Must Handle?|Example Use Case|
|---|---|---|---|
|Checked|`Exception`|✅ Yes|File not found, age validation|
|Unchecked|`RuntimeException`|❌ Optional|Illegal argument, invalid operation|

---

Would you like **practice programs** using both checked and unchecked custom exceptions?