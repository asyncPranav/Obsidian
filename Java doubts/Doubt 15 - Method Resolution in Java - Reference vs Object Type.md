
Animal ad = new Dog(); method will be called depending upon object or reference

---

In **Java**, method calls in the context of **inheritance and polymorphism** depend on whether the method is **overridden** or not.

### **Understanding `Animal ad = new Dog();`**

Here,

- `Animal` is the **reference type** (parent class).
- `Dog` is the **object type** (child class).

This setup enables **polymorphism**, meaning we can use a parent class reference to hold a child class object.

### **Method Call Behavior**

1. **If the method is overridden in the child class**
    
    - The method call is resolved at **runtime** based on the **object type** (Dog).
    - This is called **dynamic method dispatch** (runtime polymorphism).
    - The overridden method in `Dog` will be executed.
    
```java
class Animal {
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Dog barks");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal ad = new Dog();
        ad.makeSound();  // Output: Dog barks
    }
}
```
    
Even though `ad` is of type `Animal`, since `makeSound()` is **overridden**, the method in `Dog` is executed.
    
2. **If the method is not overridden in the child class**
    
    - The method from the **parent class** (`Animal`) is executed.
    - This happens because **no overridden version exists** in the child class.
    
```java
class Animal {
    void sleep() {
        System.out.println("Animal sleeps");
    }
}

class Dog extends Animal { 
    // No overriding of sleep()
}

public class Test {
    public static void main(String[] args) {
        Animal ad = new Dog();
        ad.sleep();  // Output: Animal sleeps
    }
}
```
    
3. **If the method exists only in the child class**
    
    - You **cannot** directly call it using the parent class reference.
    - You'll need to **typecast** to access the method.
    
```java
class Animal { }

class Dog extends Animal {
    void wagTail() {
        System.out.println("Dog wags tail");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal ad = new Dog();
        // ad.wagTail();  // ❌ Compile-time error: cannot find symbol
        
        // Correct approach:
        ((Dog) ad).wagTail();  // ✅ Output: Dog wags tail
    }
}
```
    

### **Conclusion**

- **Overridden methods** → Resolved **based on the object type (`Dog`)** at runtime (**Dynamic Method Dispatch**).
- **Non-overridden methods** → Resolved **based on the reference type (`Animal`)** at compile time.
- **Child-specific methods** → Cannot be accessed unless explicitly cast to the child class.

---




# **🚀 Detailed Discussion**

---


### **Understanding Method Resolution in Java: Reference vs Object Type**

This problem is about **how Java determines which method to call when an object is accessed through a reference of a parent class**. It revolves around **polymorphism**, **dynamic method dispatch**, and **method resolution rules**. Let's break it down step by step.

---

## **1. Understanding the Statement: `Animal ad = new Dog();`**

This statement consists of two parts:

- `Animal ad;` → This is a **reference variable** of type `Animal` (parent class).
- `new Dog();` → This is an **object** of type `Dog` (child class), which is assigned to the reference variable `ad`.

### **Key Observations**

- This demonstrates **upcasting**, where a subclass (`Dog`) object is assigned to a superclass (`Animal`) reference.
- **UPCASTING -**  [[Doubt 16 - Upcasting and Downcasting]]
- The reference variable (`ad`) determines **which methods can be accessed** at **compile time**.
- The actual object (`new Dog()`) determines **which overridden methods are executed** at **runtime**

---

## **2. How Method Resolution Works in Java**

### **Scenario 1: If the Method is Overridden in the Child Class**

- When a method in the child class **overrides** a method in the parent class, **Java calls the method of the child class at runtime**, even if the reference is of the parent type.
- This is known as **dynamic method dispatch** or **runtime polymorphism**.

#### **Example**

```java
class Animal {
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {  // Overridden method
        System.out.println("Dog barks");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal ad = new Dog();
        ad.makeSound();  // Output: Dog barks
    }
}
```

**Why does this happen?**

