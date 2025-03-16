
---


## **1. What is a Final Class?**

A **final class** is a class that **cannot be inherited (extended)** by any other class. The `final` keyword prevents modification of the class structure, ensuring its implementation remains unchanged.

### **Syntax:**

```java
final class Parent {  
    void show() {  
        System.out.println("This is a final class.");  
    }  
}  

class Child extends Parent {  // ❌ ERROR: Cannot inherit from final class  
}
```

🚫 **Compilation Error:**

```sh
error: cannot inherit from final Parent
```

✅ **Correct Usage:**  
You can create **objects** of a `final` class and use its methods:

```java
final class FinalClass {
    void display() {
        System.out.println("Final class method");
    }
}

public class Main {
    public static void main(String[] args) {
        FinalClass obj = new FinalClass();
        obj.display();  // Output: Final class method
    }
}
```

---

## **2. Characteristics of Final Classes**

|**Feature**|**Final Class Behavior**|
|---|---|
|**Can be Inherited?**|❌ No|
|**Can Have Methods?**|✅ Yes|
|**Can Have Constructors?**|✅ Yes|
|**Can Have Final Methods?**|✅ Yes|
|**Can Have Static Methods?**|✅ Yes|
|**Can Be Instantiated?**|✅ Yes|

---

## **3. Why Use a Final Class?**

### **✅ Prevents Inheritance**

- Stops subclasses from **modifying critical functionality** (e.g., security-related classes).

### **✅ Improves Security**

- Used in **Immutable Classes** (e.g., `String` class in Java).
- Prevents accidental modification of behavior.

### **✅ Increases Performance**

- JVM can apply **optimizations** since the class won't be extended.

---

## **4. Real-World Examples of Final Classes**

### **A. Java Built-in Final Class (`String`)**

java

Copy code

`final class String {     // Internal implementation of String class }`

🚀 **Why is `String` Final?**

- To **prevent modification of string behavior**.
- Ensures **immutability** for efficiency and security.

---

### **B. Preventing Inheritance in Banking System**

java

Copy code

`final class BankAccount {     private double balance = 1000;      void showBalance() {         System.out.println("Balance: $" + balance);     } }  // class SavingsAccount extends BankAccount {}  // ❌ ERROR: Cannot extend final class  public class Main {     public static void main(String[] args) {         BankAccount account = new BankAccount();         account.showBalance();  // Output: Balance: $1000     } }`

✅ **Why Use `final` Here?**

- Prevents **unauthorized changes** to banking logic.

---

## **5. Can a Final Class Have Final Methods?**

Yes! A **final class can have final methods** to prevent modification of critical functions.

java

Copy code

`final class SecureData {     final void encrypt() {         System.out.println("Encrypting data...");     } }`

- **The class is final → Cannot be inherited.**
- **The method is final → Cannot be overridden.**

---

## **6. Can a Final Class Have Static Methods?**

Yes! A final class can have static methods.

java

Copy code

`final class Utility {     static void printMessage() {         System.out.println("Hello from final class!");     } }  public class Main {     public static void main(String[] args) {         Utility.printMessage();  // Output: Hello from final class!     } }`

✅ **Why Use `static` in a Final Class?**

- **Utility classes** (e.g., `Math`, `Collections`) often use `static` methods inside a `final` class.

---

## **7. Can a Final Class Have a Constructor?**

Yes! **Final classes can have constructors** to initialize values.

java

Copy code

`final class Car {     private final String model;      Car(String model) {         this.model = model;     }      void show() {         System.out.println("Car model: " + model);     } }  public class Main {     public static void main(String[] args) {         Car c = new Car("Tesla");         c.show();  // Output: Car model: Tesla     } }`

✅ **Why Use a Constructor?**

- To **initialize object properties**.
- **Final class prevents inheritance, but objects can still be created!**

---

## **8. Final Class vs. Abstract Class**

|**Feature**|**Final Class**|**Abstract Class**|
|---|---|---|
|**Can be Inherited?**|❌ No|✅ Yes|
|**Can Have Abstract Methods?**|❌ No|✅ Yes|
|**Can Be Instantiated?**|✅ Yes|❌ No|
|**Purpose**|Prevents inheritance|Serves as a base class|

---

## **9. When to Use a Final Class?**

✔ **When you want to prevent inheritance (security, immutability).**  
✔ **For utility/helper classes with only static methods.**  
✔ **For classes that should not be modified (e.g., core business logic).**

---

## **10. Summary of Final Class**

|**Feature**|**Behavior**|
|---|---|
|**Can be inherited?**|❌ No|
|**Can have objects?**|✅ Yes|
|**Can have final methods?**|✅ Yes|
|**Can have static methods?**|✅ Yes|
|**Used in Java?**|`String`, `Math`, `Wrapper Classes`|

---

### **Final Thoughts**

- **Final classes prevent modification through inheritance.**
- **They improve security, performance, and maintainability.**
- **Used in immutable classes and utility classes.**

Would you like a practical coding exercise on final classes? 😊