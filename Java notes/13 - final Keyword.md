
# **`final` Keyword in Java: Complete Guide**

## **Introduction:**

The **`final`** keyword in Java is a **non-access modifier** used to apply restrictions on **variables**, **methods**, and **classes**. It is primarily used for:

- **Constants** (with `final` variables)
- **Preventing method overriding** (with `final` methods)
- **Preventing inheritance** (with `final` classes)

---

## **1. Final Variables:**

### **Definition:**

A **final variable** is a **constant** whose **value cannot be changed** once **initialized**.

### **Syntax:**

```java
final int MAX_VALUE = 100;
```

### **Characteristics:**

- **Initialization Methods:**
    - At the **time of declaration**.
    - **Inside a constructor** (for **instance variables**).
    - **Inside a static block** (for **static variables**).

---

### **1.1. Final Primitive Variable:**

```java
public class FinalVariableDemo {
    final int SPEED_LIMIT = 90; // Initialized during declaration
	
    void run() {
        // SPEED_LIMIT = 100; // ❌ Error: Cannot assign a value to final variable
        System.out.println("Speed Limit: " + SPEED_LIMIT);
    }
	
    public static void main(String[] args) {
        FinalVariableDemo obj = new FinalVariableDemo();
        obj.run();
    }
}
```

### **Output:**

```shell
Speed Limit: 90
```

---

### **1.2. Final Reference Variable:**

```java
public class FinalReferenceDemo {
    final StringBuilder sb = new StringBuilder("Hello");
	
    void modify() {
        sb.append(" World"); // ✅ Allowed: Modifying the object itself
        // sb = new StringBuilder("New"); // ❌ Error: Cannot reassign a final reference
        System.out.println(sb);
    }
	
    public static void main(String[] args) {
        FinalReferenceDemo obj = new FinalReferenceDemo();
        obj.modify();
    }
}

```

### **Output:**

```shell
Hello World
```

### **Explanation:**

- **Final Reference Variable:** You **cannot reassign** the **reference**, but you can **modify the object** it points to.

---

### **1.3. Blank Final Variable:**

A **blank final variable** is a **final variable** that is **not initialized** at the time of **declaration**. It can be initialized:

- **Inside a constructor** (for **instance variables**).
- **Inside a static block** (for **static variables**).

#### **Example:**

```java
public class BlankFinalVariableDemo {
    final int MAX;
	
    BlankFinalVariableDemo() {
        MAX = 100; // ✅ Allowed: Initialized in constructor
    }
	
    public static void main(String[] args) {
        BlankFinalVariableDemo obj = new BlankFinalVariableDemo();
        System.out.println("MAX: " + obj.MAX);
    }
}
```

### **Output:**

```shell
MAX: 100
```

---

### **1.4. Final Static Variable:**

- **Also called a Constant:** Commonly declared with `public static final`.
- **Memory Efficiency:** Stored in the **method area**.

#### **Example:**

```java
public class FinalStaticVariableDemo {
    public static final int AGE = 21;
	
    public static void main(String[] args) {
        System.out.println("Age: " + AGE);
    }
}
```

### **Output:**

```shell
Age: 21
```


**Final Variable :** [[Doubt 23 - Final Variable]] 

---

## **2. Final Methods:**

### **Definition:**

A **final method** **cannot be overridden** by **subclasses**.

### **Syntax:**

```java
class Parent {
    final void show() {
        System.out.println("Final Method in Parent");
    }
}
```
---

### **2.1. Example of Final Method:**

```java
class Parent {
    final void display() {
        System.out.println("Display from Parent");
    }
}

class Child extends Parent {
    // void display() { // ❌ Error: Cannot override the final method
    //     System.out.println("Display from Child");
    // }
}

public class FinalMethodDemo {
    public static void main(String[] args) {
        Child obj = new Child();
        obj.display(); // Calls Parent's display method
    }
}
```

### **Output:**

```java
Display from Parent
```

### **Use Cases:**

- Preventing **unintended method overriding**.
- Ensuring **method behavior consistency** across subclasses.

---

**FINAL METHOD :** [[Doubt 24 - Final Method]]

---

## **3. Final Classes:**

### **Definition:**

A **final class** **cannot be inherited** by other classes.

### **Syntax:**

```java
final class Animal {
}

// class Dog extends Animal { // ❌ Error: Cannot inherit from a final class
// }
```

---

### **3.1. Example of Final Class:**

```java
final class Car {
    void drive() {
        System.out.println("Driving a car...");
    }
}

// class SportsCar extends Car { // ❌ Compilation Error: Cannot extend final class
// }

public class FinalClassDemo {
    public static void main(String[] args) {
        Car car = new Car();
        car.drive();
    }
}
```

### **Output:**

```shell
Driving a car...
```

---

**FINAL CLASS :** 

---

## **4. Final Keyword with Inheritance:**

### **4.1. Final Variables in Inheritance:**

- **Cannot be re-initialized** in **subclasses**.

```java
class Parent {
    final int x = 10;
}

class Child extends Parent {
    void change() {
        // x = 20; // ❌ Error: Cannot assign a value to final variable
    }
}
```

---

### **4.2. Final Methods in Inheritance:**

- **Cannot be overridden** by **subclasses**, ensuring **method consistency**.

---

### **4.3. Final Classes in Inheritance:**

- **Cannot be extended**, **preventing modification** of **class behavior**.

---

## **5. Final and Constructors:**

### **Key Points:**

- **Final variables** can be **initialized** in the **constructor** but **cannot be modified** afterward.
- Useful for **dependency injection** or **initialization logic**.

---

## **6. Final and Performance:**

### **Performance Benefits:**

- **Final methods** are considered for **inlining** by the **Java compiler**, potentially improving **performance**.
- **Final variables** can help the **JVM optimize memory usage**.

---

## **7. Common Mistakes with Final Keyword:**

1. **Trying to modify final variables:**

```java
final int x = 10;
// x = 20; // ❌ Error
```

2. **Overriding final methods:**

```java
class Parent {
    final void show() {}
}

class Child extends Parent {
    // void show() {} // ❌ Error: Cannot override final method
}
```

3. **Extending final classes:**

```java
final class Animal {}

// class Dog extends Animal {} // ❌ Error: Cannot inherit from final class
```

---

## **8. Key Takeaways:**

- **Final Variables:** Make **constants**, can be initialized **only once**.
- **Final Methods:** Prevent **method overriding**, ensure **method consistency**.
- **Final Classes:** Prevent **inheritance**, useful for **utility** or **core behavior classes**.
- **Memory and Performance:** **Helps JVM optimize** and **potentially improve performance**.

---

## **Conclusion:**

The **`final` keyword** is a **powerful tool** in Java for achieving **immutability**, **ensuring class integrity**, and **improving performance**. It plays a **vital role** in building **secure**, **reliable**, and **maintainable code**.


---


**NEXT ->** [[14 - Polymorphism]]