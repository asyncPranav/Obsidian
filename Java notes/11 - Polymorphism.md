

---

# 📚 **Polymorphism in Java**

## ✅ **Definition:**

Polymorphism in Java refers to the ability of an object to take on many forms. It allows a single action to behave differently based on the object that is performing it. Polymorphism is a fundamental concept of **Object-Oriented Programming (OOP)** that enhances flexibility, scalability, and maintainability of code.

---

## 🧠 **Types of Polymorphism:**

1. **Compile-Time (Static) Polymorphism:** Also known as **method overloading**.
2. **Runtime (Dynamic) Polymorphism:** Also known as **method overriding**.

---

# 1. 🌀 **Compile-Time (Static) Polymorphism**

### ✅ **Definition:**

Compile-time polymorphism is achieved by **method overloading**, where **multiple methods** have the **same name** but **different parameter lists** (**different number or type of parameters**).

### 🛠️ **Example:**

```java
class Calculator {
    // Method with two integer parameters
    public int add(int a, int b) {
        return a + b;
    }

    // Overloaded method with three parameters
    public int add(int a, int b, int c) {
        return a + b + c;
    }

    // Overloaded method with different parameter types
    public double add(double a, double b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println(calc.add(5, 10));           // Calls method with 2 int parameters
        System.out.println(calc.add(5, 10, 15));      // Calls method with 3 int parameters
        System.out.println(calc.add(5.5, 10.2));      // Calls method with 2 double parameters
    }
}

```

### 🎯 **Key Points:**

- **Binding Time:** Compile-time (**static binding**).
- **Method Signature:** Must **differ** in **parameter type**, **number**, or **both**.
- **Return Type:** Can **differ**, but **not enough** to **differentiate methods**.
- **Inheritance Required?:** **No**.

### ❗ **Exceptions & Facts:**

- Method overloading **cannot** be achieved by **changing only the return type**.
- Method overloading **can** be done with **static methods**.
- **Constructors** can also be **overloaded**.

---

# 2. 🚦 **Runtime (Dynamic) Polymorphism**

### ✅ **Definition:**

Runtime polymorphism is achieved by **method overriding**, where a **subclass** provides a **specific implementation** of a **method** already **defined** in its **superclass**.

### 🛠️ **Example:**

```java
class Animal {
    // Method to be overridden
    public void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    // Overriding method
    @Override
    public void sound() {
        System.out.println("Dog barks");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myAnimal = new Dog(); // Upcasting
        myAnimal.sound(); // Calls the Dog's sound() method at runtime
    }
}

```
### 🎯 **Key Points:**

- **Binding Time:** **Runtime** (**dynamic binding**).
- **Method Signature:** Must be **exactly the same** in **parent** and **child classes**.
- **Inheritance Required?:** **Yes**.
- **Annotations:** Using `@Override` is **recommended** for **clarity**.
- The **method name, return type**, and **parameters** must be **exactly the same**.
- The **access modifier** of the overriding method **cannot be more restrictive** than the overridden method.
- It supports **runtime polymorphism** using **dynamic method dispatch**.
- The **method in the child class** is called **based on the object type**, not the **reference type**.

### ❗ **Exceptions & Facts:**

- **Static methods** cannot be **overridden**, but can be **hidden**.
- **Private methods** are **not inherited**, so they cannot be **overridden**.
- **Final methods** cannot be **overridden**.

---

# 💡 **Polymorphism with Interfaces:**

```java
interface Animal {
    void sound();
}

class Dog implements Animal {
    @Override
    public void sound() {
        System.out.println("Dog barks");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myAnimal = new Dog();
        myAnimal.sound(); // Output: Dog barks
    }
}
```

---

# 🧠 **Differences Between Method Overloading and Method Overriding:**

|**Feature**|**Compile-Time Polymorphism (Overloading)**|**Runtime Polymorphism (Overriding)**|
|---|---|---|
|**Binding Time**|**Compile-Time** (**Static Binding**)|**Runtime** (**Dynamic Binding**)|
|**Inheritance Required?**|**No**|**Yes**|
|**Method Signature**|**Must differ** (**parameter type/number**)|**Must be exactly the same**|
|**Polymorphism Type**|**Static**|**Dynamic**|
|**Return Type**|**Can differ**, but not the **sole criterion**|**Must be same or covariant**|
|**Annotations**|**Not applicable**|**`@Override` recommended**|
|**Static Methods**|**Can overload static methods**|**Cannot override, only hide**|
|**Private Methods**|**Can overload**|**Cannot override**|