- Even though `ad` is of type `Animal`, Java **dynamically binds** the method call to `Dog`'s `makeSound()` at runtime because `makeSound()` is overridden.
- This is the essence of **runtime polymorphism**.

---

### **Scenario 2: If the Method is Not Overridden in the Child Class**

- If the method exists in the parent class but is **not overridden** in the child class, **Java executes the parent class's method** because the reference is of type `Animal`.

#### **Example**

```java
class Animal {
    void sleep() {
        System.out.println("Animal sleeps");
    }
}

class Dog extends Animal {
    // No overridden method
}

public class Test {
    public static void main(String[] args) {
        Animal ad = new Dog();
        ad.sleep();  // Output: Animal sleeps
    }
}
```

**Why does this happen?**

- Since `Dog` does **not** override `sleep()`, Java simply calls the method from `Animal` as per the **reference type**.

---

### **Scenario 3: If the Method Exists Only in the Child Class**

- If the method is defined **only in the child class**, it **cannot be accessed** using the parent class reference.
- Java will throw a **compile-time error**.

#### **Example**

```java
class Animal {
}

class Dog extends Animal {
    void wagTail() {
        System.out.println("Dog wags tail");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal ad = new Dog();
        // ad.wagTail();  // ❌ Compile-time error: cannot find symbol
        
        // Correct approach: Explicitly typecast to Dog
        ((Dog) ad).wagTail();  // ✅ Output: Dog wags tail
    }
}
```

**Why does this happen?**

- Since `wagTail()` is **not defined** in `Animal`, the **reference type** (`Animal`) does not know it exists.
- To access it, **explicit typecasting** is required: `((Dog) ad).wagTail();`

---

## **3. Other Important Points to Consider**

### **A. Static Methods Are Resolved at Compile Time (Not Runtime)**

- **Static methods are not polymorphic.** They are bound at **compile time** based on the reference type.
- This means **static methods are not overridden but hidden**.

#### **Example**

```java
class Animal {
    static void staticMethod() {
        System.out.println("Static method in Animal");
    }
}

class Dog extends Animal {
    static void staticMethod() {
        System.out.println("Static method in Dog");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal ad = new Dog();
        ad.staticMethod();  // Output: Static method in Animal
    }
}
```

**Explanation:**

- The method is resolved at **compile time** based on the **reference type** (`Animal`).
- Even though the object is of type `Dog`, the **static method from `Animal` is called**.

---

### **B. Final Methods Cannot Be Overridden**

- Methods declared as `final` in the parent class **cannot be overridden** in the child class.
- However, they are still **resolved at runtime** if they exist in both classes.

#### **Example**

```java
class Animal {
    final void finalMethod() {
        System.out.println("Final method in Animal");
    }
}

class Dog extends Animal {
    // void finalMethod() {  // ❌ Compile-time error: Cannot override final method
    //     System.out.println("Final method in Dog");
    // }
}

public class Test {
    public static void main(String[] args) {
        Animal ad = new Dog();
        ad.finalMethod();  // Output: Final method in Animal
    }
}
```

---

### **C. Private Methods Are Not Inherited**

- **Private methods are not inherited** by subclasses.
- They are resolved at **compile time** based on the reference type.

#### **Example**

```java
class Animal {
    private void privateMethod() {
        System.out.println("Private method in Animal");
    }

    void test() {
        privateMethod();  // This works within the class
    }
}

class Dog extends Animal {
    private void privateMethod() {  // This is NOT an override
        System.out.println("Private method in Dog");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal ad = new Dog();
        // ad.privateMethod();  // ❌ Compile-time error: cannot find symbol
        ad.test();  // Output: Private method in Animal
    }
}
```

**Explanation:**

- `privateMethod()` in `Animal` **is not inherited**, so `Dog` does not override it.
- Instead, `Dog` has its own separate `privateMethod()`, which is unrelated.

---

## **4. Summary of Method Resolution Rules**

