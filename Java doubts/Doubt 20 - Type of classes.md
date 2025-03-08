

---


## **1. Concrete Class**

A **concrete class** is a normal Java class that contains both **fields (variables)** and **methods (with implementations)**. It **can be instantiated** (objects can be created from it).

### **Characteristics:**

- Can contain **fully implemented methods**.
- Can have constructors, variables, and methods.
- Can be **inherited** by other classes.
- Used when you need a fully functional class.

### **Example:**

```java
class Car {
    String brand;
    
    Car(String brand) {
        this.brand = brand;
    }
	
    void drive() {
        System.out.println(brand + " is driving");
    }
}

public class Main {
    public static void main(String[] args) {
        Car myCar = new Car("Toyota");
        myCar.drive(); // Output: Toyota is driving
    }
}
```

Here, `Car` is a **concrete class** since it is fully implemented and can be instantiated.

---

## **2. Abstract Class**

An **abstract class** is a class that **cannot be instantiated**. It **must be inherited** by a subclass. It **may or may not contain abstract methods** (methods without a body).

### **Characteristics:**

- Declared using the `abstract` keyword.
- **Cannot be instantiated directly**.
- Can have **abstract methods (without a body)**.
- Can also have **concrete methods** (with a body).
- Used when you need **a base class** for other classes.

### **Example:**

```java
abstract class Animal {
    abstract void makeSound(); // Abstract method (must be implemented by subclasses)

    void sleep() { // Concrete method
        System.out.println("Sleeping...");
    }
}

class Dog extends Animal {
    void makeSound() {
        System.out.println("Barking...");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.makeSound(); // Output: Barking...
        d.sleep();     // Output: Sleeping...
    }
}
```

Here:

- `Animal` is an **abstract class**.
- `makeSound()` is an **abstract method** (must be implemented by subclasses).
- `sleep()` is a **concrete method** (already implemented in `Animal`).

---

## **3. Final Class**

A **final class** is a class that **cannot be extended (inherited)**. It is used when you **want to prevent modifications** to a class.

### **Characteristics:**

- Declared using the `final` keyword.
- **Cannot be subclassed (inherited)**.
- Often used for **security** and **utility classes**.

### **Example:**

```java
final class Calculator {
    int add(int a, int b) {
        return a + b;
    }
}

// class AdvancedCalculator extends Calculator {} // ❌ Error! Cannot extend a final class.

public class Main {
    public static void main(String[] args) {
        Calculator c = new Calculator();
        System.out.println(c.add(5, 3)); // Output: 8
    }
}
```

Here:

- `Calculator` is **final**, so no other class can extend it.

---

## **4. Static Nested Class**

A **static nested class** is a class defined inside another class and **declared as static**. It can be **accessed without an instance of the outer class**.

### **Characteristics:**

- Declared using the `static` keyword inside another class.
- Can access only **static members** of the outer class.
- Does **not require an instance** of the outer class to be created.

### **Example:**

```java
class Outer {
    static class StaticNested {
        void show() {
            System.out.println("Inside Static Nested Class");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Outer.StaticNested obj = new Outer.StaticNested();
        obj.show(); // Output: Inside Static Nested Class
    }
}
```

Here:

- `StaticNested` is a **static nested class**, so it can be used without creating an `Outer` instance.

---

## **5. Inner (Non-Static Nested) Class**

An **inner class** is a class **inside another class** but **without the static keyword**. It requires an instance of the outer class to be created.

### **Characteristics:**

- **Cannot be accessed without an instance** of the outer class.
- Can access **both static and non-static members** of the outer class.

### **Example:**

java

CopyEdit

`class Outer {     class Inner {         void display() {             System.out.println("Inside Inner Class");         }     } }  public class Main {     public static void main(String[] args) {         Outer outer = new Outer();         Outer.Inner inner = outer.new Inner();         inner.display(); // Output: Inside Inner Class     } }`

Here:

- `Inner` class requires an instance of `Outer` to be created.

---

## **6. Anonymous Class**

An **anonymous class** is a **class without a name**, usually created **on the fly** for short-term use.

### **Characteristics:**

- Used for **quick implementations** of interfaces or abstract classes.
- **Cannot be reused**.
- Typically used for **event handling** and **small custom behaviors**.

### **Example:**

java

CopyEdit

`abstract class Person {     abstract void speak(); }  public class Main {     public static void main(String[] args) {         Person p = new Person() {             void speak() {                 System.out.println("Hello, I am an anonymous class!");             }         };         p.speak(); // Output: Hello, I am an anonymous class!     } }`

Here:

- An **anonymous class** extends `Person` **without naming it**.

---

## **7. Interface (Technically Not a Class, But Similar)**

An **interface** is a blueprint for a class, containing only **abstract methods** (until Java 8). A class **must implement all methods** of an interface.

### **Characteristics:**

- Declared using the `interface` keyword.
- Can contain **abstract methods, default methods, and static methods**.
- **Cannot have constructors**.
- **Used for multiple inheritance**.

### **Example:**

java

CopyEdit

`interface Animal {     void makeSound(); }  class Cat implements Animal {     public void makeSound() {         System.out.println("Meowing...");     } }  public class Main {     public static void main(String[] args) {         Cat c = new Cat();         c.makeSound(); // Output: Meowing...     } }`

Here:

- `Animal` is an **interface**, and `Cat` **implements** it.

---

## **8. Record Class (Introduced in Java 14)**

A **record class** is a compact way to create **immutable data objects** with **less boilerplate code**.

### **Characteristics:**

- Declared using the `record` keyword.
- Automatically generates **constructors, getters, equals(), hashCode(), and toString()**.

### **Example:**

java

CopyEdit

`record Employee(String name, int id) {}  public class Main {     public static void main(String[] args) {         Employee e = new Employee("Alice", 101);         System.out.println(e.name()); // Output: Alice         System.out.println(e);        // Output: Employee[name=Alice, id=101]     } }`

Here:

- `Employee` is a **record**, reducing the need for boilerplate code.

---

## **Conclusion**

Each type of class serves a specific purpose:

- **Concrete Class** → Standard class with full implementation.
- **Abstract Class** → Serves as a base class, cannot be instantiated.
- **Final Class** → Cannot be extended.
- **Static Nested Class** → Nested but independent of outer class instance.
- **Inner Class** → Nested and dependent on outer class instance.
- **Anonymous Class** → Used for short-term implementations.
- **Interface** → Defines a contract for implementing classes.
- **Record Class** → Compact, immutable data storage.