---


# 🧠 **5. Polymorphism with Abstract Classes & Interfaces:**

### 📂 **Abstract Class Example:**

```java
abstract class Vehicle {
    abstract void start();
}

class Car extends Vehicle {
    @Override
    void start() {
        System.out.println("Car starts with a key");
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle myVehicle = new Car(); // Runtime Polymorphism
        myVehicle.start(); // Calls Car's start() method
    }
}
```

### 📂 **Interface Example:**

```java
interface Animal {
    void makeSound();
}

class Cat implements Animal {
    @Override
    public void makeSound() {
        System.out.println("Meow");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myAnimal = new Cat(); // Runtime Polymorphism
        myAnimal.makeSound(); // Calls Cat's makeSound() method
    }
}
```

---

# 🕹️ **6. Real-Life Examples of Polymorphism:**

1. **Print Function:** `System.out.println()` can accept **different types** of **parameters** (**String**, **int**, **Object**, etc.).
2. **Shapes Example:** A method `draw()` can be used for **different shapes** like **Circle**, **Rectangle**, **Triangle**, etc.

---

# 🔥 **7. Advanced Topics in Polymorphism:**

### 💡 **Covariant Return Type:**

- Allows **overridden methods** to **return a more specific type** than the **method in the parent class**

```java
class Parent {
    Parent get() {
        return this;
    }
}

class Child extends Parent {
    @Override
    Child get() {
        return this;
    }
}
```

### 💡 **Polymorphic Arrays:**

- An array of the **parent class type** can hold **objects of child classes**.

```java
Animal[] animals = new Animal[2];
animals[0] = new Dog();
animals[1] = new Cat();
```

### 💡 **Dynamic Method Dispatch:**

- **JVM** decides which **method implementation** to execute at **runtime**.

---

# 🚧 **8. Limitations of Polymorphism:**

- **Static methods** cannot be **overridden**, only **hidden**.
- **Final methods** cannot be **overridden**.
- **Constructors** do not exhibit **polymorphic behavior**.
- **Private methods** cannot be **overridden**.


---


# 🚨 **Common Mistakes & Exceptions:**

1. **Overloading vs. Overriding:**

```java
class Parent {
    void display(int a) { } // Method 1
}

class Child extends Parent {
    // Overloaded, not overridden
    void display(double a) { } // Method 2
}
```

- **Overloading:** Different **parameter type**, considered as a **new method**.
- **Overriding:** Would require the **same method signature**.

---

1. **Static Method Limitation:**

```java
class Parent {
    static void method() {
        System.out.println("Parent's static method");
    }
}

class Child extends Parent {
    static void method() {
        System.out.println("Child's static method");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent obj = new Child();
        obj.method(); // Output: Parent's static method (Method Hiding, not Overriding)
    }
}
```

- **Static methods are hidden, not overridden.**
- **Method called** is **based on reference type**, not **object type**.

---

## 🧬 **Summary:**

Polymorphism enhances code **flexibility** and **maintainability**, but understanding the **difference between overloading and overriding** is crucial to avoid **common pitfalls**, especially with **inheritance**, **static methods**, and **method signatures**.


---

#DOUBT  - **when we access method1 then child method is accessible upon object obj of Parent class but when we access method2(34) then child method is not accessible upon object obj of Parent class**

```java
class Parent {
    Parent() {
        System.out.println("Parent's constructor");
    }
	
    void method1() {
        System.out.println("Parent's method");
    }
	
    void method2() {
        System.out.println("Parent's method");
    }
}

class Child extends Parent {
    Child() {
        System.out.println("Child's constructor");
    }
	
    @Override
    void method1() {
        System.out.println("Child's method");
    }
	
    void method2(int a) {
        System.out.println("Child's overloaded method with int parameter");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent obj = new Child(); // Reference of Parent, Object of Child
        obj.method1(); // ✅ Child's method (Method Overriding)
        obj.method2(); // ✅ Parent's method
        // obj.method2(1); // ❌ Compilation Error
    }
}

```

Great observation! This behavior is due to the difference between **method overriding** and **method overloading** in Java, and how **dynamic binding** works. Let's break it down in detail:

