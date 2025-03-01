
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