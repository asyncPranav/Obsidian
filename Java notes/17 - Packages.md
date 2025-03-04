
---

# **Java Packages: A Complete Guide (Basic to Advanced)**

## **📌 Introduction to Packages**

A **package** in Java is a **group of related classes and interfaces** that help organize code, prevent name conflicts, and improve maintainability.  
Packages work like folders in a file system, helping to **categorize** different classes based on their functionality.

---

## **1️⃣ What is a Package in Java?**

A **package** is a namespace that organizes classes, interfaces, and sub-packages logically.
    **OR**
A **package** is a namespace that groups related classes and interfaces together. It works like a folder structure in an operating system.

### **🔹 Why Use Packages?**

✔ **Organizes Code** – Keeps classes and interfaces categorized.  
✔ **Avoids Name Conflicts** – Prevents clashes between classes with the same name in different packages.  
✔ **Access Control** – Restricts access using **private, protected, and public** modifiers.  
✔ **Code Reusability** – Encourages modular development.  
✔ **Better Maintenance** – Makes large projects easier to manage.

---

## **2️⃣ Types of Packages in Java**

There are **two types** of packages in Java:

1. **Built-in Packages** – Predefined Java packages like `java.util`, `java.io`, etc.
2. **User-defined Packages** – Custom packages created by developers.

---

## **3️⃣ Built-in Java Packages (Predefined)**

Java provides several built-in packages for different functionalities:

|Package Name|Purpose|
|---|---|
|`java.lang`|Provides fundamental classes like `Math`, `String`, `Object`, etc. (Imported by default)|
|`java.util`|Contains utility classes like `ArrayList`, `HashMap`, `Collections`, etc.|
|`java.io`|Supports input/output operations (e.g., `FileReader`, `BufferedReader`)|
|`java.net`|Provides networking functionalities (e.g., `Socket`, `URL`)|
|`java.sql`|Contains JDBC classes for database connectivity|
|`javax.swing`|Provides GUI components (e.g., `JFrame`, `JButton`)|

✔ You can use built-in packages using the `import` statement:

```java
import java.util.Scanner;  // Importing Scanner class
```

---

## **4️⃣ User-Defined Packages (Creating Custom Packages)**

A user-defined package is created using the `package` keyword.

### **🔹 Creating a Package**

1. Define a package using the `package` keyword.
2. Save the file inside a directory matching the package name.

```java
package mypackage; // Declaring a package

public class Hello {
    public void greet() {
        System.out.println("Hello from mypackage!");
    }
}
```

✔ **Package declaration must be the first statement in the file (except comments).**

---

## **5️⃣ Compiling and Using Packages**

To **compile** a package:

```shell
javac -d . Hello.java
```

✔ `-d .` stores the compiled `.class` file in the package folder (`mypackage/Hello.class`).

### **🔹 Using a User-Defined Package in Another Class**

To use a package in another file, you must **import** it.

```java
import mypackage.Hello; // Importing the class

public class Main {
    public static void main(String[] args) {
        Hello obj = new Hello();
        obj.greet();  // Output: Hello from mypackage!
    }
}
```

✔ `import mypackage.Hello;` allows using the `Hello` class from `mypackage`.

---

## **6️⃣ Sub-Packages in Java**

A **sub-package** is a package inside another package. It helps in **better code organization**.

### **🔹 Example of a Sub-Package**


```java
package mypackage.subpackage;

public class SubClass {
    public void display() {
        System.out.println("Inside Sub-Package!");
    }
}
```


✔ **Using the Sub-Package Class**


```java
import mypackage.subpackage.SubClass;

public class Test {
    public static void main(String[] args) {
        SubClass obj = new SubClass();
        obj.display();  // Output: Inside Sub-Package!
    }
}
```


✔ **A package can contain multiple sub-packages.**

---

## **7️⃣ Access Modifiers and Packages**

Access modifiers define how a class or method can be accessed across packages.

|Modifier|Same Class|Same Package|Subclass (Different Package)|Outside Package|
|---|---|---|---|---|
|`public`|✅|✅|✅|✅|
|`protected`|✅|✅|✅|❌|
|**Default** (no modifier)|✅|✅|❌|❌|
|`private`|✅|❌|❌|❌|

✔ **Public** → Accessible everywhere.  
✔ **Protected** → Accessible within the same package + subclasses.  
✔ **Default** → Accessible only within the same package.  
✔ **Private** → Accessible only inside the same class.

---

## **8️⃣ Importing Packages in Java**

There are **three ways** to import packages:

### **1️. Import a Specific Class**

```java
import java.util.Scanner;  // Only Scanner class is imported
```

### **2️. Import All Classes in a Package**

```java
import java.util.*;  // Imports all classes from java.util package
```

### **3️. Fully Qualified Name (Without Import)**

```java
public class Main {
    public static void main(String[] args) {
        java.util.Scanner sc = new java.util.Scanner(System.in);
    }
}
```

✔ **No `import` required**, but writing full package names repeatedly can be tedious.

---

## **9️⃣ Static Import (Java 5 Feature)**

**Static import** allows importing static members (variables & methods) directly.

### **🔹 Without Static Import**

```java
import java.lang.Math;

public class Main {
    public static void main(String[] args) {
        System.out.println(Math.sqrt(16));  // Output: 4.0
    }
}
```

### **🔹 With Static Import**

```java
import static java.lang.Math.sqrt;

public class Main {
    public static void main(String[] args) {
        System.out.println(sqrt(16));  // Output: 4.0
    }
}
```

✔ **Avoids prefixing `Math.` every time.**  
✔ **Use carefully to avoid ambiguity.**

---

## **🔟 Difference Between Packages and Modules (Java 9)**

|Feature|Package|Module|
|---|---|---|
|Purpose|Groups related classes|Groups related packages|
|Accessibility|Uses `import`|Uses `requires` in `module-info.java`|
|Encapsulation|Only classes are encapsulated|Entire packages can be encapsulated|

---

## **1️⃣1️⃣ Package Naming Conventions**

✔ Package names should be **lowercase**.  
✔ Use **reverse domain name** for uniqueness.

### **Example:**

For a company **ABC Technologies** with domain **abctech.com**, package names should be:

```java
package com.abctech.projectname;
```

---

## **1️⃣2️⃣ Summary**

✔ **Packages** group related classes and interfaces.  
✔ Java has **built-in** (`java.util`, `java.io`) and **user-defined packages**.  
✔ Use `import` to access classes from another package.  
✔ **Sub-packages** help with better code organization.  
✔ **Access Modifiers** control visibility across packages.  
✔ **Static import** allows importing static members directly.  
✔ **Package vs Module** – Packages organize classes, while modules organize packages.

