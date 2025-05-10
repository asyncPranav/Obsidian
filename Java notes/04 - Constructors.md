

A **constructor** in Java is a special method that is called when an object is created. It is used to initialize the object. Unlike regular methods, constructors have the same name as the class and do not have a return type (not even `void`).

---

## **1. Purpose of Constructors**

- Initialize objects and assign initial values to instance variables.
- Allocate resources or perform setup operations for the object.
- Ensure the object is in a consistent state when created.

---

## **2. Characteristics of Constructors**

- Must have the same name as the class.
- Do not have a return type (not even `void`).
- Are called automatically when an object is created using the `new` keyword.
- Can be overloaded to create multiple constructors with different parameters.
- If no constructor is explicitly defined, Java provides a **default constructor**.

---

## **3. Types of Constructors**

### 3.1. **Default Constructor**

- A constructor with no parameters.
- If no constructor is defined, Java provides a default constructor.
- Initializes instance variables to default values (`0`, `null`, `false`, etc.).

**Example:**

```java
class Student {
    String name;
    int age;

    // Default Constructor
    Student() {
        name = "Unknown";
        age = 0;
    }

    void display() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class Main {
    public static void main(String[] args) {
        Student student = new Student(); // Default constructor is called
        student.display();
    }
}
```

**Output:**

```shell
Name: Unknown, Age: 0
```

### 3.2. **Parameterized Constructor**

- Accepts arguments to initialize instance variables with specific values.
- Allows creating objects with custom initial values.

**Example:**

```java
class Student {
    String name;
    int age;

    // Parameterized Constructor
    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    void display() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class Main {
    public static void main(String[] args) {
        Student student1 = new Student("John", 20); // Parameterized constructor is called
        Student student2 = new Student("Alice", 22);
        
        student1.display();
        student2.display();
    }
}
```

**Output:**

```shell
Name: John, Age: 20
Name: Alice, Age: 22
```

### 3.3. **No-Argument Constructor**

- A constructor with no parameters but explicitly defined by the programmer.
- Often used to provide default initialization.

**Example:**

```java
class Car {
    String model;
    int year;

    // No-Argument Constructor
    Car() {
        model = "Unknown";
        year = 0;
    }

    void display() {
        System.out.println("Model: " + model + ", Year: " + year);
    }
}

public class Main {
    public static void main(String[] args) {
        Car car = new Car(); // No-argument constructor is called
        car.display();
    }
}
```

**Output:**

```shell
Model: Unknown, Year: 0
```

---

## **4. Constructor Overloading**

- Defining multiple constructors with different parameters in the same class.
- Differentiated by the number, type, and order of parameters.

**Example:**

```java
class Employee {
    String name;
    int age;
    double salary;

    // No-Argument Constructor
    Employee() {
        name = "Unknown";
        age = 0;
        salary = 0.0;
    }

    // Parameterized Constructor
    Employee(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Another Parameterized Constructor
    Employee(String name, int age, double salary) {
        this.name = name;
        this.age = age;
        this.salary = salary;
    }

    void display() {
        System.out.println("Name: " + name + ", Age: " + age + ", Salary: $" + salary);
    }
}

public class Main {
    public static void main(String[] args) {
        Employee emp1 = new Employee();
        Employee emp2 = new Employee("John", 30);
        Employee emp3 = new Employee("Alice", 28, 50000);

        emp1.display();
        emp2.display();
        emp3.display();
    }
}
```

**Output:**

```shell
Name: Unknown, Age: 0, Salary: $0.0
Name: John, Age: 30, Salary: $0.0
Name: Alice, Age: 28, Salary: $50000.0
```


### ✅ Focus: Differentiation by **Order of Parameters**

Even if two constructors have the **same number** and **types** of parameters, you can differentiate them by the **order** in which those parameter types appear.

```java
public class Person {
    String name;
    int age;
	
    // Constructor 1: String first, then int
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
	
    // Constructor 2: int first, then String
    public Person(int age, String name) {
        this.name = name;
        this.age = age;
    }
	
    public void show() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class Main {
    public static void main(String[] args) {
        Person p1 = new Person("Alice", 25);  // calls Constructor 1
        Person p2 = new Person(30, "Bob");    // calls Constructor 2
		
        p1.show();
        p2.show();
    }
}

```


---

## **5. `this()` Constructor Call**

- Used to call another constructor in the same class.
- Helps in constructor chaining to reduce code duplication.
- Must be the first statement in the constructor.

**Example:**

```java
class Person {
    String name;
    int age;

    // Constructor with no parameters
    Person() {
        this("Unknown", 0); // Calling another constructor
    }

    // Constructor with parameters
    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    void display() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class Main {
    public static void main(String[] args) {
        Person person1 = new Person();
        Person person2 = new Person("John", 25);

        person1.display();
        person2.display();
    }
}
```

**Output:**

```shell
Name: Unknown, Age: 0
Name: John, Age: 25
```

---

## **6. `super()` Constructor Call**

- Used to call the parent class constructor.
- Must be the first statement in the subclass constructor.
- If not explicitly called, the compiler automatically invokes the parent class's no-argument constructor.

**Example:**

```java
class Animal {
    Animal() {
        System.out.println("Animal Constructor");
    }
}

class Dog extends Animal {
    Dog() {
        super(); // Calling parent class constructor
        System.out.println("Dog Constructor");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
    }
}
```

**Output:**

```shell
Animal Constructor
Dog Constructor
```

---

## **7. Key Points to Remember**

- Constructors do not have a return type.
- Constructors are automatically called when an object is created.
- If no constructor is defined, the compiler provides a default constructor.
- Constructors can be overloaded.
- `this()` is used to call another constructor in the same class.
- `super()` is used to call a parent class constructor.
- Constructors cannot be inherited but can be called using `super()` in a subclass.
- `static`, `final`, and `abstract` keywords cannot be used with constructors.

---

## **8. Why Use Constructors?**

- To initialize objects and provide default or custom values to instance variables.
- To allocate resources or perform startup operations.
- To maintain encapsulation by restricting direct access to instance variables.

---

## **9. Differences Between Constructor and Method**

|Constructor|Method|
|---|---|
|Same name as the class|Can have any name|
|No return type|Must have a return type (including `void`)|
|Called automatically when an object is created|Called explicitly using the object|
|Used for object initialization|Used for performing actions or calculations|
|Cannot be `static`, `final`, or `abstract`|Can be `static`, `final`, or `abstract`|

---

This covers all the essential points about constructors in Java. If you need more examples or have any questions, let me know!




---



**NEXT ->** [[05 - this Keyword]]

