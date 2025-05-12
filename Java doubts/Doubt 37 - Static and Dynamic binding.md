
---


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

```java
class Animal {
    public void sound() {
        System.out.println("Animal makes a sound.");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Dog barks.");
    }
}

public class DynamicBindingExample {
    public static void main(String[] args) {
        Animal animal = new Dog();  // Animal reference, Dog object
        animal.sound();  // Dynamic binding (resolved at runtime)
    }
}
```


---

### ✅ **Difference Between Static and Dynamic Binding**

|**Feature**|**Static Binding**|**Dynamic Binding**|
|---|---|---|
|**Time of Binding**|Compile-time|Runtime|
|**Method Type**|Applies to **static, final, and private** methods|Applies to **instance methods** (method overriding)|
|**Binding Type**|Based on reference type|Based on object type|
|**Polymorphism**|Does not support polymorphism|Supports polymorphism|
|**Example**|Static methods, final methods|Instance methods with inheritance|
|**Performance**|Faster (since binding is done at compile time)|Slightly slower (since binding is done at runtime)|

---

### ✅ **Summary of Definitions:**

- **Static Binding** (Early Binding): The method or variable reference is resolved **at compile-time**. It is used for methods that cannot be overridden (e.g., static, final, or private methods).
    
- **Dynamic Binding** (Late Binding): The method reference is resolved **at runtime** based on the actual object type, enabling **polymorphism**.

- **Static Binding** is faster and occurs when method calls are resolved at compile-time. It is used for methods that cannot be overridden (like static, private, and final methods).
    
- **Dynamic Binding** happens at runtime and is used for methods that are overridden in subclasses, enabling **polymorphism** and allowing the actual method to be resolved based on the object type.