---

### 🚦 **The Code:**

```java
Parent obj = new Child();
obj.method1();    // Output: Child's method
obj.method2();    // Output: Parent's method
//obj.method2(1); // Compilation Error
```

---

### 🧠 **Why Does This Happen?**

#### 1. **Method Overriding (`method1`)**

```java
void method1() {
    System.out.println("Child's method");
}
```

- `method1()` is **overridden** in the **Child** class.
- **Dynamic Binding:** At **runtime**, the **JVM** determines the **actual object's type** (`Child`), not just the **reference type** (`Parent`).
- Therefore, `obj.method1()` calls the **Child’s method1**, resulting in:

```shell
Child's method
```

---

#### 2. **Method Overloading (`method2(int a)`)**

```java
void method2(int a) {
    System.out.println("Parent's method");
}
```

- `method2(int a)` is **not** an **overridden method** but an **overloaded method** in the **Child** class.
- **Overloading** is a **compile-time concept**, meaning the **method signature** is **resolved at compile time**, based on the **reference type**.
- Since the **reference type** is `Parent`, only the **methods declared in `Parent`** are **accessible**.

##### **What Methods Are Visible in `Parent`?**

```java
void method2() {
    System.out.println("Parent's method");
}
```

- `Parent` has a method `method2()` with **no parameters**, but it **does not have** `method2(int a)`.
- Therefore, when you try to call `obj.method2(1)`, the **compiler checks the reference type (`Parent`)**, does **not find `method2(int a)`**, and throws a **compilation error**:

```shell
error: method2(int) is not defined in Parent
```

---

### ⚠️ **Key Concept:**

|**Concept**|**Method Overriding**|**Method Overloading**|
|---|---|---|
|**Binding Type**|**Runtime (Dynamic Binding)**|**Compile-Time (Static Binding)**|
|**Resolution Based On**|**Actual object type** (`Child` in this case)|**Reference type** (`Parent` in this case)|
|**Inheritance Required?**|**Yes**|**No**|
|**Method Signature**|Must be **exactly the same**|Must **differ in parameters**|

---

### 🧬 **Simplified Explanation:**

1. When you call `obj.method1()`, the method is **overridden**, so the **Child's version** is called (**Runtime Polymorphism**).
    
2. When you call `obj.method2()`, since there is a **matching method signature** in the **Parent class**, it calls the **Parent's method2()**.
    
3. When you try to call `obj.method2(1)`, the **compiler only sees** the **reference type (`Parent`)**, and since **no method2(int)** exists in **Parent**, it fails.
    

---

### 💡 **How to Access `method2(int a)` from Child Class?**

If you want to **access `method2(int a)`**, you need a **Child reference type**, like this:

```java
Child obj = new Child();
obj.method2(1); // Output: Parent's method
```

Now, the **method2(int a)** is **visible**, and you can **call it** without any **compilation error**.

---

### 😊 **Summary:**

- **Overriding:** Works with **dynamic binding**, actual object's method is called.
- **Overloading:** Works with **static binding**, only methods **visible to the reference type** are accessible.
- In your code, `obj` is of type `Parent`, so only **methods declared in `Parent`** are **visible**, even if the **object** is **actually of type `Child`**.

---

#DOUBT - **I did not understand the answer can you explain again in detail**


Sure! Let's break it down thoroughly and understand why **method overriding** works differently from **method overloading** in this scenario.

---

## 🧠 **The Code:**

```java
class Parent {
    Parent() {
        System.out.println("Parent's constructor");
    }
	
    void method1() {
        System.out.println("Parent's method");
    }
	
    void method2() {
        System.out.println("Parent's method");
    }
}

class Child extends Parent {
    Child() {
        System.out.println("Child's constructor");
    }
	
    @Override
    void method1() {
        System.out.println("Child's method");
    }
	
    void method2(int a) {
        System.out.println("Child's overloaded method with int parameter");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent obj = new Child(); // Reference of Parent, Object of Child
        obj.method1(); // ✅ Child's method (Method Overriding)
        obj.method2(); // ✅ Parent's method
        // obj.method2(1); // ❌ Compilation Error
    }
}
```

---

## 1. **Understanding Object Creation:**

```java
Parent obj = new Child();
```

