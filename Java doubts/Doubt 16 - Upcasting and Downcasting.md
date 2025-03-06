
---


### **1. What is Upcasting?**

**Upcasting** is the process of converting a **subclass reference** (`Child`) into a **superclass reference** (`Parent`).

✅ **It happens automatically (Implicit Casting).**  
✅ **It enables polymorphism** (Allows calling overridden methods).  
✅ **Only superclass methods and fields are accessible (unless overridden).**

### **Example of Upcasting**

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

    void wagTail() {
        System.out.println("Dog is wagging its tail");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Dog();  // ✅ Upcasting (Implicit)
        a.makeSound();  // Output: Dog barks (Dynamic Binding)
        
        // a.wagTail();  ❌ Not Allowed (Reference type decides access)
    }
}
```
### **Key Points of Upcasting**

1. **Happens implicitly** (Java automatically converts `Dog` to `Animal`).
2. **Only methods from `Animal` can be called** (except overridden ones).
3. **We cannot call `wagTail()`** because `a` is of type `Animal`, even though it refers to a `Dog` object.

---

### **2. What is Downcasting?**

**Downcasting** is the process of converting a **superclass reference** (`Parent`) back into a **subclass reference** (`Child`).

🚨 **It must be done explicitly (Manual Casting).**  
🚨 **It can cause `ClassCastException` if not done correctly.**

### **Example of Downcasting**

```java
class Animal {
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    void wagTail() {
        System.out.println("Dog is wagging its tail");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Dog();  // ✅ Upcasting (Implicit)

        Dog d = (Dog) a;  // ✅ Downcasting (Explicit)
        d.wagTail();  // Output: Dog is wagging its tail
    }
}
```

### **Key Points of Downcasting**

1. **Must be explicitly cast** (`(Dog) a`).
2. **Allows access to subclass-specific methods** (e.g., `wagTail()`).
3. **Can fail at runtime if the object is not actually of that subclass**.

---

### **3. What Happens If Downcasting is Incorrect?**

🚨 **If the object is not actually of the subclass type, Java throws `ClassCastException`.**

```java
class Animal {
}

class Dog extends Animal {
}

class Cat extends Animal {
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Cat();  // ✅ Upcasting
		
        Dog d = (Dog) a;  // ❌ Incorrect Downcasting
        // Runtime Error: ClassCastException
    }
}
```

**🔴 Why?**

- `a` actually refers to a `Cat`, **not a `Dog`**.
- So when we forcefully cast `a` to `Dog`, Java throws an error.

---

### **4. How to Prevent ClassCastException?**

Use `instanceof` before downcasting to check if the object belongs to the correct type.

```java
public class Test {
    public static void main(String[] args) {
        Animal a = new Cat();  // ✅ Upcasting
		
        if (a instanceof Dog) {
            Dog d = (Dog) a;  // ✅ Safe Downcasting
            System.out.println("Downcasting successful!");
        } else {
            System.out.println("Downcasting not possible.");
        }
    }
}
```

```sh
Downcasting not possible.
```

---

## **5. Summary: Upcasting vs. Downcasting**

|Feature|**Upcasting**|**Downcasting**|
|---|---|---|
|**Definition**|Converting a subclass reference into a superclass reference|Converting a superclass reference back into a subclass reference|
|**Happens**|Implicitly (Automatic)|Explicitly (Manual)|
|**Allowed Methods**|Only superclass methods (unless overridden)|All subclass methods|
|**Flexibility**|More flexible (Enables Polymorphism)|Less flexible (Risky if not checked)|
|**Risk**|No risk (Always safe)|Can cause `ClassCastException` if incorrect|

---

## **Final Thought**

- **Use Upcasting when you want general behavior (polymorphism).**
- **Use Downcasting carefully, only when you're sure of the actual object type.**