|Scenario|Behavior|
|---|---|
|**Overridden Method**|Resolved at **runtime** based on the **object type** (Dynamic Method Dispatch)|
|**Non-Overridden Method**|Resolved at **compile time** based on the **reference type**|
|**Method Exists Only in Child**|Cannot be accessed unless **explicitly typecasted**|
|**Static Methods**|Resolved at **compile time** based on **reference type**|
|**Final Methods**|Cannot be overridden, but behave like instance methods|
|**Private Methods**|Not inherited, cannot be overridden|

---

## **Conclusion**

- Java determines method execution based on **whether the method is overridden or not**.
- If **overridden**, the **child class method is executed at runtime** (polymorphism).
- If **not overridden**, the **method from the reference type (parent class) is executed**.
- **Static, private, and final methods** follow different resolution rules.


---



#DOUBT  - **what does "dynamically binds the method" means explain in detail**


The phrase **"dynamically binds the method"** refers to **dynamic method dispatch**, also known as **runtime polymorphism**. This means that Java determines **which version of a method to call at runtime, based on the actual object type, not the reference type**.

---

## **1. What is "Binding" in Java?**

**Binding** is the process of **linking a method call to its actual implementation** in memory.

There are two types of binding:

1. **Static Binding (Early Binding)** → Happens at **compile time**.
2. **Dynamic Binding (Late Binding)** → Happens at **runtime**.

---

## **2. What is Dynamic Binding?**

**Dynamic binding means that the method that gets executed is determined at runtime based on the actual object's type.**

- When a method is **overridden** in a subclass, Java decides **at runtime** which version of the method to call.
- Even if a reference variable is of the parent class, the method **from the actual object (child class) is executed**.

### **Example: Dynamic Binding in Action**

```java
class Animal {
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Dog barks");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal ad = new Dog();  // Upcasting
        ad.makeSound();  // Output: Dog barks (NOT "Animal makes a sound")
    }
}
```

### **Explanation**

- **At compile time:** Java checks that `makeSound()` exists in `Animal` (the reference type).
- **At runtime:** Java sees that `ad` actually holds a `Dog` object, so it calls `Dog`'s overridden `makeSound()` instead of `Animal`'s.

This is **dynamic method dispatch**, and **Java dynamically binds `makeSound()` to the `Dog` class method at runtime**.


* At **compile time**, Java **only** looks at the **reference type**, not the actual object it holds. This is why it **does not bind the method call to the `Dog` class immediately**.

* However, at **runtime**, Java sees that `ad` actually refers to a `Dog` object and **dynamically binds** the overridden method from `Dog`.

### **Why Doesn't Java Bind It at Compile Time?**

1. **Reference Type Checking:**
    
    - Java ensures at compile time that the method exists in the reference type (`Animal`).
    - This is why if `Dog` had a method that `Animal` didn't have, calling it through `ad` (`Animal ad = new Dog();`) would result in a **compilation error**.
2. **Polymorphism & Late Binding:**
    
    - Java follows the **"Program to an interface, not an implementation"** principle.
    - The actual method to be called depends on the **object type at runtime**, allowing for **flexibility and extensibility**.


#### **What happens step by step?**

- **Compile-time:** Java checks if `makeSound()` exists in `Animal`. ✅
- **Runtime:** Java sees that `ad` holds a `Dog` object, so it dynamically binds `makeSound()` from `Dog`. ✅

### **Key Takeaway**

🔹 **Java does NOT bind the method at compile time because it only knows the reference type, NOT the actual object.**  
🔹 **It binds at runtime based on the actual object type, enabling polymorphism.**


---

## **3. Why Does Java Use Dynamic Binding?**

Dynamic binding enables **polymorphism**, allowing Java to: 
✔ Achieve **method overriding**.  
✔ Support **extensibility** (e.g., adding new subclasses without modifying existing code).  
✔ Allow **flexibility** in handling different object types via a common interface.