- `Parent obj`: The **reference type** is `Parent`.
- `new Child()`: The **actual object** created is of the **Child class**.

This is an example of **Upcasting**, where a **Child class object** is **referred to** by a **Parent class reference variable**.

---

## 2. **Method Overriding (`method1()`)**

### 📝 **What is Method Overriding?**

- **Overriding** occurs when a **child class** provides a **specific implementation** of a **method** that is **already defined** in its **parent class** with the **same signature**.
- Allows **dynamic polymorphism**, meaning **method binding** occurs at **runtime**.

### 🔍 **What Happens in the Code?**

```java
obj.method1();
```

- The **reference type** is `Parent`, but the **actual object** is of `Child`.
- At **runtime**, the **JVM** uses **dynamic method dispatch** to **determine** which **method to call**.
- It **sees** that the **object** is of the **Child class**, so it **calls the overridden method** in **Child**:

```shell
Child's method
```

### 🎯 **Key Point:**

- In **overriding**, the **method version** called depends on the **actual object type**, not the **reference type**.

---

## 3. **Method Overloading (`method2(int a)`)**

### 📝 **What is Method Overloading?**

- **Overloading** occurs when there are **multiple methods** with the **same name** but **different parameters** (**method signature**).
- It is a form of **compile-time polymorphism**, meaning **method binding** happens at **compile time**.

### 🔍 **What Happens in the Code?**

```java
obj.method2();
```

- The **reference type** is `Parent`, so the **compiler only knows** about the **methods declared** in the `Parent` class.
- The `Parent` class has a **method2()** without parameters, so it **calls this method**:

```shell
Parent's method
```

---

### 🚫 **Why Does `obj.method2(1)` Give an Error?**

```java
obj.method2(1);
```

- The **compiler** only **sees the reference type**, which is **`Parent`**.
- It **looks for `method2(int a)`** in the **`Parent` class**, but **no such method exists**!
- Therefore, the **compiler throws an error** at **compile time**, saying:

```shell
error: method2(int) is not defined in Parent
```

### 🎯 **Key Point:**

- In **overloading**, the **compiler** only **checks methods** available in the **reference type**, not the **actual object type**.

---

## 4. **Compile-Time vs. Runtime Behavior:**

|**Concept**|**Method Overriding**|**Method Overloading**|
|---|---|---|
|**Binding Type**|**Runtime (Dynamic Binding)**|**Compile-Time (Static Binding)**|
|**Resolution Based On**|**Actual Object Type** (`Child`)|**Reference Type** (`Parent`)|
|**Inheritance Required?**|**Yes**|**No**|
|**Method Signature**|Must be **exactly the same**|Must **differ in parameters**|
|**Method Selection**|**Decided at runtime** (JVM)|**Decided at compile time**|
|**Access Rules**|**All overridden methods accessible**|**Only methods in the reference type are accessible**|

---

## 5. **How to Fix This?**

To **access** `method2(int a)` from the **Child class**, you need a **Child class reference type**:

```java
Child obj = new Child();
obj.method2(1); // Output: Child's overloaded method with int parameter
```

Now, the **method2(int a)** is **visible**, and you can **call it** without any **compilation error**.

---

## 🧠 **Summary:**

1. **Method Overriding**: Works at **runtime**, uses **actual object type** (`Child`), which is why `method1()` calls the **Child's method**.
    
2. **Method Overloading**: Works at **compile time**, uses **reference type** (`Parent`), which is why `method2(int a)` is **not accessible**, resulting in a **compilation error**.
    
3. The **compiler only cares** about the **reference type** when **resolving overloaded methods**, while **runtime** uses the **actual object type** for **overridden methods**.
    

---

#DOUBT - `new Child()`: The **actual object** created is of the **Child class**.
but earlier you had told me that objected is created of the class that we pass as reference 



Ah, I see where the confusion might be! Let's clarify this thoroughly:

---

### 🧠 **Understanding Reference Type vs. Object Creation**

When you write:

```java
Parent obj = new Child();
```

- **Reference Type:** `Parent` — This tells the compiler what methods and fields are **accessible** through the `obj` reference.
- **Actual Object Created:** `Child` — This is the **real object** created in **memory** using the `new Child()` constructor.

---

### 🎯 **Key Distinction:**

