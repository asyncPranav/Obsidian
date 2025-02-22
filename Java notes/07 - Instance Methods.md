

An **instance method** is a method that **belongs to an instance** of a class. In simpler terms, these are methods that require an **object** to be created before they can be called. They **operate on instance variables** of the class and can **access other instance methods and variables** directly.

---

# **1. What is an Instance Method?**

- An instance method is **associated with an object** of a class.
- It is **called on an object** and can access instance variables and other instance methods.
- Instance methods **do not** have the `static` keyword.
- They can access:
    - **Instance variables** directly (without object reference).
    - **Instance methods** directly (without object reference).
    - **Static variables and methods** using the class name (though not recommended for readability).

---

# **2. Characteristics of Instance Methods**

- **Belongs to an Object:** They are called using an object reference (e.g., `obj.methodName()`).
- **Can Access Instance Variables:** Directly access or modify instance variables of the class.
- **No `static` Keyword:** Unlike static methods, instance methods are **not** prefixed with `static`.
- **Require Object Creation:** You need to create an object of the class to invoke an instance method.
- **Can Use `this`:** They can use the `this` keyword to refer to the current object.

---

# **3. Syntax of an Instance Method**

```java
return_type methodName(parameters) {
    // Method body
}
```

### **Example:**

```java
public int calculateArea() {
    return length * width;
}
```

- `public`: Access modifier.
- `int`: Return type of the method.
- `calculateArea`: Method name.
- `()` : Parameter list (empty in this case).
- `{ ... }`: Method body containing the logic.

---

# **4. Example of Instance Methods in Java**

```java
class Rectangle {
    // Instance variables
    int length;
    int width;

    // Constructor to initialize length and width
    public Rectangle(int l, int w) {
        length = l;
        width = w;
    }

    // Instance method to calculate area
    public int calculateArea() {
        return length * width;
    }

    // Instance method to calculate perimeter
    public int calculatePerimeter() {
        return 2 * (length + width);
    }

    // Instance method to display rectangle details
    public void displayDetails() {
        System.out.println("Length: " + length);
        System.out.println("Width: " + width);
        System.out.println("Area: " + calculateArea());
        System.out.println("Perimeter: " + calculatePerimeter());
    }
}

public class Main {
    public static void main(String[] args) {
        // Creating an object of Rectangle
        Rectangle rect1 = new Rectangle(10, 5);
        
        // Calling instance methods using the object
        rect1.displayDetails();
        
        System.out.println();
        
        // Creating another object of Rectangle
        Rectangle rect2 = new Rectangle(7, 7);
        
        // Calling instance methods using the object
        rect2.displayDetails();
    }
}
```

### **Output:**

```shell
Length: 10
Width: 5
Area: 50
Perimeter: 30

Length: 7
Width: 7
Area: 49
Perimeter: 28
```

### **Explanation:**

- **`calculateArea()`** and **`calculatePerimeter()`** are instance methods that operate on the instance variables `length` and `width`.

- **`displayDetails()`** is an instance method that calls other instance methods (`calculateArea()` and `calculatePerimeter()`) directly.

- Instance methods are called using **object references** (`rect1.displayDetails()` and `rect2.displayDetails()`).

- **Different objects** can have **different states** (`length` and `width`), and the instance methods operate on these states individually.

---

# **5. Key Points to Remember**

- Instance methods are **not static**.
- They can **access and modify instance variables** directly.
- They are called using **object references**.
- They can **call other instance methods** directly.
- They can **access static variables and methods** (though it’s better to use the class name for clarity).

---

# **6. Difference Between Instance Method and Static Method**

|Instance Method|Static Method|
|---|---|
|Belongs to an **object**.|Belongs to the **class** itself.|
|Called using an **object reference**.|Called using the **class name**.|
|Can access **instance variables** and **instance methods**.|Cannot access instance variables or methods directly (needs an object reference).|
|Can access **static variables and methods**.|Can access only **static variables and methods**.|
|Does **not** have the `static` keyword.|**Has** the `static` keyword.|
|Each object has its **own copy** of instance variables.|**Shared** across all instances of the class.|

---

# **7. When to Use Instance Methods?**

- When you need to **access or modify instance variables**.
- When the method behavior **depends on the state of an object**.
- When you want to **encapsulate** behavior related to a specific object.

---

# **8. Real-World Analogy**

Imagine a **remote control** for a TV:

- Each remote control (object) has its **own state** (like battery level, brand, etc.).
- **Instance methods** are like the buttons on the remote:
    - `increaseVolume()`, `decreaseVolume()`, `changeChannel()`.
- Each method **operates on the state** of that particular remote control, not on other remotes.

---

# **9. Summary**

- Instance methods in Java are **object-specific** methods that operate on the instance variables of the class.
- They are **not static** and are called using **object references**.
- They **encapsulate** behavior that is **specific to each object**.
- They **promote code reusability** and **maintainability**.