---

## **4. How is Dynamic Binding Different from Static Binding?**

| Feature          | **Static Binding (Early Binding)**                                     | **Dynamic Binding (Late Binding)**                          |
| ---------------- | ---------------------------------------------------------------------- | ----------------------------------------------------------- |
| Happens at       | **Compile time**                                                       | **Runtime**                                                 |
| Used for         | **Static methods, final methods, private methods, overloaded methods** | **Overridden methods**                                      |
| Method execution | Based on **reference type**                                            | Based on **object type**                                    |
| Performance      | Faster, since method resolution is done at compile time                | Slightly slower, since method resolution is done at runtime |

---

## **5. Example of Static vs Dynamic Binding**

### **Static Binding (Compile Time) Example**

```java
class Animal {
    static void staticMethod() {
        System.out.println("Animal static method");
    }
}

class Dog extends Animal {
    static void staticMethod() {
        System.out.println("Dog static method");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal ad = new Dog();
        ad.staticMethod();  // Output: Animal static method (NOT "Dog static method")
    }
}
```

**Explanation:**

- Static methods are **resolved at compile time** based on the **reference type (Animal)**.
- Even though `ad` refers to a `Dog` object, the method **is not overridden**, but **hidden**.

---

### **Dynamic Binding (Runtime) Example**

```java
class Animal {
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Dog barks");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal ad = new Dog();
        ad.makeSound();  // Output: Dog barks (Determined at runtime)
    }
}
```

- The method is **overridden**, so **Java resolves it at runtime** based on the object type (`Dog`).
- This is **dynamic binding**.

---

## **6. Key Takeaways**

✔ **Dynamic Binding happens when a method is overridden.**  
✔ **Java binds the method call at runtime, based on the object type, not the reference type.**  
✔ **Static methods, private methods, and final methods use static binding (compile-time resolution).**  
✔ **Dynamic Binding enables runtime polymorphism, making Java more flexible and extensible.**


---

# **🚀 Understand in detail**


## **Static Binding vs. Dynamic Binding in Java (Detailed Explanation)**

In Java, **binding** refers to the process of **linking a method call to its actual implementation** in memory. There are two types of binding:

1. **Static Binding (Early Binding)**
2. **Dynamic Binding (Late Binding)**

Let’s break them down **step by step** with examples and explanations.

---

# **1. Static Binding (Early Binding)**

### **What is Static Binding?**

Static binding occurs **at compile time**, meaning the method call is resolved during compilation based on the **reference type**, not the actual object type.

### **When Does Static Binding Occur?**

✔ **Method Overloading**  
✔ **Static Methods**  
✔ **Final Methods**  
✔ **Private Methods**

### **Example 1: Static Binding with Method Overloading**

```java
class StaticBindingExample {
    void show(int a) {
        System.out.println("Integer method: " + a);
    }

    void show(double b) {
        System.out.println("Double method: " + b);
    }

    public static void main(String[] args) {
        StaticBindingExample obj = new StaticBindingExample();
        obj.show(10);     // Output: Integer method: 10
        obj.show(10.5);   // Output: Double method: 10.5
    }
}
```

### **Explanation**

- The compiler **knows at compile time** which `show()` method to call based on the **method signature (parameter type)**.
- This is **method overloading**, and it follows **static binding**.

---

### **Example 2: Static Binding with Static Methods**

```java
class Parent {
    static void display() {
        System.out.println("Static method in Parent");
    }
}

class Child extends Parent {
    static void display() {
        System.out.println("Static method in Child");
    }
}

public class Test {
    public static void main(String[] args) {
        Parent p = new Child();
        p.display();  // Output: Static method in Parent
    }
}
```

### **Explanation**

- Static methods are **not overridden**, they are **hidden**.
- **Method resolution happens at compile time**, and since `p` is of type `Parent`, `Parent.display()` is called.

---

### **Example 3: Static Binding with Final Methods**

