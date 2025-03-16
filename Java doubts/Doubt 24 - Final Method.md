
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

```java
class Parent {
    final void display() {  // Final method
        System.out.println("Final method in Parent class.");
    }
}

class Child extends Parent {
    // void display() { // ❌ ERROR: Cannot override final method
    //     System.out.println("Trying to override final method.");
    // }
}

public class Main {
    public static void main(String[] args) {
        Child obj = new Child();
        obj.display();  // Output: Final method in Parent class.
    }
}
```

✅ **Explanation:**

- The `display()` method in `Parent` is `final`, so `Child` **cannot** override it.
- However, `Child` can still **inherit and use** the `display()` method.

---

## **4. Attempting to Override a Final Method (Error Example)**

```java
class Parent {
    final void show() {
        System.out.println("This is a final method in Parent.");
    }
}

class Child extends Parent {
    void show() {  // ❌ ERROR: Cannot override final method
        System.out.println("Trying to override.");
    }
}
```

🚫 **Compilation Error:**

```sh
error: show() in Child cannot override show() in Parent
  overridden method is final
```

---

## **5. Final Methods in Abstract Classes**

- **An abstract class can have a final method, but the method must have a body.**

```java
abstract class AbstractClass {
    final void finalMethod() {
        System.out.println("This is a final method in an abstract class.");
    }
}
class SubClass extends AbstractClass {
    // Cannot override finalMethod(), but can use it
}
```

---

## **6. Final Method vs. Static & Private Methods**

|Feature|`final` Method|`static` Method|`private` Method|
|---|---|---|---|
|**Can be Overridden?**|❌ No|❌ No|❌ No|
|**Can be Inherited?**|✅ Yes|✅ Yes (but hidden, not overridden)|❌ No|
|**Can be called using Class Name?**|❌ No|✅ Yes|❌ No|
|**Belongs to Object or Class?**|Object|Class|Object|

### **Example**

```java
class Example {
    final void finalMethod() {
        System.out.println("Final Method");
    }
	
    static void staticMethod() {
        System.out.println("Static Method");
    }
	
    private void privateMethod() {
        System.out.println("Private Method");
    }
}
```

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

```java
class Bank {
    final void calculateInterest() {
        System.out.println("Calculating interest in Bank.");
    }
}

class SavingsAccount extends Bank {
    // Cannot override calculateInterest()
}

public class Main {
    public static void main(String[] args) {
        SavingsAccount account = new SavingsAccount();
        account.calculateInterest();  // Output: Calculating interest in Bank.
    }
}
```

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


---

# **💡Can We Use `final` Methods in Interfaces?**

No, **interfaces cannot have `final` methods** because:

1. **Methods in an interface are implicitly `abstract` (before Java 8)** → They must be overridden in implementing classes.
2. **Even default and static methods (introduced in Java 8) are not meant to be final** → They can be overridden.

---

## **1. Attempting to Use `final` in an Interface (Compilation Error)**

🚫 **Invalid Code:**

java

Copy code

`interface MyInterface {     final void show(); // ❌ ERROR! Cannot declare final method in an interface }`

💡 **Why?**

- Interface methods must be **overridden** by implementing classes.
- A `final` method **cannot be overridden** → This contradicts the purpose of interfaces.

---

## **2. What About Default and Static Methods?**

In **Java 8 and later**, interfaces allow `default` and `static` methods.

- **Default methods** can be **overridden** in implementing classes.
- **Static methods** **cannot** be overridden but are **implicitly final**.

🚫 **Trying to make a default method final (Invalid Code)**:

java

Copy code

`interface MyInterface {     final default void show() {  // ❌ ERROR: Cannot use final with default methods         System.out.println("Default method in interface");     } }`

💡 **Error Explanation:**

- `final` prevents overriding, but default methods **must** be overridable.

---

## **3. Static Methods in Interfaces Are Implicitly Final**

✔ **Valid Code:**

java

Copy code

`interface MyInterface {     static void display() {  // ✅ Allowed, implicitly final         System.out.println("Static method in interface");     } } class Test {     public static void main(String[] args) {         MyInterface.display();  // ✅ Calling static method     } }`

💡 **Why Does This Work?**

- Static methods in interfaces **cannot be overridden** in implementing classes.
- Java **implicitly makes them `final`**.

🚫 **Trying to Override an Interface Static Method (Invalid Code)**:

java

Copy code

`class MyClass implements MyInterface {     static void display() {  // ❌ ERROR: Static method cannot be overridden         System.out.println("Trying to override");     } }`

💡 **Error Explanation:**

- A static method **belongs to the interface** and cannot be overridden.

---

## **4. Alternative: Using `final` Methods in an Abstract Class**

If you need a **final method with predefined logic**, use an **abstract class** instead of an interface.

✔ **Valid Code:**

java

Copy code

`abstract class MyAbstractClass {     final void show() {  // ✅ Final method         System.out.println("Final method in abstract class");     } } class MyClass extends MyAbstractClass {     // Cannot override show() but can use it }`

💡 **Why Use an Abstract Class Instead?**

- Abstract classes allow **concrete methods** (which can be `final`).
- Interfaces are meant for **full abstraction**, so `final` contradicts their purpose.

---

## **5. Summary**

|Feature|Interface Methods|Abstract Class Methods|
|---|---|---|
|**Can Be `final`?**|❌ No|✅ Yes|
|**Can Have `default` Methods?**|✅ Yes (Java 8+)|✅ Yes|
|**Can Have `static` Methods?**|✅ Yes (Implicitly `final`)|✅ Yes|
|**Can Be Overridden?**|✅ Yes (except static)|✅ Yes (unless `final`)|

---

### **Key Takeaways**

✔ **You cannot declare a method `final` in an interface.**  
✔ **Static methods in interfaces are implicitly final.**  
✔ **If you need a `final` method, use an `abstract class` instead of an interface.**

Would you like a practical example showing when to use an interface vs. an abstract class? 😊

4o