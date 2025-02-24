
---


### **Array of Objects in Java**

---


#### **1. What is an Array of Objects?**

- An **array of objects** is a collection of **object references** stored in a contiguous memory location.
- Unlike primitive arrays (e.g., `int[]`, `char[]`), an array of objects holds references to the objects rather than the objects themselves.

---

#### **2. Declaration and Initialization**

**Syntax:**

```java
ClassName[] arrayName = new ClassName[size];
```

**Example:**

```java
// Declare an array to store 5 Student objects
Student[] students = new Student[5];
```

**Key Points:**

- The array `students` can hold references to `Student` objects.
- At the time of declaration, each element of the array is initialized to `null` by default.

---

#### **3. Creating Objects and Assigning to Array Elements**

**Approach 1: Using Index Assignment**

```java
students[0] = new Student("John", 18);
students[1] = new Student("Alice", 19);
```

**Approach 2: Using a Loop**

```java
for (int i = 0; i < students.length; i++) {
    students[i] = new Student("Student" + (i + 1), 18 + i);
}
```

**Explanation:**

- Objects are created using the `new` keyword.
- The array elements store the **references** to these objects.

---

#### **4. Accessing and Using Objects from the Array**

```java
System.out.println(students[0].getName());
students[1].displayInfo();
```

**Key Points:**

- Use array indexing to access specific objects.
- Call methods of the objects using the dot (`.`) operator.

---

#### **5. Example: Complete Code**

```java
class Student {
    String name;
    int age;
	
    // Constructor
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
	
    // Method to display student information
    public void displayInfo() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class ArrayOfObjectsDemo {
    public static void main(String[] args) {
        // Creating an array of Student objects
        Student[] students = new Student[3];
	
        // Initializing the array elements
        students[0] = new Student("John", 18);
        students[1] = new Student("Alice", 19);
        students[2] = new Student("Bob", 20);
	
        // Accessing and displaying student information
        for (Student student : students) {
            student.displayInfo();
        }
    }
}
```

**Output:**

```shell
Name: John, Age: 18  
Name: Alice, Age: 19  
Name: Bob, Age: 20  
```

---

#### **6. Important Concepts**

1. **Null Check:**  
    Before accessing objects, ensure they are not `null` to avoid `NullPointerException`.
    
```java
if (students[0] != null) {
    students[0].displayInfo();
}
```
    
2. **Array Length:**  
    The `length` property can be used to determine the size of the array.
    
```java
System.out.println("Total Students: " + students.length);
```
    
3. **Memory Management:**
    
    - An array of objects consumes memory for the array itself (references) and the objects.
    - Unused references can be set to `null` for garbage collection.
    
```java
students[0] = null; // Eligible for garbage collection if no other reference exists
```
    

---

#### **7. Use Cases of Array of Objects**

- **Storing Multiple Instances:** Useful in scenarios like maintaining a list of employees, students, or products.
- **Data Management:** Easily manage and manipulate collections of objects using loops and array methods.
- **Object-Oriented Design:** Supports the creation of structured, modular code by leveraging object behaviors and properties.

---

#### **8. Common Mistakes to Avoid**

- **Not Initializing Objects:** Leads to `NullPointerException`.
- **Array Index Out of Bounds:** Ensure index is within `0` to `length-1`.
- **Mixing Types:** The array must contain only objects of the specified class type or its subclasses.