- The **object** that is **actually created** is always of the **class specified after the `new` keyword**, which in this case is `Child`.
- The **reference type** (`Parent`) only **determines** what **methods and variables** are **accessible** at **compile time**.

---

### 🚦 **What Does This Mean in Practice?**

#### 1. **Object Creation:**

```java
new Child(); 
```

- The **JVM allocates memory** for a **Child object**.
- The **Child's constructor** runs (and also **Parent's constructor** due to **constructor chaining**).
- A **Child object** is **created** in **memory**.

#### 2. **Reference Type Assignment:**

```java
Parent obj = new Child();
```

- The `obj` **reference** is of **type `Parent`**, but it **points to** the **`Child` object**.
- The **compiler** treats `obj` as a **Parent type**, meaning it only **knows about** **Parent's methods** and **fields**.

---

### 🔍 **When You Call Methods:**

#### 🟢 **Method Overriding (`method1`)**

```java
obj.method1(); 
```

- Since **`method1`** is **overridden** in `Child`, the **JVM** will **invoke** the **Child's version** at **runtime**.
- **Output:**

```shell
Child's method
```

#### 🔴 **Method Overloading (`method2(int a)`)**

```java
obj.method2(1);
```

- The **compiler** only **looks at** the **reference type**, which is `Parent`.
- In the **Parent class**, there is **no method** called **`method2(int a)`** (with an **int parameter**).
- Therefore, it **throws a compilation error**, saying:

```shell
error: method2(int) is not defined in Parent
```

---

### 🔥 **Why Does This Happen?**

- **Overloading** is a **compile-time** concept. The **compiler** only **checks methods** in the **reference type** (`Parent`).
- **Overriding** is a **runtime** concept. The **JVM** **looks at the actual object type** (`Child`) to **decide** which **method** to **call**.

---

### ✅ **Correct Approach to Access Overloaded Method:**

If you want to **access** `method2(int a)` from the **Child class**, you need a **Child reference type**:

```java
Child obj = new Child();
obj.method2(1); // Output: Child's overloaded method with int parameter
```

Now, the **method2(int a)** is **visible** and can be **called** without any **compilation error**.

---

### 📦 **Memory Visualization:**

#### **When You Write:**

```java
Parent obj = new Child();
```

- A **Child object** is created in **memory**, but the **reference type** is **Parent**.

```java
          Reference Type: Parent
                 obj
                   |
               +----+
               |    | (Points to a Child Object)
               +----+
                 |
               Child Object in Heap
             +-------------------+
             | method1() (Child) |
             | method2(int)      |
             | method2() (Parent)|
             +-------------------+
```

- When you **call `method1()`**, the **JVM** calls the **method in the Child**.
- When you **try to call `method2(int a)`**, the **compiler** only **sees the Parent reference type** and **throws an error**.

---

### 🧠 **Summary:**

1. The **object created** is **always** of the **class specified** after the `new` keyword (`Child` in this case).
2. The **reference type** (`Parent`) only **controls what is accessible** at **compile time**.
3. **Method Overriding:** Uses **actual object type** at **runtime** (dynamic binding).
4. **Method Overloading:** Uses **reference type** at **compile time** (static binding).
5. If **method signature** is **not found** in the **reference type**, you get a **compilation error**, even if it **exists** in the **actual object type**.

---

### 🧠 **Memory Visualization for:**

```java
Parent obj = new Parent();
```

---

### 🗂 **Memory Layout:**

1. **Stack Memory:**

- Stores **references**, **local variables**, and **method calls**.
- The **`obj` reference** is **stored** here, which **points to** the **Parent object** in **heap memory**.

1. **Heap Memory:**

- Stores **objects** and their **instance variables**.
- The **Parent object** is **created** here using `new Parent()`.
- Contains **methods** and **fields** **specific** to the **Parent class**.

---

### 🔍 **Memory Visualization:**

```java
               +------------------+
               |   Stack Memory   |
               +------------------+
               | obj (Reference)  |
               |      |           |
               |      V           |
               +------------------+
                      |
                      |
                      V
               +------------------+
               |   Heap Memory    |
               +------------------+
               | Parent Object    |
               |------------------|
               | method1()        |
               | method2()        |
               +------------------+
```
---

### 📝 **Explanation:**

1. **Reference Creation:**

```java
Parent obj;
```

