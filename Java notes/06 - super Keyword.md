
The `super` keyword is a powerful and essential concept in Java, primarily used in the context of **inheritance**. It allows a subclass to **access members (variables and methods)** of its **parent class**. Let's explore it in detail, starting from the basics and moving to advanced use cases, ensuring a complete understanding.

---

# **1. What is `super` in Java?**

- `super` is a **reference variable** used to refer to the **immediate parent class** object.
- It is primarily used in **inheritance** to access:
    - **Parent class instance variables** (when they are hidden by subclass variables).
    - **Parent class methods** (when overridden in the subclass).
    - **Parent class constructors** (to initialize parent class properties).

---

# **2. Why Use `super`?**

- To **avoid ambiguity** when the subclass has members with the same name as the parent class.
- To **reuse parent class methods** that are overridden in the subclass.
- To **invoke the parent class constructor** for initializing inherited fields.
- To **access hidden fields** (variables in the parent class that are shadowed by subclass variables).

---

# **3. Using `super` to Access Parent Class Variables**

- If a subclass has an instance variable with the **same name** as the parent class, it hides the parent class variable.
- `super` is used to **access the hidden variable** from the parent class.

### **Example Without `super` (Problem)**

```java
class Parent {
    String name = "Parent";
}

class Child extends Parent {
    String name = "Child";

    void display() {
        System.out.println("Name: " + name); // Refers to Child's name
    }
}

public class Main {
    public static void main(String[] args) {
        Child obj = new Child();
        obj.display();
    }
}
```

**Output:**

```shell
Name: Child
```

### **Why is it "Child"?**

- The `name` variable in the `Child` class **hides** the `name` variable in the `Parent` class.
- By default, `name` refers to the variable in the **current class (Child)**.

---

### **Example Using `super` (Solution)**

```java
class Parent {
    String name = "Parent";
}

class Child extends Parent {
    String name = "Child";
	
    void display() {
        System.out.println("Child Name: " + name);           // Refers to Child's name
        System.out.println("Parent Name: " + super.name);    // Refers to Parent's name
    }
}

public class Main {
    public static void main(String[] args) {
        Child obj = new Child();
        obj.display();
    }
}
```

**Output:**

```shell
Child Name: Child
Parent Name: Parent
```

### **Explanation:**

- `super.name` refers to the `name` variable of the **Parent** class.
- This allows access to the hidden variable, resolving the naming conflict.

---

# **4. Using `super` to Call Parent Class Methods**

- `super` is used to **call methods** of the parent class that are **overridden** in the subclass.
- It helps to **reuse** the parent class functionality without rewriting the code.

### **Example:**

```java
class Parent {
    void show() {
        System.out.println("Parent's show()");
    }
}

class Child extends Parent {
    void show() {
        System.out.println("Child's show()");
    }

    void display() {
        show();          // Calls Child's show()
        super.show();    // Calls Parent's show()
    }
}

public class Main {
    public static void main(String[] args) {
        Child obj = new Child();
        obj.display();
    }
}
```

**Output:**

```shell
Child's show()
Parent's show()
```

### **Explanation:**

- `show()` refers to the **overridden method** in the Child class.
- `super.show()` explicitly calls the **Parent's** version of `show()`.

---

# **5. Using `super()` to Call Parent Class Constructors**

- `super()` is used to **call the constructor of the parent class**.
- This is useful for **constructor chaining** and **initializing parent class fields**.
- It **must be the first statement** in the subclass constructor.

### **Example Without `super()` (Default Call)**


```java
class Parent {
    Parent() {
        System.out.println("Parent's Constructor");
    }
}

class Child extends Parent {
    Child() {
        System.out.println("Child's Constructor");
    }
}

public class Main {
    public static void main(String[] args) {
        Child obj = new Child();
    }
}
```

**Output:**

```shell
Parent's Constructor
Child's Constructor
```

### **Explanation:**

- Java **implicitly** calls `super()` if not specified.
- This calls the **no-argument constructor** of the Parent class.

---

### **Example Using `super()` (Explicit Call)**

```java
class Parent {
    Parent(String message) {
        System.out.println("Parent's Constructor: " + message);
    }
}

class Child extends Parent {
    Child() {
        super("Hello from Child"); // Explicitly calling Parent's parameterized constructor
        System.out.println("Child's Constructor");
    }
}

public class Main {
    public static void main(String[] args) {
        Child obj = new Child();
    }
}
```

**Output:**

```shell
Parent's Constructor: Hello from Child
Child's Constructor
```

### **Explanation:**

- `super("Hello from Child")` explicitly calls the **parameterized constructor** of the Parent class.
- This allows passing custom values to the Parent class constructor.

---

# **6. Rules for Using `super()`**

- `super()` **must be the first statement** in the subclass constructor.
- If not explicitly written, Java **implicitly** calls `super()` with no arguments.
- It can be used **only once** in a constructor.
- It is used to **initialize inherited fields**.

---

# **7. Using `super` to Access Parent Class Methods and Variables Together**

**Example:**

```java
class Animal {
    String type = "Animal";

    void displayType() {
        System.out.println("Type: " + type);
    }
}

class Dog extends Animal {
    String type = "Dog";

    void display() {
        System.out.println("Type: " + type);         // Child's type
        System.out.println("Super Type: " + super.type); // Parent's type
        super.displayType();                        // Parent's method
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.display();
    }
}
```

**Output:**

```shell
Type: Dog
Super Type: Animal
Type: Animal
```

---

# **8. Key Points to Remember**

- `super` is a **reference to the parent class** object.
- It is used to:
    - Access **parent class variables** when hidden by subclass variables.
    - Call **parent class methods** when overridden in the subclass.
    - Invoke **parent class constructors** for initialization.
- `super()` **must be the first statement** in the constructor.
- `super` **cannot be used in static methods** as it refers to instance objects.
- It helps to **avoid ambiguity** and **reuse parent class functionality**.

---

# **9. Summary**

- `super` provides a way for subclasses to **access and reuse** the properties and methods of their parent class.
- It enhances **code maintainability** by promoting **code reuse**.
- It is crucial for **method overriding**, **constructor chaining**, and **variable hiding** scenarios.

---

This comprehensive explanation covers every aspect of the `super` keyword in Java. If you need more examples or further clarifications, feel free to ask!

