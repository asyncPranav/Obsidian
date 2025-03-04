
---

# **Java Packages: A Complete Guide (Basic to Advanced)**

## **📌 Introduction to Packages**

A **package** in Java is a **group of related classes and interfaces** that help organize code, prevent name conflicts, and improve maintainability.  
Packages work like folders in a file system, helping to **categorize** different classes based on their functionality.

---

## **1️⃣ What is a Package in Java?**

A **package** is a namespace that organizes classes, interfaces, and sub-packages logically.
		
	OR
		
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

✔ `-d .` stores the compiled `.class` file in the package folder (`mypackage/Hello.class`)

The `-d` option in `javac -d . Hello.java` specifies the **destination directory** where the compiled `.class` files should be placed.

#### **Explanation:**

- `javac` → Java compiler command.
- `-d .` → `-d` (destination) followed by `.` (current directory).
- `Hello.java` → The Java source file to be compiled.

### **How it Works:**

1. When you compile a Java file that belongs to a package, the `-d` option ensures that the package directory structure is created automatically.
2. The compiled `.class` file is placed inside the corresponding package folder.

### **Example:**

#### **Source File (Hello.java)**

```java
package mypackage;

public class Hello {
    public void greet() {
        System.out.println("Hello from mypackage!");
    }
}
```

#### **Compiling with `-d .`**

```shell
javac -d . Hello.java
```

- The compiler creates a **directory named `mypackage`** in the **current directory (`.`)**.
- Inside `mypackage`, the compiled `Hello.class` file is stored:
    
```shell
./mypackage/Hello.class
```
    

#### **Without `-d`**

```shell
javac Hello.java
```

- The `.class` file will be generated in the same directory as `Hello.java`, **without any package structure**.

### **Alternative: Specifying a Custom Directory**

```shell
javac -d /home/user/output Hello.java
```

- This will place `Hello.class` inside `/home/user/output/mypackage/`.

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


---


# **Extra-Content**

## **1️⃣ File compilation**

If you compile a **normal Java file** (i.e., a file **without a `package` declaration**) using the same command:

```shell
javac -d . Hello.java
```

### **What Happens?**

- Since the Java file **does not** belong to any package, the `-d` option has **no effect** on the output location.
- The compiled `.class` file (`Hello.class`) will still be placed in the **current directory (`.`)**, just like running `javac Hello.java`.

### **Example: Normal Java File (No Package)**

#### **Hello.java**

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

#### **Compiling with `-d .`**

```sell
javac -d . Hello.java
```

#### **Output:**

```shell
Hello.class (Generated in the same directory as Hello.java)
```

✔ **Since there's no package, `-d` has no visible effect.**  
✔ **It behaves the same as `javac Hello.java`**

---

### **When Does `-d` Matter?**

- It only affects **Java files that have a `package` declaration**.
- If a package exists, `-d` **ensures the correct package directory structure** is created.

---

### **Summary**

|Scenario|Command|Output Location|
|---|---|---|
|**With Package (`package mypackage;`)**|`javac -d . Hello.java`|`./mypackage/Hello.class`|
|**Without Package**|`javac -d . Hello.java`|`./Hello.class` (Same as `javac Hello.java`)|

🚀 **Key Takeaway:** If your file **does not** use a package, `-d` won't change anything!

---


```shell
javac -d .. Hello.java
```

### **Explanation:**

- `javac` → Java compiler command.
- `-d ..` → `-d` specifies the **destination directory**, and `..` means **the parent directory**.
- `Hello.java` → Java source file to compile.

---

### **What Happens When You Run This?**

#### **1️⃣ If `Hello.java` Contains No Package**

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

📌 **Output:** `Hello.class` will be placed in **the parent directory (`..`)**, because no package structure is involved.

---

#### **2️⃣ If `Hello.java` Contains a Package**

```java
package mypackage;

public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello from mypackage!");
    }
}
```

📌 **Output:** The `mypackage/Hello.class` file will be generated in the **parent directory (`..`)** like this:

```shell
../mypackage/Hello.class
```

✔ **The compiler automatically creates the `mypackage/` folder in the parent directory.**

---

### **Incorrect Syntax & Fixes**

|❌ Incorrect|✅ Correct|
|---|---|
|`javac -d.. Hello.java`|`javac -d .. Hello.java`|
|`javac -d..Hello.java`|`javac -d .. Hello.java`|
|`javac -d. Hello.java`|`javac -d . Hello.java`|

🚀 **Key Takeaway:**

- Always put a **space after `-d`** before specifying the destination directory!





