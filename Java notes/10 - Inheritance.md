
# 📝 **Inheritance in Java: A Complete Guide**

---

## 🚦 **1. What is Inheritance?**

**Inheritance** is an **Object-Oriented Programming (OOP)** concept where a **child class** (**subclass**) **inherits** the **properties** and **methods** of a **parent class** (**superclass**).


### 💡 **Key Idea:**

- Promotes **code reusability** and **modular design**.
- Enables **code reusability** and **method overriding**.
- Establishes a **parent-child relationship** between **classes**.
- Establishes a **"is-a" relationship**, e.g., `Dog is a Pet`.
- Helps in **implementing polymorphism** and **method overriding**.

---

## 🎯 **2. Why Use Inheritance?**

- **Code Reusability:** Avoids code duplication.
- **Method Overriding:** Enables runtime polymorphism.
- **Extensibility:** Easy to extend the functionality of existing code.
- **Maintainability:** Centralized code management.
-  **Readability & Maintainability:** Easier to **manage** and **extend**.
- **Polymorphism Support:** Enables **dynamic method dispatch**.
- **Extensibility:** Allows **modification** and **enhancement** of **existing code**

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

### 🔑 **Important Keywords:**

- **extends:** Used to **establish inheritance** between **classes**.
- **super:** Refers to the **parent class** and is used to **access parent class methods** and **constructors**.
---

## 🌐 **4. Types of Inheritance in Java:**

|Type|Description|Example|
|---|---|---|
|**Single**|One subclass inherits one superclass|`Dog extends Animal`|
|**Multilevel**|One subclass inherits from a derived class|`Puppy extends Dog`|
|**Hierarchical**|Multiple subclasses inherit one superclass|`Dog extends Animal`, `Cat extends Animal`|
|**Multiple**|**Not Supported** directly in Java using classes, but possible with **interfaces**.|-|
|**Hybrid**|Combination of **multiple** and **multilevel**, achieved using **interfaces**.|-|


| **Inheritance Type**         | **Description**                                                   | **Support in Java**                                                   |
| ---------------------------- | ----------------------------------------------------------------- | --------------------------------------------------------------------- |
| **Single Inheritance**       | One **child class** inherits from **one parent class**            | ✅ **Supported**                                                       |
| **Multilevel Inheritance**   | A **child class** is derived from another **child class**         | ✅ **Supported**                                                       |
| **Hierarchical Inheritance** | **Multiple child classes** inherit from the **same parent class** | ✅ **Supported**                                                       |
| **Multiple Inheritance**     | One **child class** inherits from **multiple parent classes**     | ❌ **Not supported** with **classes** (can use **interfaces** instead) |
| **Hybrid Inheritance**       | A **combination** of **two or more types** of **inheritance**     | ❌ **Not directly supported** (use **interfaces**)                     |

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

**❌Using final Keyword:**

```java
final class Parent {
    // Cannot be extended
}

class Child extends Parent { // ❌ Compile-time error
}
```

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


## 🔍 **11. Common Mistakes in Inheritance:**

1. **Accidental Overriding:** Method **signature mismatch**.
2. **Circular Inheritance:** Java **does not support**.
3. **Not Using super() Properly:** To **call parent constructor**.
4. **Shadowing Variables:** Avoid **name conflicts**.

---

## 🎯 **Conclusion:**

- **Inheritance** promotes **code reuse** and **polymorphism**, but should be used **wisely**.
- Mastering **inheritance** involves understanding **method overriding**, the **`super` keyword**, and **constructors**.


---


**NEXT ->** [[11 - static Keyword]]  or/and [[13 - Polymorphism]]


---


## 📚 **Next Steps:**

Would you like to:

- **See more code examples** on specific scenarios?
- **Understand how inheritance interacts** with **interfaces** and **abstract classes**?
- **Explore advanced concepts** like **dynamic method dispatch**?


---


# 🚀 **Advanced Concepts of Inheritance in Java:**

---

### 1. **Multiple Inheritance (Through Interfaces):**

- **Java** does not support **multiple inheritance** with **classes** to avoid the **diamond problem**.
- However, **multiple inheritance** can be achieved using **interfaces**.

```java
interface A {
    void methodA();
}

interface B {
    void methodB();
}

class C implements A, B {
    public void methodA() {
        System.out.println("Method A from Interface A");
    }
    
    public void methodB() {
        System.out.println("Method B from Interface B");
    }
}

public class Main {
    public static void main(String[] args) {
        C obj = new C();
        obj.methodA();
        obj.methodB();
    }
}
```

---

### 2. **Diamond Problem:**