- **`obj`** is **declared** as a **reference of type `Parent`**.
- **Memory allocated** in the **stack** but **no object** is **created yet**.
- **Initially**, `obj` **points to null**.

1. **Object Creation:**

```java
obj = new Parent();
```

- **`new Parent()`** **creates** a **new object** in the **heap memory**.
- The **`Parent` constructor** is **called**, and the **object** is **initialized**.
- **Methods** like **`method1()`** and **`method2()`** are **available** in the **Parent object**.

1. **Reference Assignment:**

- The **`obj` reference** now **points** to the **`Parent object`** in the **heap memory**.
- When you **call `obj.method1()`**, the **Parent's method1()** is **executed**.

---

### 🚦 **Key Takeaways:**

- **Object Creation:** The **object** is **always created** in the **heap memory** using the `new` keyword.
- **Reference Handling:** The **reference** is **stored** in the **stack memory** and **points to** the **object** in the **heap**.
- **Method Access:** The **methods available** are **determined** by the **reference type** and **implemented** by the **actual object**.

---

### 🧠 **Memory Visualization for:**

```java
Parent obj = new Child();
```

---

### 📂 **Memory Layout:**

1. **Stack Memory:**

- Stores **references**, **local variables**, and **method calls**.
- The **`obj` reference** is **stored** here, which **points to** the **Child object** in **heap memory**.

1. **Heap Memory:**

- Stores **objects** and their **instance variables**.
- The **Child object** is **created** here using `new Child()`.
- Contains **methods** and **fields** **specific** to both **Parent** and **Child classes**.

---

### 🔍 **Memory Visualization:**

```java
               +------------------+
               |   Stack Memory   |
               +------------------+
               | obj (Reference)  |
               |      |           |
               |      V           |
               +------------------+
                      |
                      |
                      V
               +------------------+
               |   Heap Memory    |
               +------------------+
               |    Child Object  |
               |------------------|
               | method1() (Child)|
               | method2() (Child)|
               | method2() (Parent)|
               | Parent Fields     |
               | Child Fields      |
               +------------------+
```

---

### 📝 **Explanation:**

1. **Reference Creation:**

```java
Parent obj;
```

- **`obj`** is **declared** as a **reference of type `Parent`**.
- **Memory allocated** in the **stack**, but **no object** is **created yet**.
- **Initially**, `obj` **points to null**.

1. **Object Creation:**

```java
obj = new Child();
```

- **`new Child()`** **creates** a **new object** in the **heap memory**.
- The **Child constructor** is **called**, along with the **Parent constructor** due to **constructor chaining**.
- The **Child object** is **initialized** with **methods** and **fields** from **both `Parent`** and **`Child` classes**.

1. **Reference Assignment:**

- The **`obj` reference** now **points** to the **`Child object`** in the **heap memory**.
- When you **call `obj.method1()`**, the **JVM** **dynamically binds** and **calls the Child's `method1()`** because **method1** is **overridden**.

---

### 🚦 **Key Takeaways:**

- The **object created** is of the **class specified** by the **`new` keyword** (`Child`).
- The **reference type** (`Parent`) **controls method accessibility** at **compile time**.
- **Overridden methods** (`method1()`) are **resolved at runtime**, allowing **polymorphic behavior**.
- **Overloaded methods** (`method2(int a)`) are **resolved at compile time**, based on the **reference type**.

---


#DOUBT - **why childs object is created and not of parent's**


Great question! The **reason** why a **Child object** is **created** instead of a **Parent object** in the statement:

```java
Parent obj = new Child();
```
### 🔍 **Breaking It Down:**

1. **The `new` Keyword:**

- The **`new Child()`** **explicitly creates** an **object of the `Child` class**.
- The **`new` keyword** is **responsible** for **allocating memory** and **calling the constructor** of the **specified class** (`Child` in this case).

---

1. **Reference Type vs. Object Type:**

- **`Parent obj`** means that **`obj`** is a **reference variable** of **type `Parent`**.
    
- The **reference type** **controls**:
    
    - What **methods** and **fields** are **accessible** at **compile time**.
    - It **does not control** the **actual object type**.
- The **`new Child()`** part is what **creates** the **object in the heap**.
    
- The **actual object type** is **`Child`**, irrespective of the **reference type** being **`Parent`**.
    

---

1. **Constructor Call Flow:**

When **`new Child()`** is **executed**:

