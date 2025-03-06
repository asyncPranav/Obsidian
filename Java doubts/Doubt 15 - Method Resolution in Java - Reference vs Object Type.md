
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
    
    java
    
    CopyEdit
    
    `class Animal {     void sleep() {         System.out.println("Animal sleeps");     } }  class Dog extends Animal {      // No overriding of sleep() }  public class Test {     public static void main(String[] args) {         Animal ad = new Dog();         ad.sleep();  // Output: Animal sleeps     } }`
    
3. **If the method exists only in the child class**
    
    - You **cannot** directly call it using the parent class reference.
    - You'll need to **typecast** to access the method.
    
    java
    
    CopyEdit
    
    `class Animal { }  class Dog extends Animal {     void wagTail() {         System.out.println("Dog wags tail");     } }  public class Test {     public static void main(String[] args) {         Animal ad = new Dog();         // ad.wagTail();  // ❌ Compile-time error: cannot find symbol                  // Correct approach:         ((Dog) ad).wagTail();  // ✅ Output: Dog wags tail     } }`
    

### **Conclusion**

- **Overridden methods** → Resolved **based on the object type (`Dog`)** at runtime (**Dynamic Method Dispatch**).
- **Non-overridden methods** → Resolved **based on the reference type (`Animal`)** at compile time.
- **Child-specific methods** → Cannot be accessed unless explicitly cast to the child class.

Let me know if you need further clarification! 🚀