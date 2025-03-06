

---


### **Understanding `Sub s3 = (Sub) new Super();` in Java**

This statement attempts to **cast an instance of the superclass (`Super`) to a subclass (`Sub`)**, but **it is fundamentally incorrect and will cause a runtime error**.

---

### **1️⃣ Breaking It Down Step-by-Step**

```java
class Super {
}

class Sub extends Super {
}

public class Test {
    public static void main(String[] args) {
        Sub s3 = (Sub) new Super(); // ❌ This will cause a runtime error
    }
}
```

### **2️⃣ Why Does This Fail?**

- `Super` is the **parent class** of `Sub`.
- `new Super()` creates an object of `Super`, which does **not** contain any properties or behavior of `Sub`.
- When you **forcefully cast** `Super` to `Sub`, Java will throw a **`ClassCastException`** at runtime.

---

### **3️⃣ What Happens at Runtime?**

🔴 **`ClassCastException`** is thrown because:

- The object is **actually of type `Super`**, not `Sub`.
- Java does **not** allow casting a parent object into a child type unless the object was originally a child type.

---

### **4️⃣ Correct Way: Upcasting & Downcasting**

#### **✅ Upcasting (Safe)**

```java
Super s1 = new Sub();  // Upcasting (Implicit, Always Safe)
```

- Here, a `Sub` object is stored in a `Super` reference.
- **Allowed because a subclass object "is-a" superclass object.**

#### **✅ Downcasting (Requires Type Check)**

```java
Super s2 = new Sub(); // Upcasting
Sub s3 = (Sub) s2;   // ✅ Downcasting (Explicit, Safe)
```

- Since `s2` **actually refers to a `Sub` object**, the downcasting works.

#### **❌ Incorrect Downcasting (Causes Error)**

```java
Super s4 = new Super();
Sub s5 = (Sub) s4; // ❌ ClassCastException!
```

- **Fails** because `s4` is not a `Sub` object.

---

### **5️⃣ Preventing ClassCastException**

✅ **Use `instanceof` to check before downcasting:**

```java
if (s4 instanceof Sub) {
    Sub s5 = (Sub) s4;
} else {
    System.out.println("Downcasting not possible.");
}
```

---

### **6️⃣ Final Summary**

