
# 📝 **Inheritance in Java: A Complete Guide**

---

## 🚦 **1. What is Inheritance?**

**Inheritance** is an **Object-Oriented Programming (OOP)** concept where a **child class** (**subclass**) **inherits** the **properties** and **methods** of a **parent class** (**superclass**).

- Enables **code reusability** and **method overriding**.
- Establishes a **"is-a" relationship**, e.g., `Dog is a Pet`.

---

## 🎯 **2. Why Use Inheritance?**

- **Code Reusability:** Avoids code duplication.
- **Method Overriding:** Enables runtime polymorphism.
- **Extensibility:** Easy to extend the functionality of existing code.
- **Maintainability:** Centralized code management.

---

## 🧱 **3. Syntax:**

```java
class Parent {
    // Parent class properties and methods
}

class Child extends Parent {
    // Child class properties and methods
}
```

---

## 🌐 **4. Types of Inheritance in Java:**

|Type|Description|Example|
|---|---|---|
|**Single**|One subclass inherits one superclass|`Dog extends Animal`|
|**Multilevel**|One subclass inherits from a derived class|`Puppy extends Dog`|
|**Hierarchical**|Multiple subclasses inherit one superclass|`Dog extends Animal`, `Cat extends Animal`|
|**Multiple**|**Not Supported** directly in Java using classes, but possible with **interfaces**.|-|
|**Hybrid**|Combination of **multiple** and **multilevel**, achieved using **interfaces**.|-|

---

## 📂 **5. Examples of Each Type:**

### 🔹 **Single Inheritance:**

```java
class Animal {
    void eat() {
        System.out.println("Eating...");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Barking...");
    }
}

public class SingleInheritance {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.eat(); // From Parent Class
        d.bark(); // From Child Class
    }
}
```

---

### 🔹 **Multilevel Inheritance:**

```java
class Animal {
    void eat() {
        System.out.println("Eating...");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Barking...");
    }
}

class Puppy extends Dog {
    void weep() {
        System.out.println("Weeping...");
    }
}

public class MultilevelInheritance {
    public static void main(String[] args) {
        Puppy p = new Puppy();
        p.eat();
        p.bark();
        p.weep();
    }
}
```

---

### 🔹 **Hierarchical Inheritance:**

```java
class Animal {
    void eat() {
        System.out.println("Eating...");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Barking...");
    }
}

class Cat extends Animal {
    void meow() {
        System.out.println("Meowing...");
    }
}

public class HierarchicalInheritance {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.eat();
        d.bark();
		
        Cat c = new Cat();
        c.eat();
        c.meow();
    }
}
```

---

## 🚫 **6. Why Multiple Inheritance is Not Supported in Java?**

- **Ambiguity Problem:** Known as the **Diamond Problem**.
- **Example:** If both parent classes have a method with the same name, the compiler gets confused about which method to inherit.

---

## 🔍 **7. How to Achieve Multiple Inheritance in Java?**

### ✅ **Using Interfaces:**

```java
interface Printable {
    void print();
}

interface Showable {
    void show();
}

class Demo implements Printable, Showable {
    public void print() {
        System.out.println("Printing...");
    }
    public void show() {
        System.out.println("Showing...");
    }
}

public class MultipleInheritanceWithInterfaces {
    public static void main(String[] args) {
        Demo d = new Demo();
        d.print();
        d.show();
    }
}
```

---

## 🎯 **8. Key Concepts in Inheritance:**

### 🔹 **Method Overriding:**

- **Purpose:** Provide a specific implementation of a method in the **child class**.
- **Rules:**
    - Method name, return type, and parameters must be the same.
    - Cannot override **static**, **final**, or **private** methods.

```java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}
```

---

### 🔹 **`super` Keyword:**

- **Access Parent Class Members:**
- **Method Invocation:** Call the **parent class method** in the **child class**.
- **Constructor Chaining:** Call the **parent class constructor**.

```java
class Animal {
    String color = "white";
	
    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    String color = "black";
	
    void displayColor() {
        System.out.println(color); // Output: black
        System.out.println(super.color); // Output: white
    }
	
    @Override
    void sound() {
        super.sound(); // Calls Animal's sound()
        System.out.println("Dog barks");
    }
}
```

---

### 🔹 **`final` Keyword:**

- **`final class`:** Cannot be **inherited**.
- **`final method`:** Cannot be **overridden**.
- **`final variable`:** Cannot change its **value**.

---

### 🔹 **Constructors in Inheritance:**

- When a **child class object** is created, the **parent class constructor** is called **first**.
- Use **`super()`** to invoke a **specific parent constructor**.

```java
class Animal {
    Animal() {
        System.out.println("Animal constructor");
    }
}

class Dog extends Animal {
    Dog() {
        super(); // Calls the parent class constructor
        System.out.println("Dog constructor");
    }
}
```

---

## 🛠️ **9. Practical Use Cases of Inheritance:**

- **Frameworks:** Spring, Hibernate use inheritance heavily.
- **UI Components:** Swing and JavaFX components inherit common behavior.
- **Design Patterns:** Many patterns (e.g., **Decorator**, **Template Method**) use inheritance.

---

## 🚦 **10. Best Practices:**

- Use inheritance only if there is a **genuine "is-a" relationship**.
- Prefer **composition over inheritance** if it makes more sense.
- Avoid **deep inheritance hierarchies** for better maintainability.

---

## 🎯 **Conclusion:**

- **Inheritance** promotes **code reuse** and **polymorphism**, but should be used **wisely**.
- Mastering **inheritance** involves understanding **method overriding**, the **`super` keyword**, and **constructors**.


---



**NEXT ->** [[11 - Polymorphism]]

## 📚 **Next Steps:**

Would you like to:

- **See more code examples** on specific scenarios?
- **Understand how inheritance interacts** with **interfaces** and **abstract classes**?
- **Explore advanced concepts** like **dynamic method dispatch**?