```java
class Parent {
    final void finalMethod() {
        System.out.println("Final method in Parent");
    }
}

class Child extends Parent {
    // Cannot override finalMethod() since it's final
}

public class Test {
    public static void main(String[] args) {
        Parent p = new Child();
        p.finalMethod();  // Output: Final method in Parent
    }
}
```

### **Explanation**

- `finalMethod()` is `final`, so it **cannot be overridden**.
- The compiler **binds the method at compile time**, following **static binding**.

---

### **Example 4: Static Binding with Private Methods**

```java
class Parent {
    private void privateMethod() {
        System.out.println("Private method in Parent");
    }

    void test() {
        privateMethod();  // This works within the class
    }
}

class Child extends Parent {
    private void privateMethod() {  // This is NOT overriding, just a separate method
        System.out.println("Private method in Child");
    }
}

public class Test {
    public static void main(String[] args) {
        Parent p = new Child();
        p.test();  // Output: Private method in Parent
    }
}
```

### **Explanation**

- **Private methods are not inherited**, so `privateMethod()` in `Child` is a **separate method**, not an override.
- The compiler **binds the method at compile time** to `Parent.privateMethod()`.

---

## **2. Dynamic Binding (Late Binding)**

### **What is Dynamic Binding?**

Dynamic binding occurs **at runtime**, meaning the method call is resolved based on the **actual object type**, not the reference type.

### **When Does Dynamic Binding Occur?**

✔ **Method Overriding (Polymorphism)**

### **Example 1: Dynamic Binding with Method Overriding**

```java
class Animal {
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Dog barks");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Dog();  // Upcasting
        a.makeSound();  // Output: Dog barks
    }
}
```

### **Explanation**

- The method `makeSound()` is **overridden** in `Dog`.
- Even though `a` is of type `Animal`, it **actually refers to a `Dog` object**.
- At runtime, Java **binds the method call to `Dog`'s `makeSound()`**, not `Animal`'s.

---

### **Example 2: Dynamic Binding with Multiple Subclasses**

```java
class Parent {
    void show() {
        System.out.println("Parent class method");
    }
}

class Child1 extends Parent {
    @Override
    void show() {
        System.out.println("Child1 class method");
    }
}

class Child2 extends Parent {
    @Override
    void show() {
        System.out.println("Child2 class method");
    }
}

public class Test {
    public static void main(String[] args) {
        Parent p1 = new Child1();
        Parent p2 = new Child2();

        p1.show();  // Output: Child1 class method
        p2.show();  // Output: Child2 class method
    }
}
```

### **Explanation**

- The `show()` method is overridden in `Child1` and `Child2`.
- At runtime, Java **binds the method based on the actual object type**.

---

## **3. Static Binding vs. Dynamic Binding**

|Feature|**Static Binding (Early Binding)**|**Dynamic Binding (Late Binding)**|
|---|---|---|
|**Happens at**|**Compile time**|**Runtime**|
|**Used for**|**Overloaded methods, static methods, private methods, final methods**|**Overridden methods**|
|**Method execution**|Based on **reference type**|Based on **object type**|
|**Performance**|Faster (Resolved at compile time)|Slightly slower (Resolved at runtime)|
|**Polymorphism**|No|Yes|
|**Flexibility**|Less flexible|More flexible|

---

## **4. Summary**

✔ **Static Binding (Early Binding)**

- Occurs at **compile time**
- Used for **method overloading, static methods, final methods, private methods**
- **Faster** execution since resolution happens at compile time
- Method execution depends on the **reference type**

✔ **Dynamic Binding (Late Binding)**

- Occurs at **runtime**
- Used for **method overriding (polymorphism)**
- **Slower** than static binding (since method resolution happens at runtime)
- Method execution depends on the **object type**

---

## **5. Final Thoughts**

- If you want **compile-time resolution** (faster execution), use **static binding**.
- If you want **runtime flexibility** (polymorphism), use **dynamic binding**.
