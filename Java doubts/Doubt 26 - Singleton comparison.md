
---

### **Difference between `c1 == c2` and `c1.equals(c2)` in Java**

|**Comparison**|`c1 == c2`|`c1.equals(c2)`|
|---|---|---|
|**Type**|Reference comparison|Content comparison (unless overridden)|
|**Checks**|Whether `c1` and `c2` refer to the **same object** in memory|Whether `c1` and `c2` contain **equal values** (depends on `equals()` method)|
|**Default Behavior (if not overridden)**|True only if both refer to the same memory location|Same as `==` (unless `equals()` is overridden)|
|**Common Use Case**|Used for **Singleton**, checking if two references point to the same object|Used for comparing **values of objects** (e.g., Strings, custom classes)|

---

### **Example: Singleton Check (`==`)**

```java
class CoffeeMachine {
    private static CoffeeMachine cm = null;

    private CoffeeMachine() {}

    public static CoffeeMachine getInstance() {
        if (cm == null) {
            cm = new CoffeeMachine();
        }
        return cm;
    }
}

public class SingletonTest {
    public static void main(String[] args) {
        CoffeeMachine c1 = CoffeeMachine.getInstance();
        CoffeeMachine c2 = CoffeeMachine.getInstance();

        System.out.println("Using == : " + (c1 == c2));       // True (Same object)
        System.out.println("Using equals(): " + c1.equals(c2)); // True (Same reference)
    }
}

```

✅ Output:

sql

Copy code

`Using == : true Using equals(): true`

- Since `c1` and `c2` refer to the **same instance**, both `==` and `equals()` return `true`.

---

### **Example: When `equals()` is Different from `==`**

If `equals()` is overridden, it compares object **values**, not references.

java

Copy code

`class Car {     String model;      Car(String model) {         this.model = model;     }      @Override     public boolean equals(Object obj) {         if (this == obj) return true; // Check if both references are same         if (obj == null || getClass() != obj.getClass()) return false;         Car car = (Car) obj;         return this.model.equals(car.model); // Compare values     } }  public class CarTest {     public static void main(String[] args) {         Car car1 = new Car("Tesla");         Car car2 = new Car("Tesla");          System.out.println("Using == : " + (car1 == car2));       // False (Different objects)         System.out.println("Using equals(): " + car1.equals(car2)); // True (Same model)     } }`

✅ Output:

sql

Copy code

`Using == : false Using equals(): true`

- `==` is `false` because `car1` and `car2` are **different objects**.
- `equals()` is `true` because **both have the same model**.

---

### **Final Notes**

✔ Use **`==`** to check **if two references point to the same object** (used in Singleton).  
✔ Use **`equals()`** to check **if two objects are logically equal** (like String, custom objects).  
✔ Override `equals()` in custom classes for **meaningful comparisons**.