In Java, **instance variables** are variables declared inside a class but outside of any method, constructor, or block. They represent the **attributes or properties** of an object, and each instance (object) of the class gets its own copy of these variables.

---

## In Your Example:

```java
class Television {
    private int channel;
    private int volume;
    public void changeChannel() {}
    public void changeVolume() {}
}
```

Here, the instance variables are:

- `channel`
- `volume`

### Key Characteristics of Instance Variables:

1. **Defined Outside Methods**:
    
    - They are declared directly in the class, outside any method, constructor, or block.
    - In this case: `private int channel;` and `private int volume;`
2. **Access Modifiers**:
    
    - Here, they are marked as `private`, which means they can only be accessed within the `Television` class.
    - To access them from outside the class, you would need **getter and setter** methods.
3. **Memory Allocation**:
    
    - Each **object** of the class gets its **own copy** of the instance variables.
    - Example:
        
```java
Television t1 = new Television();
Television t2 = new Television();
```
        
        - `t1` and `t2` each have their own `channel` and `volume`.
        - Changing `channel` on `t1` does not affect `channel` on `t2`.
4. **Default Values**:
    
    - In Java, instance variables are automatically initialized to default values if not explicitly initialized:
        - `int` → `0`
        - `boolean` → `false`
        - `Object references` → `null`
    - In your class:
        - `channel` and `volume` will both be initialized to `0` by default.

---

## Example to Illustrate:

```java
class Television {
    private int channel;  // Default value: 0
    private int volume;   // Default value: 0
	
    public void displaySettings() {
        System.out.println("Channel: " + channel);
        System.out.println("Volume: " + volume);
    }
}

public class Test {
    public static void main(String[] args) {
        Television t1 = new Television();
        Television t2 = new Television();
		
        t1.displaySettings();
        t2.displaySettings();
    }
}
```

### Output:

```shell
Channel: 0
Volume: 0
Channel: 0
Volume: 0
```

### Explanation:

- Even though `channel` and `volume` were not explicitly initialized, they are automatically set to `0` because:
    - In Java, `int` instance variables get a default value of `0`.
- Both `t1` and `t2` have their own `channel` and `volume`, independent of each other.

---

## Summary:

- **Instance variables** are attributes specific to an instance (object) of a class.
- They are initialized with **default values** if not explicitly initialized.
- Each object gets its own copy, allowing each object to have **unique state**.
- They are typically `private` for **encapsulation**, accessed via getter and setter methods.

This is a fundamental concept in **Object-Oriented Programming**, enabling each object to maintain its own state.

[[07 - Instance Methods]]

[07jsjs](google)