1. The **Child constructor** is **called**.
2. The **first line** of the **Child constructor** **implicitly calls** the **Parent constructor** (`super()` is called automatically).
3. The **Parent constructor** **executes first**, then the **Child constructor** completes.

```java
Parent obj = new Child();
```

**Output:**

```shell
Parent's constructor
Child's constructor
```

This **output** **confirms** that an **object of `Child`** is **created**, not of **`Parent`**.

---

1. **Why Not a `Parent` Object?**

- The **`new` keyword** **always creates** an **object of the specified class**, in this case, **`Child`**.
- If you **wanted** a **Parent object**, you would **need**:

```java
Parent obj = new Parent();
```

- The **reference type** (**`Parent`**) **does not alter** the **object creation process**.

---

1. **Polymorphism in Action:**

- The **reference type** (`Parent`) allows **polymorphism**, where:

```java
obj.method1();
```

- Calls the **overridden method** in the **Child class**, showing **dynamic method dispatch**.

However:

```java
obj.method2(1); // Compilation Error
```

- This **fails** because **overloaded methods** are **resolved at compile time** using the **reference type**, which is **`Parent`**, and **Parent** does **not have `method2(int)`**.

---

### 🚦 **Conclusion:**

- The **`new Child()`** **ensures** the **creation** of a **Child object** in **heap memory**.
- The **reference type** **influences** **method accessibility** but **not the object type**.
- The **constructor call chain** **confirms** the **Child object creation**.

---


#DOUBT  - `new child()` - why creates object of child , its only child constructor ??


Ah, got it! Let's dive deeper into why `Child()` creates an object of the **Child class**, even when the reference type is **Parent**:

---

### 🕵️ **The Key Idea:**

```java
Parent obj = new Child();
```

- The **reference type** is **`Parent`**, but the **object type** is determined by the **constructor** you **use** with the **`new` keyword**.
- **`Child()`** is a **constructor call**, and it **belongs to the `Child` class**, so an **object of `Child`** is **created**.

---

### 🧠 **How Does It Work?**

1. **The `new` Keyword:**

- The **`new` keyword** is **responsible** for **creating objects** in **heap memory**.
- When you **write `new Child()`**, the **JVM**:
    1. **Allocates memory** for a **Child object**.
    2. **Calls the `Child` constructor**.
    3. **Initializes the object** as a **Child instance**, not a **Parent instance**.

---

1. **Constructor Call Flow:**

- When **`new Child()`** is **executed**:

```java
Parent obj = new Child();
```

- The **process** is:

```java
1. Allocate memory for Child object.
2. Call Child's constructor.
3. Call Parent's constructor (via implicit super() call in Child's constructor).
4. Initialize the object as a Child type.
```
- The **constructor call chain**:

```shell
Parent's constructor
Child's constructor
```

---

### 🔍 **Why Not Create a Parent Object Instead?**

- The **constructor** used is **`Child()`**, not **`Parent()`**, so a **Child object** is **created**.
- To **create a Parent object**, you would **need**:

```java
Parent obj = new Parent();
```

- Here, **`new Parent()`** would **create** a **Parent object**, and the **output** would be:

```shell
Parent's constructor
```

---

### 💡 **Analogy:**

- Think of **classes** as **blueprints**, and **constructors** as **instructions** to **build specific models**.
- If the **reference type** is **`Vehicle`**, and you use **`new Car()`**, you **build a Car**, not a **generic Vehicle**.
- The **reference type** **only controls** **how you can use** the **object**, not **what is built**.

---

### 📂 **What If Constructors Did Not Match?**

❌ **Invalid Code:**

```java
Parent obj = new Child(); // ✅ Valid
Parent obj = new Parent(); // ✅ Valid
Child obj = new Parent(); // ❌ Compile-time Error
```

- The **third line** is **invalid** because a **Parent object** **cannot be stored** in a **Child reference**.
- This **violates inheritance rules** (**Parent** is **not** a **Child**).

---

### 🚦 **Key Takeaways:**

- **The `new` keyword** always **creates an object** of the **class specified** by the **constructor**.
- The **reference type** (`Parent`) **controls method accessibility**, but **not the object type**.
- **Method behavior** is **polymorphic** (dynamic method dispatch), but **object creation** is **not**.

---


**NEXT ->** [[12 - Abstraction]]
