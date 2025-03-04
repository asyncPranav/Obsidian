
---
Interfaces are a fundamental part of Java that enable abstraction and multiple inheritance. This guide covers everything about **interfaces**, from basic concepts to advanced use cases.

---

# **1️⃣ What is an Interface?**

- An **interface** in Java is a blueprint of a class that contains only abstract methods and constants.
- An **interface** in Java is a blueprint for a class that contains **abstract methods** and **constants** (variables that are `public static final` by default).
- It is used for achieving **abstraction** and **multiple inheritance** in Java.
- Declared using the `interface` keyword.
- It allows **multiple inheritance** and **defines a contract** that implementing classes must follow.

### **🔹 Key Characteristics of an Interface**

✔ Contains **abstract methods** (before Java 8).  
✔ Contains **static and final variables** (constants).  
✔ Methods are **public and abstract** by default.  
✔ Used for **abstraction** and **multiple inheritance**.  
✔ Implemented by a class using the **`implements`** keyword.

---

# **2️⃣ Syntax & Declaration of an Interface**

An interface is declared using the `interface` keyword.

```java
interface Animal {
    int age = 10; // Automatically public, static, and final
	
    void makeSound(); // Abstract method (public and abstract by default)
}
```

- The method `makeSound()` is **implicitly public and abstract**.
- The variable `age` is **public, static, and final** by default.

---

# **3️⃣ Implementing an Interface**

A class must **implement** all methods of an interface.

```java
interface Animal {
    void makeSound();
}

class Dog implements Animal {
    public void makeSound() { // Must be public
        System.out.println("Bark");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.makeSound();  // Output: Bark
    }
}
```

✔ A class uses the `implements` keyword.  
✔ All interface methods **must** be implemented.  
✔ The implemented method **must be public** (because interface methods are implicitly public).

---

# **4️⃣ Multiple Interface Implementation**

A class can implement multiple interfaces.

```java
interface A {
    void methodA();
}

interface B {
    void methodB();
}

class C implements A, B {
    public void methodA() {
        System.out.println("Method A");
    }
    
    public void methodB() {
        System.out.println("Method B");
    }
}

public class Main {
    public static void main(String[] args) {
        C obj = new C();
        obj.methodA();  // Output: Method A
        obj.methodB();  // Output: Method B
    }
}
```

✔ **Java supports multiple inheritance** via interfaces.  
✔ A class can implement multiple interfaces, but must provide **implementations for all methods**.

---

# **5️⃣ Interface Variables (Constants)**

All variables in an interface are: ✔ `public` (accessible everywhere).  
✔ `static` (belongs to the interface itself, not instances).  
✔ `final` (cannot be changed).

```java
interface Constants {
    int VALUE = 100; // public static final by default
}

public class Test {
    public static void main(String[] args) {
        System.out.println(Constants.VALUE); // Output: 100
    }
}
```

✔ **Cannot modify** `VALUE` since it's `final`.  
✔ Accessed using `InterfaceName.VARIABLE_NAME`.

---

# **6️⃣ Interface in Java 8**

Before Java 8, interfaces could only have **abstract methods**.  
**Java 8 introduced:**

1. **Default Methods** (Methods with body)
2. **Static Methods** (Methods inside interfaces)

### **🔹 Default Method (Method with Implementation)**

```java
interface Printer {
    void print();
    
    default void show() { // Default method
        System.out.println("Default method in interface");
    }
}

class TextPrinter implements Printer {
    public void print() {
        System.out.println("Printing text...");
    }
}

public class Main {
    public static void main(String[] args) {
        TextPrinter obj = new TextPrinter();
        obj.print();  // Output: Printing text...
        obj.show();   // Output: Default method in interface
    }
}
```

✔ **Default methods** prevent breaking existing code when adding new methods.  
✔ Implementing classes **can override** default methods.

### **🔹 Static Method in Interface**

```java
interface MathOperations {
    static void info() {
        System.out.println("This is a static method in an interface");
    }
}

public class Main {
    public static void main(String[] args) {
        MathOperations.info(); // Output: This is a static method in an interface
    }
}
```

✔ **Static methods** belong to the interface itself and **cannot be overridden**.

---

# **7️⃣ Interface in Java 9+ (Private Methods)**

Java 9 introduced **private methods** in interfaces to **avoid code duplication** inside default and static methods.

```java
interface Helper {
    default void show() {
        commonMethod();
        System.out.println("Default method");
    }
	
    static void display() {
        commonMethod();
        System.out.println("Static method");
    }
	
    private static void commonMethod() { // Private method
        System.out.println("Common logic inside interface");
    }
}

public class Main {
    public static void main(String[] args) {
        Helper.display(); // Output: Common logic inside interface
    }
}
```

✔ **Private methods** improve code reusability inside interfaces.

---

# **8️⃣ Marker Interface**

A **marker interface** is an interface **without any methods or fields**.  
✔ Used to **tag** classes for special behavior.  
✔ Examples:

- `Serializable`
- `Cloneable`
- `Remote`

```java
import java.io.Serializable;

class Employee implements Serializable {
    int id;
    String name;
}
```

✔ The **presence** of the marker interface itself enables certain functionalities (e.g., serialization).

---

# **9️⃣ Functional Interface (Java 8)**

A **functional interface** has **exactly one abstract method** and can be used with **lambda expressions**.

```java
@FunctionalInterface
interface MyFunctionalInterface {
    void display();
}

public class Main {
    public static void main(String[] args) {
        MyFunctionalInterface obj = () -> System.out.println("Lambda Expression");
        obj.display(); // Output: Lambda Expression
    }
}
```

✔ The `@FunctionalInterface` annotation ensures the interface has **only one method**.  
✔ Functional interfaces **enable functional programming** in Java.

---

# **🔟 Interface vs Abstract Class**

|Feature|Interface|Abstract Class|
|---|---|---|
|Methods|Only abstract (until Java 8)|Can have abstract & concrete methods|
|Variables|`public static final`|Can have instance variables|
|Constructor|Not allowed|Allowed|
|Multiple Inheritance|Yes (Multiple interfaces)|No (Only one abstract class)|
|Default Methods|Allowed (Java 8+)|Not needed|

---

# **1️⃣1️⃣ Multiple Inheritance Conflicts**

If multiple interfaces have the **same method signature**, Java requires explicit resolution.

```java
interface A {
    default void show() {
        System.out.println("Interface A");
    }
}

interface B {
    default void show() {
        System.out.println("Interface B");
    }
}

class C implements A, B {
    public void show() { // Must resolve conflict
        A.super.show(); // Calling A's show()
    }
}

public class Main {
    public static void main(String[] args) {
        C obj = new C();
        obj.show(); // Output: Interface A
    }
}
```

---

# **1️⃣2️⃣ Summary**

✔ Interfaces provide **abstraction** and **multiple inheritance**.  
✔ Java 8+ introduced **default and static methods**.  
✔ Java 9+ introduced **private methods** in interfaces.  
✔ Functional Interfaces **support lambda expressions**.  
✔ **Marker interfaces** are empty interfaces used for tagging.

🚀 **This is a complete and detailed guide to Java Interfaces from scratch to advanced!**

