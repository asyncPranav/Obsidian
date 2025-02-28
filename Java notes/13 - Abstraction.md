# 📝 **Abstraction and Abstract Classes in Java: Comprehensive Notes**

---

## 🌟 **1. What is Abstraction?**

### 🎯 **Definition:**

**Abstraction** is an **Object-Oriented Programming (OOP)** concept that focuses on **hiding complex implementation details** and **showing only the essential features** of an object. It simplifies complex systems by exposing only what is necessary for the user.

### 💡 **Key Points:**

- Helps to manage **complexity** by presenting only **relevant details**.
- Achieves a **clear separation** between **interface** and **implementation**.
- **Encapsulation** is often used to support **abstraction**, but they are **not the same**.

### 🚦 **Real-World Example:**

When you use a **TV remote**, you only interact with **buttons** without needing to know the **internal electronics**.

### 🧠 **In Programming:**

For example, a `List` in **Java** allows you to **add** and **remove items** without showing **how the items are stored internally**.

---

## 🚦 **2. Why Use Abstraction?**

### 🛠️ **Advantages:**

- **Reduces Complexity:** Only the **necessary details** are exposed to the **user**.
- **Enhances Security:** **Internal implementation** is **hidden**.
- **Improves Code Maintenance:** Changes in the **internal code** do **not affect** the **external code**.
- **Increases Flexibility:** **Easier** to **modify and extend** code.

---

## 🌐 **3. Ways to Achieve Abstraction in Java:**

### **Java provides two ways to achieve Abstraction:**

1. **Abstract Classes** (_0% to 100% Abstraction_)
2. **Interfaces** (_100% Abstraction_)

---

# 🌟 **4. Abstract Classes in Java:**

### 🎯 **Definition:**

An **Abstract Class** is a **special class** that:

- Can **contain both abstract methods** (**without implementation**) and **concrete methods** (**with implementation**).
- Cannot be **instantiated** (**no objects** can be **created** directly).
- Provides **partial abstraction**, unlike **interfaces** which provide **full abstraction**.

---

## 💡 **5. Key Features of Abstract Classes:**

|Feature|Description|
|---|---|
|**Abstract Methods**|Methods declared with the `abstract` keyword, **no body**, must be **implemented** in **subclasses**.|
|**Concrete Methods**|Can have **fully implemented methods** that **subclasses can use or override**.|
|**Constructors**|Can **have constructors**, but **cannot instantiate objects** directly.|
|**Fields and Variables**|Can contain **member variables**, **constants**, and **non-static fields**.|
|**Access Modifiers**|Can use all types of **access modifiers** (`public`, `protected`, `private`).|
|**Inheritance**|**Supports inheritance**, allowing **subclasses** to **inherit** or **override methods**.|

---

## 📚 **6. Syntax of Abstract Classes:**

```java
// Abstract class declaration
abstract class Animal {
    
    // Abstract method (without implementation)
    abstract void makeSound();
    
    // Concrete method (with implementation)
    void eat() {
        System.out.println("Eating...");
    }
}

// Subclass inheriting the abstract class
class Dog extends Animal {
    
    // Implementing the abstract method
    @Override
    void makeSound() {
        System.out.println("Bark");
    }
}

// Using the abstract class and subclass
public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.makeSound(); // Output: Bark
        dog.eat(); // Output: Eating...
    }
}
```
---

## 🚦 **7. Rules of Abstract Classes:**

1. **Cannot instantiate** an **abstract class**.
2. **Must implement all abstract methods** in **non-abstract subclasses**.
3. Can **extend only one abstract class** (single inheritance).
4. **Abstract classes** can have **constructors**, but they are **called when subclass objects are created**.
5. Can **have static methods**, **final methods**, and **static fields**.

---

## 📂 **8. Use Cases of Abstract Classes:**

- When you want to **provide a base class** with **default behavior** while allowing **custom behavior** in **subclasses**.
- When you need to **share code** among **several closely related classes**.
- When **interfaces are not sufficient**, i.e., when you need **fields** or **common methods** with **implementation**.

---

# 🌟 **9. Abstract Classes vs. Interfaces:**

|Feature|**Abstract Class**|**Interface**|
|---|---|---|
|**Abstraction Level**|**Partial Abstraction** (0% to 100%)|**Full Abstraction** (100%)|
|**Methods**|Can have **abstract** and **concrete methods**|Before Java 8: Only **abstract methods**  <br>After Java 8: Can have **default** and **static methods**|
|**Variables**|Can have **member variables**|Only **public static final** constants|
|**Constructors**|Can **have constructors**|**Cannot have constructors**|
|**Multiple Inheritance**|**Not supported** directly|**Supported** through **multiple interfaces**|
|**Performance**|**Faster**, as **partially implemented**|**Slower**, as **methods need implementation** in **child classes**|
|**When to Use**|When **classes are related** and **share behavior**|When **unrelated classes** need to **share contracts** (methods)|

---

# 🚦 **10. Best Practices:**

- Use **Abstract Classes** when:
    
    - You want to provide **default behavior** to **subclasses**.
    - **State or fields** need to be **shared**.
    - There is a **clear relationship** between **parent** and **child classes**.
- Use **Interfaces** when:
    
    - You need **multiple inheritance**.
    - You want to **define a contract** for **unrelated classes**.
    - **Behavior** needs to be **shared** without enforcing a **hierarchy**.

---

# 💡 **11. Real-World Example:**

### **Abstract Class Example:**

```java
abstract class Shape {
    abstract void draw(); // Abstract method
	
    void description() { // Concrete method
        System.out.println("This is a shape.");
    }
}

class Circle extends Shape {
    @Override
    void draw() {
        System.out.println("Drawing a circle.");
    }
}

public class TestShape {
    public static void main(String[] args) {
        Shape shape = new Circle();
        shape.draw(); // Output: Drawing a circle.
        shape.description(); // Output: This is a shape.
    }
}
```

---

# 🚀 **12. Key Takeaways:**

- **Abstraction** is about **hiding complexity** and **showing only essential features**.
- **Abstract Classes** provide a **foundation** for other **classes** with **partial implementation**.
- **Abstract Classes** allow you to **reuse code** and **enforce a contract** for **subclasses**.
- Use **Abstract Classes** when **shared behavior** and **state** are needed.
- **Abstract Classes** offer **flexibility** in **designing robust systems** with **maintainable code**.

---


**NEXT ->** [[15 - Abstract classes]]



