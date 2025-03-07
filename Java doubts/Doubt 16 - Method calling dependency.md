
---


In Java, whether a method is called based on the **object** or the **reference type** depends on the type of method being invoked:

1. **Instance Methods (Overridden Methods) → Based on Object (Runtime Polymorphism)**
2. **Static Methods & Method Overloading → Based on Reference Type (Compile-time Polymorphism)**

---

### **1. Overridden Methods (Dynamic Method Dispatch) – Based on Object**

When a method is **overridden** in a subclass, the method that gets called is based on the **actual object type (runtime object)**, even if it is accessed via a reference of the superclass.

#### **Example**

```java
class Parent {
    void show() {
        System.out.println("Parent's show() method");
    }
}

class Child extends Parent {
    @Override
    void show() {
        System.out.println("Child's show() method");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent ref = new Child();  // Reference of Parent, but object of Child
        ref.show();  // Calls Child's show() method (based on object type)
    }
}
```

**Output:**

```sh
Child's show() method
```

👉 **Why?** Because method overriding is resolved at runtime (**Dynamic Binding**), so Java determines the method based on the **actual object (Child)**.

---

### **2. Static Methods & Method Overloading – Based on Reference Type**

#### **(a) Static Methods**

Static methods are resolved at **compile time** based on the reference type. They are not overridden, but rather hidden.

#### **Example**

```java
class Parent {
    static void display() {
        System.out.println("Parent's static method");
    }
}

class Child extends Parent {
    static void display() {
        System.out.println("Child's static method");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent ref = new Child();
        ref.display();  // Calls Parent's display() method (based on reference type)
    }
}
```

**Output:**

rust

Copy code

`Parent's static method`

👉 **Why?** Static methods are **resolved at compile-time** and are called based on **reference type**, not object.

---

#### **(b) Method Overloading**

Overloaded methods are resolved at **compile time**, based on the reference type.

#### **Example**

java

Copy code

`class Demo {     void show(int a) {         System.out.println("Integer method");     }          void show(String s) {         System.out.println("String method");     } }  public class Main {     public static void main(String[] args) {         Demo d = new Demo();         d.show(10);      // Calls show(int)         d.show("Hello"); // Calls show(String)     } }`

👉 **Why?** The compiler decides which method to call based on the method signature at **compile-time**.

---

### **Summary**

|Method Type|Called Based On|Resolution Time|
|---|---|---|
|Overridden Method|**Object Type**|Runtime (Dynamic Binding)|
|Static Method|**Reference Type**|Compile Time (Static Binding)|
|Overloaded Method|**Reference Type**|Compile Time (Static Binding)|