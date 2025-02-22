### **1. Introduction to Object-Oriented Programming (OOP)**

- **What is OOP?**
    
    - OOP is a programming paradigm based on the concept of "objects", which can contain both data (attributes) and methods (behaviors).
    - It focuses on modularity, reusability, and scalability of code.
- **Basic Principles of OOP**:
    
    - **Encapsulation**: Bundling data and methods into a single unit (class) and restricting access to some components using access modifiers.
    - **Inheritance**: Mechanism to derive new classes from existing ones, enabling code reuse.
    - **Polymorphism**: The ability of different classes to provide different implementations of the same method.
    - **Abstraction**: Hiding implementation details and only exposing the necessary parts of the code.

---

### **2. Classes and Objects**

- **Class**: A blueprint for creating objects. It defines data and methods that operate on that data.
- **Object**: An instance of a class.
- **Example**:
    
```java
class Car {
    String color;
    String model;
	
    void startEngine() {
        System.out.println("Engine started");
    }
}

// Creating an object
Car car1 = new Car();
car1.color = "Red";
car1.model = "Sedan";
car1.startEngine();

```
    

---

### **3. Encapsulation**

- **Definition**: Encapsulation is the process of hiding the internal details of an object and only exposing what is necessary. It helps achieve data security.
- **Access Modifiers**:
    - `private`: Accessible only within the same class.
    - `protected`: Accessible within the same package and subclasses.
    - `public`: Accessible from anywhere.
    - Default: Accessible only within the same package.
- **Example**:
    
```java
class Student {
    private String name;

    // Getter method
    public String getName() {
        return name;
    }

    // Setter method
    public void setName(String name) {
        this.name = name;
    }
}

```

---

### **4. Constructors**

- **Definition**: Constructors are special methods used to initialize objects.
- **Types**:
    - **Default Constructor**: A constructor with no parameters.
    - **Parameterized Constructor**: A constructor that takes arguments.
- **Example**:
    
```java
class Book {
    String title;
    String author;

    // Parameterized constructor
    Book(String title, String author) {
        this.title = title;
        this.author = author;
    }

    // Default constructor
    Book() {
        this.title = "Unknown";
        this.author = "Unknown";
    }
}

```
    

---

### **5. Inheritance**

- **Definition**: Inheritance is a mechanism where a new class inherits the properties and behaviors of an existing class.
- **Syntax**: `class Subclass extends Superclass`
- **Example**:
    
```java
class Animal {
    void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog is barking");
    }
}

Dog dog = new Dog();
dog.eat();  // Inherited method
dog.bark(); // Method of Dog

```
    

---

### **6. Method Overloading**

- **Definition**: Method overloading allows a class to have more than one method with the same name but different parameters.
- **Example**:
    
```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }
}

```
    

---

### **7. Method Overriding**

- **Definition**: Method overriding allows a subclass to provide a specific implementation of a method that is already defined in the superclass.
- **Syntax**: `@Override` annotation
- **Example**:
    
```java
class Animal {
    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}

```
    

---

### **8. Polymorphism**

- **Definition**: Polymorphism allows objects of different classes to be treated as objects of a common superclass.
- **Types**:
    - **Compile-time (Method Overloading)**: Resolving method calls at compile time.
    - **Runtime (Method Overriding)**: Resolving method calls at runtime based on the actual object type.
- **Example**:
    
```java
class Animal {
    void makeSound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Bark");
    }
}

class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("Meow");
    }
}

Animal animal1 = new Dog();
Animal animal2 = new Cat();

animal1.makeSound(); // Bark
animal2.makeSound(); // Meow

```
    

---

### **9. Abstraction**

- **Definition**: Abstraction is the concept of hiding the complex implementation details and showing only the necessary features.
- **Abstract Class**: A class that cannot be instantiated and may contain abstract methods (without implementation).
- **Interface**: A contract that defines methods without providing implementation.
- **Example**:
    
