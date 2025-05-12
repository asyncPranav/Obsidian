
---


### **Definition of Static and Dynamic Binding**

#### **1. Static Binding (Early Binding)**

**Static Binding** refers to the process where method calls or variable references are resolved **at compile-time**. This binding happens when the type of the object is known before the program is run.

- **In Static Binding**, method calls are resolved based on the reference type, not the object type.
    
- It is used for methods that are **static**, **final**, or **private**, as these methods cannot be overridden in subclasses.
    

##### Example:

```java
class Animal {
    public static void sound() {
        System.out.println("Animal makes a sound.");
    }
}

public class StaticBindingExample {
    public static void main(String[] args) {
        Animal animal = new Animal();
        animal.sound();  // Static binding (resolved at compile time)
    }
}
```

#### **2. Dynamic Binding (Late Binding)**

**Dynamic Binding** refers to the process where method calls are resolved **at runtime**. This binding occurs when the actual type of the object is determined during the program execution, allowing **polymorphism** to work.

- **In Dynamic Binding**, method calls are resolved based on the **actual object type** at runtime, not the reference type.
    
- It is primarily used for **overridden methods** in object-oriented programming.
    

##### Example:

```jav
```

### ✅ **Summary of Definitions:**

- **Static Binding** (Early Binding): The method or variable reference is resolved **at compile-time**. It is used for methods that cannot be overridden (e.g., static, final, or private methods).
    
- **Dynamic Binding** (Late Binding): The method reference is resolved **at runtime** based on the actual object type, enabling **polymorphism**.