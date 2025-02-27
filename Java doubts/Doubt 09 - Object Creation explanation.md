

```java
Television t = new Television();
```

let's break down each part to understand it in detail:

---

### 1. **Class Name: `Television`**

- This indicates that `Television` is a class in Java.
    
- A class is a blueprint for creating objects. It defines the properties (attributes) and behaviors (methods) that the objects of this class can have.
    
- In this context, `Television` might be defined as follows:
    
```java
public class Television {
    // Attributes or Properties
    String brand;
    int size;
    boolean isOn;
    
    // Constructor
    public Television() {
        this.brand = "Generic";
        this.size = 32;
        this.isOn = false;
    }
    
    // Methods or Behavior
    public void turnOn() {
        isOn = true;
        System.out.println("The TV is now ON.");
    }
    
    public void turnOff() {
        isOn = false;
        System.out.println("The TV is now OFF.");
    }
}
```
    In this example:
    
    - `Television` has three attributes: `brand`, `size`, and `isOn`.
    - It also has methods to turn the TV on and off.
    - A **constructor** is defined (`public Television()`) which initializes the object with default values.

---

### 2. **Object Reference: `t`**

- `t` is the **reference variable** that will store the memory address of the newly created `Television` object.
- It is not the object itself but rather a pointer to where the object is located in memory.
- You can use `t` to access the methods and attributes of the `Television` class. For example:
    
```java
t.turnOn();
System.out.println(t.brand);  // Outputs: Generic
```
    

---

### 3. **New Keyword: `new`**

- `new` is a keyword in Java used to **create an instance** (i.e., an object) of a class.
- It allocates memory for the new object on the heap.
- It also calls the **constructor** of the class, which initializes the object.

---

### 4. **Constructor Call: `Television()`**

- This is a call to the **constructor** of the `Television` class.
- A constructor is a special method that has the same name as the class and no return type.
- It initializes the new object with default or specified values.
- In this case, it sets:
    - `brand = "Generic"`
    - `size = 32`
    - `isOn = false`

---

### 5. **Putting it All Together**

- The statement:
    
```java
Television t = new Television();
```
    
    Can be read as:
    1. Declare a variable `t` of type `Television`.
    2. Create a new `Television` object in memory.
    3. Call the `Television()` constructor to initialize the object.
    4. Assign the memory address of the newly created object to the reference variable `t`.

---

### Example in Action:

Here's how you might use it in a complete program:

```java
public class Main {
    public static void main(String[] args) {
        Television t = new Television();  // Creating an object
        System.out.println("Brand: " + t.brand);  // Outputs: Brand: Generic
        t.turnOn();  // Outputs: The TV is now ON.
    }
}
```

---

### Summary:

- `Television t`: Declares a reference variable of type `Television`.
- `new Television()`: Creates a new object and calls the constructor.
- The constructor initializes the object with default values.
- `t` now holds the reference (memory address) of the created object, allowing you to access its attributes and methods.

This is the foundational concept of **object instantiation** in Java and is a key part of **Object-Oriented Programming**.

---

#DOUBT -  **How will you explain `Television t = new Television();` if we have not created constructor in Television class**

```java
class Television{  
    private int channel;  
    private int volume;  
    public void changeChannel(){}  
    public void changeVolume(){}  
}  
public class test {  
    public static void main(String[] args) {  
        Television t = new Television();  
        t.changeChannel();  
        t.changeVolume();  
    }
}
```

Great question! In Java, if no constructor is explicitly defined in a class, the compiler automatically provides a **default constructor**. Let's break down how this works in the given code:

---

### 1. **Class `Television` Analysis**

```java
class Television {
    private int channel;
    private int volume;
    public void changeChannel() {}
    public void changeVolume() {}
}
```

- This class has:
    - **Two private attributes**:
        - `channel` (of type `int`)
        - `volume` (of type `int`)
    - **Two public methods**:
        - `changeChannel()` - Currently, it does nothing but could be used to change the value of `channel`.
        - `changeVolume()` - Could be used to adjust the `volume`.

---

### 2. **Where's the Constructor?**

- In Java, if you do not explicitly define a constructor, the compiler **automatically provides a default constructor**.
- The default constructor looks like this (implicitly):
    
```java
public Television() {
    super(); // Calls the constructor of the superclass (Object class)
}
```
    
- This constructor:
    - **Initializes the object** without any arguments.
    - Sets all instance variables( In Java, **instance variables** are variables declared inside a class but outside of any method, constructor, or block. They represent the **attributes or properties** of an object, and each instance (object) of the class gets its own copy of these variables ) to their default values:
        - `int` variables (`channel` and `volume`) are initialized to `0`.
    - Does not perform any custom initialization since no code is provided inside it.

---

### 3. **Statement Explanation**

```java
Television t = new Television();
```

This statement does the following:

1. **Declare a Reference Variable**:
    
    - `Television t`: Declares `t` as a reference variable of type `Television`.
    - `t` can only point to objects of type `Television`.
2. **Create an Object Using `new`**:
    
    - `new` allocates memory for a new `Television` object on the **heap**.
3. **Call the Default Constructor**:
    
    - Since no constructor is explicitly defined, the compiler uses the **default constructor**.
    - This constructor initializes the object with:
        - `channel = 0`
        - `volume = 0`
4. **Assign the Reference**:
    
    - The memory address of the new `Television` object is stored in the reference variable `t`.

---

### 4. **Example of Default Values**

If you expand the class to check the values, you could do something like this:

```java
class Television {
    private int channel;
    private int volume;
	
    public void displaySettings() {
        System.out.println("Channel: " + channel);
        System.out.println("Volume: " + volume);
    }
}

public class Test {
    public static void main(String[] args) {
        Television t = new Television();
        t.displaySettings();
    }
}
```

**Output**:
```shell
Channel: 0
Volume: 0
```

- This shows that the `int` variables are automatically initialized to `0`, thanks to the default constructor.

---

### 5. **Key Takeaways**

- In Java, **if no constructor is explicitly defined**, the compiler provides a **default constructor**.
- This default constructor:
    - Has no parameters.
    - Initializes instance variables to their default values (e.g., `0` for `int`, `null` for objects, `false` for `boolean`).
- The statement `Television t = new Television();` works because the default constructor is implicitly present.

This behavior allows you to create objects even when no constructor is explicitly defined in the class.



---



**NEXT ->** [[Doubt09]]