```java
abstract class Animal {
    abstract void sound();
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}

interface Vehicle {
    void start();
}

class Car implements Vehicle {
    @Override
    public void start() {
        System.out.println("Car starting");
    }
}

```
    

---

### **10. Interfaces**

- **Definition**: An interface is a reference type that defines a contract of methods but does not implement them.
- **Syntax**: `interface InterfaceName`
- **Example**:
    
```java
interface Drawable {
    void draw();
}

class Circle implements Drawable {
    @Override
    public void draw() {
        System.out.println("Drawing a circle");
    }
}

class Rectangle implements Drawable {
    @Override
    public void draw() {
        System.out.println("Drawing a rectangle");
    }
}

```

---

### **11. Static Keyword**

- **Definition**: The `static` keyword indicates that a member belongs to the class rather than instances of the class.
- **Static Variables**: Variables shared among all instances of the class.
- **Static Methods**: Methods that can be called on the class itself, rather than on instances.
- **Example**:
    
```java
class Counter {
    static int count = 0;

    static void increment() {
        count++;
    }
}

Counter.increment();
System.out.println(Counter.count); // Output: 1

```
    

---

### **12. Final Keyword**

- **Definition**: The `final` keyword is used to define constants, prevent method overriding, and prevent class inheritance.
- **Example**:
    
```java
final int MAX_VALUE = 100;  // Constant

final class Animal {
    // Cannot be subclassed
}

class Dog extends Animal {  // Error: Cannot inherit from final Animal
}

```
    

---

### **13. Nested Classes**

- **Definition**: A class defined inside another class. There are two types: Static Nested Classes and Inner Classes.
- **Example**:
    
```java
class Outer {
    private int x = 10;

    // Inner class
    class Inner {
        void display() {
            System.out.println(x);  // Accesses outer class variable
        }
    }
}

```
    

---

### **14. Exception Handling**

- **Definition**: Exception handling is a mechanism to handle runtime errors, so the program can continue executing without crashing.
- **Keywords**:
    - `try`: Block of code to be tested for exceptions.
    - `catch`: Block of code to handle the exception.
    - `throw`: Used to throw an exception manually.
    - `throws`: Declares an exception to be thrown by a method.
    - `finally`: Block of code that is always executed.
- **Example**:
    
```java
try {
    int result = 10 / 0;  // Will throw ArithmeticException
} catch (ArithmeticException e) {
    System.out.println("Error: " + e.getMessage());
} finally {
    System.out.println("This is always executed.");
}

```
    

---

### **15. Generics**

- **Definition**: Generics allow classes, interfaces, and methods to operate on objects of various types while providing compile-time type safety.
- **Syntax**:
    
```java
class Box<T> {
    private T value;

    public void setValue(T value) {
        this.value = value;
    }

    public T getValue() {
        return value;
    }
}

```
    

---

### **16. Lambda Expressions (Java 8)**

- **Definition**: A feature introduced in Java 8 that allows you to pass behavior as a method argument or execute code without having to create a class.
- **Example**:
    
```java
interface MathOperation {
    int operate(int a, int b);
}

public class Test {
    public static void main(String[] args) {
        MathOperation addition = (a, b) -> a + b;
        System.out.println(addition.operate(5, 3));  // Output: 8
    }
}

```
    

---

### **17. Method References (Java 8)**

- **Definition**: A shorthand for lambda expressions, allowing methods to be referenced directly.
- **Example**:
    
```java
class Example {
    static void printMessage() {
        System.out.println("Hello, world!");
    }

    public static void main(String[] args) {
        Runnable r = Example::printMessage;
        r.run();
    }
}

```
    

---

### **18. Varargs (Variable Arguments)**

- **Definition**: A feature that allows you to pass a variable number of arguments to a method.
- **Example**:
    
```java
class Calculator {
    void add(int... numbers) {
        int sum = 0;
        for (int number : numbers) {
            sum += number;
        }
        System.out.println("Sum: " + sum);
    }
}

```
