
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
- **Binding -** [[Doubt 17 - Binding in Java]]
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


**Method calling dependency : ** [[[]]]
