

---

# **`static` Keyword in Java: Comprehensive Guide**


---


## **Introduction:**

The `static` keyword in Java is a **non-access modifier** used for memory management and to define class-level behavior. It can be applied to:

- **Variables** (Static Variables)
- **Methods** (Static Methods)
- **Blocks** (Static Initialization Blocks)
- **Nested Classes** (Static Nested Classes)

When a member is declared as `static`, it belongs to the **class** rather than any **instance** of the class.

---

## **1. Static Variables:**

### **Definition:**

A **static variable** is shared among all instances of a class. It is initialized only **once** when the class is **loaded** into memory.

### **Syntax:**

```java
class Example {
    static int count = 0; // Static variable
}
```

### **Characteristics:**

- **Memory Allocation:** Only once in the **class area** (method area).
- **Shared:** Among all instances of the class.
- **Access:** Directly using the **class name** (`Example.count`).

### **Example:**

```java
class Counter {
    static int count = 0; // Static variable
	
    Counter() {
        count++;
        System.out.println("Count: " + count);
    }
	
    public static void main(String[] args) {
        new Counter(); // Count: 1
        new Counter(); // Count: 2
        new Counter(); // Count: 3
    }
}
```

### **Output:**

```sh
Count: 1   
Count: 2   
Count: 3
```

---

## **2. Static Methods:**

### **Definition:**

**Static methods** belong to the **class**, not to any specific **object**. They can be called **without creating an object** of the class.

### **Syntax:**

```java
static void display() {
    System.out.println("Static Method");
}
```

### **Characteristics:**

- Can **access static data** directly.
- **Cannot** access **non-static data** directly (requires an object).
- **Cannot** use `this` or `super` keywords.

### **Example:**

```java
class StaticMethodDemo {
    static int a = 10; // Static variable
    int b = 20; // Non-static variable
	
    static void show() {
        System.out.println("Static Variable: " + a);
        // System.out.println(b); // ❌ Error: Cannot access non-static variable
    }
	
    public static void main(String[] args) {
        show(); // Direct call without object
    }
}
```

### **Output:**

```shell
Static Variable: 10
```

---

## **3. Static Blocks:**

### **Definition:**

A **static block** is used to **initialize static variables**. It is executed **when the class is loaded**, **before** the **main method** or any **object creation**.

### **Syntax:**

```java
static {
    System.out.println("Static Block Executed");
}
```

### **Example:**

```java
class StaticBlockDemo {
    static {
        System.out.println("Inside static block");
    }
	
    public static void main(String[] args) {
        System.out.println("Inside main method");
    }
}
```

### **Output:**

```shell
Inside static block  
Inside main method  
```

### **Multiple Static Blocks:**

```java
class MultipleStaticBlocks {
    static {
        System.out.println("Static Block 1");
    }
    
    static {
        System.out.println("Static Block 2");
    }
	
    public static void main(String[] args) {
        System.out.println("Inside main method");
    }
}
```

### **Output:**

```shell
Static Block 1  
Static Block 2  
Inside main method  
```

### **Key Points:**

- **Executed in the order** they are defined.
- Useful for **complex static initialization**.

---

## **4. Static Nested Classes:**

### **Definition:**

A **static nested class** is a **static class** defined **inside** another class. It can be accessed **without creating an instance** of the **outer class**.

### **Syntax:**

```java
class Outer {
    static class Inner {
        void display() {
            System.out.println("Static Nested Class");
        }
    }
}
```

### **Example:**

```java
public class StaticNestedClassDemo {
    public static void main(String[] args) {
        Outer.Inner obj = new Outer.Inner(); // Direct access without Outer class instance
        obj.display();
    }
}
```

### **Output:**

```shell
Static Nested Class
```

### **Characteristics:**

- **Can access static members** of the **outer class**.
- **Cannot directly access** **non-static members** of the **outer class**.

---

## **5. Execution Order:**

### **Order of Execution:**

1. **Static Variable Initialization**
2. **Static Block Execution**
3. **Main Method (if present)**
4. **Static Method (when called)**
5. **Object Creation (Constructor Call)**
6. **Non-Static Block Execution**
7. **Non-Static Method (when called)**

### **Example to Demonstrate This**


```java
public class ExecutionOrderDemo {
	
    // Static variable
    static int staticVar = initializeStaticVar();
	
    // Static block
    static {
        System.out.println("Inside static block");
    }
	
    // Non-static variable
    int nonStaticVar = initializeNonStaticVar();
	
    // Non-static block
    {
        System.out.println("Inside non-static block");
    }
	
    // Constructor
    public ExecutionOrderDemo() {
        System.out.println("Inside constructor");
	}
	
    // Static method
    public static void staticMethod() {
        System.out.println("Inside static method");
    }
	
    // Non-static method
    public void nonStaticMethod() {
        System.out.println("Inside non-static method");
    }
	
    // Initialize static variable
    private static int initializeStaticVar() {
        System.out.println("Static variable initialized");
        return 1;
    }
	
    // Initialize non-static variable
    private int initializeNonStaticVar() {
        System.out.println("Non-static variable initialized");
        return 2;
    }
	
    public static void main(String[] args) {
        System.out.println("Main method started");
        staticMethod(); // Call static method
		
        System.out.println("\nCreating an object...\n");
        ExecutionOrderDemo obj = new ExecutionOrderDemo(); // Create an object
        obj.nonStaticMethod(); // Call non-static method
    }
}
```

### **Output:**

```shell
Static variable initialized  
Inside static block  
Main method started  
Inside static method  

Creating an object...

Non-static variable initialized  
Inside non-static block  
Inside constructor  
Inside non-static method  
```

---

## **6. Use Cases of `static` Keyword:**

- **Utility or Helper Methods:** `Math.pow()`, `Collections.sort()`.
- **Memory Management:** Avoid unnecessary object creation.
- **Singleton Pattern:** Implementing **single-instance classes**.
- **Constants:** Using `static final` for **global constants**.
- **Accessing Common Properties:** Across all instances of a class.

---

## **7. Important Points to Remember:**

- **Static variables** are initialized **before static blocks**.
- **Static methods** **cannot** access **non-static data** directly.
- **`this` and `super`** **cannot** be used in **static contexts**.
- **Static blocks** are executed in the **order they appear** in the class.
- **Memory Management:** Helps in reducing **memory overhead**.

---

## **Conclusion:**

The **`static` keyword** is a **powerful feature** in Java, providing **memory efficiency**, **utility method support**, and **class-level behavior**. Mastering static behavior is crucial for building **efficient** and **maintainable** Java applications.



---



**NEXT ->** [[13 - final Keyword]]  or/and [[14 - Polymorphism]]
