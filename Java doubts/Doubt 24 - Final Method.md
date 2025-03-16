
---

# **Final Method in Java**

In Java, a **final method** is a method that **cannot be overridden** by subclasses. The `final` keyword ensures that the method remains **unchanged** throughout inheritance.

---

## **1. Syntax of Final Method**

A method is declared `final` using the following syntax:

```java
class Parent {
    final void show() {  // Final method
        System.out.println("This is a final method.");
    }
}
```

---

## **2. Key Characteristics of Final Methods**

1. **Cannot be Overridden:**
    
    - A subclass **cannot** change the implementation of a `final` method.
    - This prevents accidental modifications in child classes.
2. **Can be Inherited:**
    
    - A subclass can use the `final` method **as is** (without overriding it).
3. **Allowed in Abstract Classes:**
    
    - A `final` method can exist in an **abstract class** (but the method itself must have a body).
4. **Static and Private Methods are Implicitly Final:**
    
    - **Static methods** cannot be overridden, so marking them `final` is redundant.
    - **Private methods** are not inherited, so they are implicitly `final` (cannot be overridden).

---

## **3. Example of a Final Method**

### **A. Correct Usage of Final Method**

```jav
```

✅ **Explanation:**

- The `display()` method in `Parent` is `final`, so `Child` **cannot** override it.
- However, `Child` can still **inherit and use** the `display()` method.

---

## **4. Attempting to Override a Final Method (Error Example)**

java

Copy code

`class Parent {     final void show() {         System.out.println("This is a final method in Parent.");     } }  class Child extends Parent {     void show() {  // ❌ ERROR: Cannot override final method         System.out.println("Trying to override.");     } }`

🚫 **Compilation Error:**

sql

Copy code

`error: show() in Child cannot override show() in Parent   overridden method is final`

---

## **5. Final Methods in Abstract Classes**

- **An abstract class can have a final method, but the method must have a body.**

java

Copy code

`abstract class AbstractClass {     final void finalMethod() {         System.out.println("This is a final method in an abstract class.");     } } class SubClass extends AbstractClass {     // Cannot override finalMethod(), but can use it }`

---

## **6. Final Method vs. Static & Private Methods**

|Feature|`final` Method|`static` Method|`private` Method|
|---|---|---|---|
|**Can be Overridden?**|❌ No|❌ No|❌ No|
|**Can be Inherited?**|✅ Yes|✅ Yes (but hidden, not overridden)|❌ No|
|**Can be called using Class Name?**|❌ No|✅ Yes|❌ No|
|**Belongs to Object or Class?**|Object|Class|Object|

### **Example**

java

Copy code

`class Example {     final void finalMethod() {         System.out.println("Final Method");     }      static void staticMethod() {         System.out.println("Static Method");     }      private void privateMethod() {         System.out.println("Private Method");     } }`

- **Final methods** cannot be overridden.
- **Static methods** belong to the class and cannot be overridden (only hidden).
- **Private methods** cannot be accessed outside the class.

---

## **7. When to Use Final Methods?**

**✅ Use `final` methods when:**  
✔ You **want to prevent overriding** (e.g., security-sensitive logic).  
✔ You **want to ensure consistent behavior** across all subclasses.  
✔ You are **working with immutable classes** (e.g., `String`).  
✔ You are **writing framework/library code** where users should not modify the method.

---

## **8. When NOT to Use Final Methods?**

🚫 **Avoid `final` methods when:**  
❌ You expect the method to be **customized by subclasses**.  
❌ You need **flexibility** for future updates.  
❌ You are **designing an extensible framework**.

---

## **9. Practical Example: Preventing Overriding in Banking System**

java

Copy code

`class Bank {     final void calculateInterest() {         System.out.println("Calculating interest in Bank.");     } }  class SavingsAccount extends Bank {     // Cannot override calculateInterest() }  public class Main {     public static void main(String[] args) {         SavingsAccount account = new SavingsAccount();         account.calculateInterest();  // Output: Calculating interest in Bank.     } }`

✅ **Why Use `final` Here?**

- We **prevent subclasses from modifying** the core interest calculation logic.
- This ensures **consistent business rules** across all account types.

---

## **10. Summary of Final Methods**

|**Feature**|**Final Method**|
|---|---|
|**Prevents Overriding?**|✅ Yes|
|**Can be Inherited?**|✅ Yes|
|**Allowed in Abstract Classes?**|✅ Yes|
|**Allowed in Interfaces?**|❌ No|
|**Can be Used in Constructors?**|❌ No|
|**Common Use Cases**|Security, Frameworks, Preventing changes|

Would you like an example with interfaces and `final` methods? 😊