|**Statement**|**Allowed?**|**Why?**|
|---|---|---|
|`Super s = new Sub();`|✅ Yes|**Upcasting** (Implicit, Safe)|
|`Sub s = (Sub) new Super();`|❌ No|**Fails at runtime (`ClassCastException`)**|
|`Super s = new Super(); Sub s2 = (Sub) s;`|❌ No|**Fails (Super object can't be cast to Sub)**|
|`Super s = new Sub(); Sub s2 = (Sub) s;`|✅ Yes|**Downcasting works (because the object was originally a `Sub`)**|

### **Conclusion**

🔴 **Casting `Super` to `Sub` directly is invalid unless the object was originally created as a `Sub` instance.**  
🔵 **Always use `instanceof` to check before downcasting.**



----

# 🚀**Understand in Detail :**


### **Understanding `Sub s3 = (Sub) new Super();` in Detail**

This statement attempts to **cast a superclass (`Super`) object to a subclass (`Sub`)**, but it is **incorrect** and will result in a **runtime error**. Let's analyze this in depth.

---

## **1️⃣ Understanding Class Hierarchy**

Before explaining the issue, let's define a **simple class hierarchy**:

```java
class Super {
    void display() {
        System.out.println("Super class method");
    }
}

class Sub extends Super {
    void show() {
        System.out.println("Sub class method");
    }
}

public class Test {
    public static void main(String[] args) {
        Sub s3 = (Sub) new Super(); // ❌ Runtime Error
    }
}
```

🔴 **What happens?**

- `new Super()` creates an instance of `Super`.
- You are **forcing Java to treat it as a `Sub` object**, which is **incorrect** because the object does not have any `Sub`-specific properties or methods.
- Java will throw a **`ClassCastException` at runtime**.

---

## **2️⃣ Why Does This Cause an Error?**

🔴 Java does **not** allow **"casting up"** an object to a subclass unless it was originally a subclass instance.

### **The Key Problem: Object Type vs. Reference Type**

- The **object type** determines the actual memory structure and available methods.
- The **reference type** only dictates what methods **can** be accessed.

### **What Happens in `Sub s3 = (Sub) new Super();`?**

1. `new Super()` creates a **pure `Super` object**.
2. You **forcefully cast** it to `Sub` (`(Sub) new Super()`).
3. Java allows the **compilation** but throws a **runtime error** (`ClassCastException`) because the object is **not actually a `Sub` instance**.

🚨 **Java does not change the actual type of an object—casting only changes the reference type.**

---

## **3️⃣ What is Allowed? (Correct Upcasting & Downcasting)**

### **✅ Upcasting (Safe)**

Upcasting means assigning a **subclass object to a superclass reference**.

```java
Super s1 = new Sub(); // ✅ Upcasting (Implicit, Always Safe)
s1.display();  // Works because display() is in Super

// s1.show(); ❌ Error: Cannot call Sub's methods using Super reference
```

- **Why is this allowed?**
    - `Sub` **is a** `Super` (Inheritance relationship).
    - `Super` reference can hold a `Sub` object safely.
    - Only `Super` methods can be accessed unless overridden.

---

### **✅ Downcasting (Requires Caution)**

Downcasting is converting a **superclass reference** back to a **subclass reference**.

```java
Super s2 = new Sub(); // ✅ Upcasting
Sub s3 = (Sub) s2;   // ✅ Downcasting (Explicit)
s3.show();  // Works because s2 was originally a Sub object
```

- **Why is this allowed?**
    - `s2` actually holds a `Sub` object, so Java allows the cast.

---

### **❌ Incorrect Downcasting (Causes Error)**

```java
Super s4 = new Super(); // Pure Super object
Sub s5 = (Sub) s4; // ❌ Runtime Error: ClassCastException
```

- **Why does this fail?**
    - `s4` is **not** a `Sub` object.
    - Java does not magically "convert" it into `Sub`.

---

## **4️⃣ How to Prevent ClassCastException?**

✅ **Use `instanceof` before downcasting:**

```java
Super s6 = new Super();

if (s6 instanceof Sub) {
    Sub s7 = (Sub) s6; // Only executes if s6 is actually a Sub object
} else {
    System.out.println("Downcasting not possible.");
}
```

- Prevents **incorrect downcasting** at runtime.

---

## **5️⃣ Real-World Analogy**

Imagine you have a **general Animal class** and a **Dog subclass**:

```java
class Animal { }
class Dog extends Animal { }
```

- ✅ **Upcasting (Safe)**
    
    - A **Dog** is an **Animal**, so this is valid:
        
```java
Animal a = new Dog(); // Upcasting (Safe)
```
        
- ❌ **Downcasting (Unsafe without checking)**
    
    java
    
    CopyEdit
    
    `Animal a = new Animal();   Dog d = (Dog) a; // ❌ Runtime error: ClassCastException`
    
    - **Why?** Because `a` is **not actually a Dog**.

✅ **Fix it with `instanceof`**

java

CopyEdit

`if (a instanceof Dog) {     Dog d = (Dog) a;  // Safe downcasting }`

---

## **6️⃣ Summary: Why `Sub s3 = (Sub) new Super();` Fails**

|**Concept**|**Explanation**|
|---|---|
|**Object Type**|`new Super()` creates an object of class `Super`.|
|**Reference Type**|The reference is being forced into a `Sub` type.|
|**Compilation**|Java allows the syntax, but it doesn’t change the actual object type.|
|**Runtime Behavior**|Java throws `ClassCastException` because a `Super` object cannot become a `Sub`.|
|**Safe Alternative**|Use **Upcasting** (`Super s = new Sub();`) and then **Downcasting** only when sure.|

---

## **Final Thought**

🔴 **Casting `Super` to `Sub` directly is invalid unless the object was originally a `Sub` instance.**  
🔵 **Always use `instanceof` to check before downcasting.**

Would you like a more interactive example to test this in a program? 🚀