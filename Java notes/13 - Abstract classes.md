
---

# 🚦 **1. Introduction to Abstract Classes**

### 🎯 **What is an Abstract Class?**

An **abstract class** in **Java** is a **class** that:

- **Cannot be instantiated** directly.
- **Can contain abstract methods** (**without implementation**) and **concrete methods** (**with implementation**).
- Acts as a **blueprint** for other **subclasses**.
- **Provides partial abstraction**, allowing both **implemented** and **unimplemented methods**.

### 💡 **When to Use Abstract Classes?**

- When **common behavior** should be **shared** among **related classes**.
- When you want to **define a base class** with **default method implementations**.
- When you need **constructors**, **member variables**, or **non-static fields** in the **base class**.

---

# 🌟 **2. Syntax of Abstract Class**

```java
// Declaration of an abstract class
abstract class Animal {
    
    // Abstract method (without implementation)
    abstract void makeSound();
    
    // Concrete method (with implementation)
    void eat() {
        System.out.println("Eating...");
    }
}

// Subclass that extends the abstract class
class Dog extends Animal {
    
    // Implementation of the abstract method
    @Override
    void makeSound() {
        System.out.println("Bark");
    }
}

// Main class to test the implementation
public class TestAbstractClass {
    public static void main(String[] args) {
        Animal dog = new Dog();
        dog.makeSound(); // Output: Bark
        dog.eat(); // Output: Eating...
    }
}
```

---

# 🚦 **3. Key Features of Abstract Classes**

|Feature|Description|
|---|---|
|**Abstract Methods**|Methods declared with the `abstract` keyword, **no body**, must be **implemented in subclasses**.|
|**Concrete Methods**|Can **contain fully implemented methods** that **subclasses can use or override**.|
|**Constructors**|Can **have constructors**, but they are **called when subclass objects are created**.|
|**Member Variables**|Can contain **fields**, **constants**, and **non-static fields**.|
|**Access Modifiers**|Can use all **access modifiers** (`public`, `protected`, `private`).|
|**Inheritance**|**Supports single inheritance**, allowing **subclasses** to **inherit or override methods**.|
|**Static Methods**|Can **declare static methods**, but **cannot be overridden**, only **hidden** by **subclasses**.|
|**Final Methods**|Can have **final methods**, preventing **subclasses** from **overriding them**.|

---

# 🌟 **4. Abstract Methods**

### 🎯 **What is an Abstract Method?**

- An **abstract method** is a **method declaration without a body**.
- **Must be declared inside an abstract class**.
- **Forces subclasses** to **provide an implementation** of the method.

### 🧠 **Syntax:**

```java
abstract void methodName(); // Abstract method with no body
```

### 💡 **Example:**

```java
abstract class Vehicle {
    abstract void start(); // Abstract method
}

class Car extends Vehicle {
    @Override
    void start() {
        System.out.println("Car started!");
    }
}
```

---

# 🚦 **5. Concrete Methods in Abstract Classes**

### 🎯 **What is a Concrete Method?**

- A **concrete method** in an **abstract class** is a **method with an implementation**.
- **Optional** for **subclasses** to **override**.
- Provides **default behavior** that **subclasses** can **use as is** or **modify as needed**.

### 💡 **Example:**

```java
abstract class Animal {
    void sleep() {
        System.out.println("Sleeping...");
    }
}
```

---

# 🌟 **6. Constructors in Abstract Classes**

### 🎯 **Can Abstract Classes Have Constructors?**

- **Yes**, **abstract classes** can have **constructors**.
- These **constructors are called** when an **object of a subclass** is **created**.
- Useful for **initializing common properties** of **subclasses**.

### 💡 **Example:**

```java
abstract class Animal {
    String name;
    
    // Constructor of the abstract class
    Animal(String name) {
        this.name = name;
        System.out.println(name + " is created.");
    }
}

class Dog extends Animal {
    Dog(String name) {
        super(name); // Calling the constructor of the abstract class
    }
}

public class TestConstructor {
    public static void main(String[] args) {
        Dog dog = new Dog("Buddy");
    }
}
```
---

# 🚦 **7. Rules for Abstract Classes**

1. **Cannot Instantiate:**

```java
Animal animal = new Animal(); // Error: Animal is abstract; cannot be instantiated
```

2. **Abstract Method Implementation:**

- **Subclasses must implement** all **abstract methods** of the **abstract class**.

3. **Single Inheritance Only:**

- **Java** does **not support multiple inheritance** with **classes**, including **abstract classes**.

4. **Static and Final Methods:**

- **Static methods** are **allowed**, but they **cannot be abstract**.
- **Final methods** are **allowed**, but they **cannot be overridden**.

---

# 🌟 **8. Real-World Example of Abstract Class**

```java
abstract class Shape {
    abstract double area(); // Abstract method
	
    void display() {
        System.out.println("This is a shape.");
    }
}

class Circle extends Shape {
    double radius;
	
    Circle(double radius) {
        this.radius = radius;
    }
	
    @Override
    double area() {
        return Math.PI * radius * radius;
    }
}

public class TestShape {
    public static void main(String[] args) {
        Circle circle = new Circle(5.0);
        circle.display(); // Output: This is a shape.
        System.out.println("Area: " + circle.area()); // Output: Area: 78.54
    }
}
```

---

# 🚦 **9. Advanced Topics of Abstract Classes**

### 🧠 **1. Abstract Class with Non-Abstract Subclass**

```java
abstract class Animal {
    void sound() {
        System.out.println("Some sound");
    }
}

class Cat extends Animal {
    // Inherits the concrete method without changes
}
```

### 🧠 **2. Abstract Class with Static Methods**

```java
abstract class Utility {
    static void printHello() {
        System.out.println("Hello from abstract class!");
    }
}
```

### 🧠 **3. Abstract Class with Final Methods**

```java
abstract class Device {
    final void powerOn() {
        System.out.println("Device is powered on.");
    }
}
```

### 🧠 **4. Abstract Class and Interfaces Together**

```java
interface Movable {
    void move();
}

abstract class Vehicle implements Movable {
    abstract void start();
}
```

---

# 🌟 **10. Best Practices for Abstract Classes**

- **Use abstract classes** when you need to **share code** among **closely related classes**.
- **Prefer interfaces** when **multiple inheritance** is **required**.
- **Use abstract methods** to **enforce subclasses** to **implement specific behavior**.
- Avoid making an **abstract class final**, as it **prevents inheritance**.

---

# 🚀 **11. Summary**

- **Abstract classes** provide a **template** for **other classes**.
- They **cannot be instantiated directly**.
- **Abstract methods** **must be implemented** by **subclasses**.
- **Constructors** can be **used** to **initialize state**.
- **Concrete methods** provide **default behavior**.
- Allow **single inheritance** only.

---