- Occurs when a **class inherits** from **two classes** that have a **common base class**.
- **Java** prevents this issue by **not allowing multiple inheritance with classes**.
- It can be handled with **interfaces** using **default methods**.

```java
interface A {
    default void show() {
        System.out.println("Show from Interface A");
    }
}

interface B {
    default void show() {
        System.out.println("Show from Interface B");
    }
}

class C implements A, B {
    public void show() {
        A.super.show(); // Resolving ambiguity
    }
}

public class Main {
    public static void main(String[] args) {
        C obj = new C();
        obj.show();
    }
}
```

---

### 3. **Constructor Chaining:**

- When a **subclass constructor** calls the **parent class constructor** using **super()**.
- Helps in **initializing objects** in a **hierarchy**.

```java
class Parent {
    Parent() {
        System.out.println("Parent Constructor");
    }
}

class Child extends Parent {
    Child() {
        super(); // Calls Parent constructor
        System.out.println("Child Constructor");
    }
}

public class Main {
    public static void main(String[] args) {
        Child obj = new Child();
    }
}

```

**Output:**

```shell
Parent Constructor
Child Constructor
```

---

### 4. **Covariant Return Type:**

- Allows **overridden methods** to **return a subtype** of the **parent method's return type**.

```java
class Parent {
    Parent get() {
        System.out.println("Parent Class");
        return this;
    }
}

class Child extends Parent {
    @Override
    Child get() {
        System.out.println("Child Class");
        return this;
    }
}

public class Main {
    public static void main(String[] args) {
        Parent obj = new Child();
        obj.get(); // Calls Child's method
    }
}
```

---

### 5. **Upcasting and Downcasting:**

#### 🆙 **Upcasting:**

- **Child class** reference is **assigned** to a **parent class reference**.
- **Implicit casting**, no need for **explicit type casting**.

```java
Parent obj = new Child(); // Upcasting
obj.parentMethod();
```

---

#### ⬇️ **Downcasting:**

- **Parent class reference** is **assigned** to a **child class reference**.
- **Explicit type casting** is required.

```java
Parent obj = new Child(); // Upcasting
Child childObj = (Child) obj; // Downcasting
childObj.childMethod();
```
---

### 6. **Method Hiding:**

- When a **static method** in a **child class** has the **same signature** as a **static method** in the **parent class**.
- The **parent class method** is **hidden**, not **overridden**.

```java
class Parent {
    static void show() {
        System.out.println("Parent's Static Method");
    }
}

class Child extends Parent {
    static void show() {
        System.out.println("Child's Static Method");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent obj = new Child();
        obj.show(); // Calls Parent's static method
    }
}
```

---

### 7. **Final Keyword in Inheritance:**

#### 🚫 **Preventing Class Inheritance:**

```java
final class Parent {
    // This class cannot be inherited
}

class Child extends Parent { // ❌ Error: Cannot inherit from final class
}
```

---

#### 🚫 **Preventing Method Overriding:**

```java
class Parent {
    final void show() {
        System.out.println("This method cannot be overridden");
    }
}

class Child extends Parent {
    @Override
    void show() { // ❌ Error: Cannot override final method
    }
}
```

---

### 8. **Inheritance with Abstract Classes:**

```java
abstract class Animal {
    abstract void sound();
    
    void eat() {
        System.out.println("Eating...");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Barking...");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.sound();
        dog.eat();
    }
}
```

---

### 9. **Protected Access Modifier:**

- **Protected members** can be **accessed** within the **same package** and by **subclasses** in **different packages**.

```java
package parent;

public class Parent {
    protected void show() {
        System.out.println("Protected Method");
    }
}

package child;
import parent.Parent;

public class Child extends Parent {
    public static void main(String[] args) {
        Child obj = new Child();
        obj.show(); // Accessible through inheritance
    }
}
```

---

### 10. **Super vs This Keyword:**

|**Keyword**|**Purpose**|
|---|---|
|**this**|Refers to the **current class instance**.|
|**super**|Refers to the **parent class instance**.|

```java
class Parent {
    int a = 10;
}

class Child extends Parent {
    int a = 20;
	
    void display() {
        System.out.println("Child a: " + this.a); // 20
        System.out.println("Parent a: " + super.a); // 10
    }
}

public class Main {
    public static void main(String[] args) {
        Child obj = new Child();
        obj.display();
    }
}
```

---

# 📌 **Conclusion:**

These **advanced concepts** in **inheritance** enhance **flexibility**, **polymorphism**, and **code reusability**. Mastering these **concepts** is **crucial** for **advanced Java development** and **designing complex systems**.

If you want to **dive deeper** into any **specific topic**, just let me know! 😊