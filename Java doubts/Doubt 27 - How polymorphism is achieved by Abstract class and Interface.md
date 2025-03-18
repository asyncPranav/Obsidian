

---



### **"The purpose of an abstract class is to achieve polymorphism" – Understanding This Statement**

✅ **Abstract classes help in achieving polymorphism by allowing method overriding in subclasses while enforcing a common interface for all derived classes.**

---

### **How Abstract Classes Achieve Polymorphism?**

1. **Abstract classes define common behavior** but allow subclasses to provide specific implementations.
2. **Method calls depend on the object type at runtime (Dynamic Method Dispatch).**
3. **A parent reference can point to a child object and call overridden methods.**

---

### **Example: Achieving Polymorphism Using an Abstract Class**

```java
// Abstract class
abstract class Animal {
    abstract void makeSound(); // Abstract method (to be implemented by subclasses)
}

// Subclass 1
class Dog extends Animal {
    void makeSound() {
        System.out.println("Dog barks!");
    }
}

// Subclass 2
class Cat extends Animal {
    void makeSound() {
        System.out.println("Cat meows!");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a1 = new Dog();  // Parent reference, Child object
        Animal a2 = new Cat();  
		
        a1.makeSound();  // Calls Dog's makeSound()
        a2.makeSound();  // Calls Cat's makeSound()
    }
}
```

✅ **Output:**

```sh
Dog barks!
Cat meows!
```

---

### **Why Does This Prove Polymorphism?**

✔ **Same method (`makeSound()`) behaves differently based on the object type.**  
✔ **Parent reference (`Animal`) calls child methods (`Dog` and `Cat` implementations).**  
✔ **Decision of which method to execute happens at runtime (Runtime Polymorphism).**

---

### **Final Explanation**

✔ **Abstract classes enable polymorphism by enforcing method overriding in subclasses.**  
✔ **They allow a single reference type (superclass) to refer to different subclass objects dynamically.**  
✔ **Method execution depends on the actual object type at runtime (not reference type).**


---


### **"An interface achieves only polymorphism" – Understanding This Statement**

✅ **An interface in Java is primarily designed to achieve polymorphism because it allows multiple classes to implement the same methods in different ways while ensuring a common behavior across them.**

---

### **How Does an Interface Achieve Polymorphism?**

1. **Common method signatures**: An interface enforces method names and parameters but does not provide implementation.
2. **Different implementations**: Classes implementing the interface define the behavior in their own way.
3. **Runtime method execution**: A parent reference (interface type) can point to different child objects, and method execution depends on the actual object.

---

### **Example: Achieving Polymorphism Using an Interface**

```java
// Interface with a common method
interface Animal {
    void makeSound();  // Method without implementation
}

// Class 1 implementing the interface
class Dog implements Animal {
    public void makeSound() {
        System.out.println("Dog barks!");
    }
}

// Class 2 implementing the interface
class Cat implements Animal {
    public void makeSound() {
        System.out.println("Cat meows!");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a1 = new Dog();  // Interface reference to Dog object
        Animal a2 = new Cat();  // Interface reference to Cat object
		
        a1.makeSound();  // Calls Dog's implementation
        a2.makeSound();  // Calls Cat's implementation
    }
}
```

✅ **Output:**

```sh
Dog barks!
Cat meows!
```

---

### **Why Does This Prove Polymorphism?**

✔ **The same method (`makeSound()`) behaves differently based on the object type.**  
✔ **The interface (`Animal`) allows multiple classes (`Dog`, `Cat`) to define their unique implementations.**  
✔ **A parent reference (`Animal`) calls child methods dynamically at runtime (Runtime Polymorphism).**

---

### **Why Does an Interface Achieve ONLY Polymorphism?**

🔹 Unlike abstract classes, **interfaces do not have constructors or instance variables**, so they cannot be used for code reuse.  
🔹 Their sole purpose is **defining a common structure** that multiple classes can implement, ensuring polymorphism.

---

### **Final Explanation**

✔ **Interfaces enforce polymorphism by allowing different classes to implement the same methods in their own way.**  
✔ **They do not support code reuse like abstract classes, making their primary goal achieving polymorphism.**



---



### **Abstract Class Achieves Partial Polymorphism & Interface Achieves 100% Polymorphism – Understanding This**

✅ **Polymorphism means "one name, many forms," allowing different classes to define methods differently while ensuring a common behavior.**

---

## **🔹 Abstract Class: Partial Polymorphism**

- **Abstract classes achieve only partial polymorphism** because they can have:
    1. **Both abstract (unimplemented) and concrete (implemented) methods**.
    2. **Instance variables and constructors**, which are not part of polymorphism but code reuse.
    3. **Methods that do not always require overriding in child classes**.

### **Example: Abstract Class (Partial Polymorphism)**

```java
abstract class Animal {
    abstract void makeSound(); // Abstract method (must be overridden)
	
    void sleep() { // Concrete method (not overridden)
        System.out.println("Animal is sleeping");
    }
}

class Dog extends Animal {
    void makeSound() { // Overriding abstract method
        System.out.println("Dog barks!");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Dog(); // Runtime Polymorphism
        a.makeSound();  // ✅ Calls Dog's makeSound() (Polymorphism)
        a.sleep();      // ✅ Calls Animal's sleep() (Not Polymorphism)
    }
}
```

✅ **Output:**

```sh
Dog barks!
Animal is sleeping
```

---

## **🔹 Interface: 100% Polymorphism**

- **Interfaces achieve complete (100%) polymorphism** because:
    1. **All methods are abstract (before Java 8) and must be overridden**.
    2. **No method has a body** (ensuring pure polymorphism).
    3. **Only method signatures exist, so every class must implement methods differently**.

### **Example: Interface (100% Polymorphism)**

```java
interface Animal {
    void makeSound(); // Abstract method (must be overridden)
}

class Dog implements Animal {
    public void makeSound() {
        System.out.println("Dog barks!");
    }
}

class Cat implements Animal {
    public void makeSound() {
        System.out.println("Cat meows!");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a1 = new Dog(); // Interface reference
        Animal a2 = new Cat(); 
		
        a1.makeSound(); // ✅ Calls Dog's method (Polymorphism)
        a2.makeSound(); // ✅ Calls Cat's method (Polymorphism)
    }
}
```

✅ **Output:**

```java
Dog barks!
Cat meows!
```

---

## **🔹 Key Differences**

|Feature|Abstract Class (Partial Polymorphism)|Interface (100% Polymorphism)|
|---|---|---|
|**Abstract Methods**|Can have both abstract and concrete methods|Only abstract methods (before Java 8)|
|**Concrete Methods**|Allowed (reduces polymorphism)|Not allowed (ensures full polymorphism)|
|**Variables**|Can have instance variables|Only public static final constants|
|**Constructors**|Can have constructors|No constructors|
|**Polymorphism**|Partial (due to concrete methods)|100% (everything is abstract)|

---

## **🔹 Final Conclusion**

✔ **Abstract classes provide partial polymorphism because they allow both abstract and concrete methods.**  
✔ **Interfaces provide 100% polymorphism as they enforce complete method overriding.**
