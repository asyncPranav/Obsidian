
```java
class Parentt {
    public Parentt() {
        System.out.println("non param of Parent");
    }
    
    public Parentt(int x) {
        System.out.println("param of Parent");
    }
}

class Childd extends Parentt {
    public Childd(int x) {
        System.out.println("param of Child");
    }
}

public class _03_InheritParameterizedConstructor {
    public static void main(String[] args) {
        Childd c2 = new Childd(3);
    }
}
```


Your expectation:

```shell
param of Parent
param of Child
```

But the actual output:

```shell
non param of Parent
param of Child
```

### **Understanding Constructor Execution in Inheritance**

When an object of `Childd` is created (`new Childd(3);`), the constructor of `Childd` is called. Since `Childd` extends `Parentt`, **Java ensures the parent constructor is called first** before executing the child constructor.

---

### **Breaking Down Execution:**

```java
Childd c2 = new Childd(3);
```

- The **constructor `Childd(int x)`** is invoked.
- Since there is **no explicit call to `super(x);`**, Java **implicitly calls the no-argument constructor of `Parentt` (`super();`)**.
- **Execution Order:**
    1. **`Parentt()`** (no-parameter constructor) → Prints `"non param of Parent"`
    2. **`Childd(int x)`** executes → Prints `"param of Child"`


---


### **Detailed Explanation of Constructor Execution in Java Inheritance**


1. **When you create an object of a child class, Java first calls the constructor of the parent class** before executing the child class's constructor.
2. **If you do not explicitly call a parent constructor using `super(arguments)`, Java implicitly calls the default (no-argument) constructor of the parent class.**
3. **Java follows a strict constructor execution order:**
    - The parent class constructor runs first.
    - Then, the child class constructor executes.

---

### **Step-by-Step Execution of Your Code**

#### **Given Code:**

```java
```

---

### **What Happens When `new Childd(3);` is Executed?**

#### **Step 1: The `Childd(int x)` constructor is called**

- You created an object of `Childd` with an integer argument:
    
    java
    
    CopyEdit
    
    `Childd c2 = new Childd(3);`
    
- This means the following constructor is executed:
    
    java
    
    CopyEdit
    
    `public Childd(int x) {     System.out.println("param of Child"); }`
    
- **BUT before executing this, Java must call the constructor of `Parentt` (the parent class).**

---

#### **Step 2: Java Checks for an Explicit `super()` Call**

- In `Childd(int x)`, you **did not explicitly call `super(x);`**.
- When there is **no explicit `super(...)` call, Java automatically inserts a call to `super();` (the no-argument constructor of `Parentt`).**
- **So, Java executes:**
    
    java
    
    CopyEdit
    
    `public Parentt() {     System.out.println("non param of Parent"); }`
    
- This prints:
    
    wasm
    
    CopyEdit
    
    `non param of Parent`
    

---

#### **Step 3: The `Childd(int x)` Constructor Executes**

- Now that the parent class constructor is done executing, Java continues with the `Childd(int x)` constructor.
- It prints:
    
    wasm
    
    CopyEdit
    
    `param of Child`
    

---

### **Final Output:**

wasm

CopyEdit

`non param of Parent param of Child`

---

### **Why Not `param of Parent`?**

You were expecting:

wasm

CopyEdit

`param of Parent param of Child`

**This would have happened if the `Childd(int x)` constructor explicitly called `super(x);`.**  
To fix this, modify the `Childd(int x)` constructor like this:

java

CopyEdit

`class Childd extends Parentt {     public Childd(int x) {         super(x);  // Explicitly call parameterized constructor of Parentt         System.out.println("param of Child");     } }`

### **Now, the Execution Flow:**

1. `Childd(int x)` is called.
2. `super(x);` is executed → Calls `Parentt(int x)`, which prints:
    
    wasm
    
    CopyEdit
    
    `param of Parent`
    
3. After `Parentt(int x)` completes, `Childd(int x)` continues and prints:
    
    wasm
    
    CopyEdit
    
    `param of Child`
    

### **Now, the Corrected Output Will Be:**

wasm

CopyEdit

`param of Parent param of Child`

---

### **Key Takeaways**

1. **Java always calls a parent constructor before executing a child constructor.**
2. **If a child constructor does not explicitly call `super(...)`, Java inserts `super();` by default.**
3. **To call a specific constructor in the parent class, use `super(arguments);` explicitly.**
4. **The unexpected output happened because Java called the no-argument constructor (`Parentt()`), not `Parentt(int x)`.**