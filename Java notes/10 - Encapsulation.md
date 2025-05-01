
---

## **1. What is Encapsulation?**

Encapsulation is one of the four fundamental principles of **Object-Oriented Programming (OOP)**. It is the process of **wrapping data (variables) and methods (functions) together** in a **single unit (class)** and **restricting direct access** to some details of the object. It helps protect the integrity of the object by allowing only controlled access through public methods.

🔹 In simpler terms, **encapsulation means data hiding and protecting the integrity of an object** by restricting direct access to its internal state.

---

## **2. Key Features of Encapsulation**

1. **Data Hiding:**
    - The internal details of a class are **hidden** from the outside world.
    - Only controlled access is provided using **getter and setter methods**.
2. **Data Security:**
    - Prevents **direct modification** of data from outside the class.
    - Reduces the risk of **accidental data corruption**.
3. **Controlled Access:**
    - Access to data is **restricted** using **private** access modifiers.
    - Specific methods (**getters & setters**) allow controlled access.
4. **Improves Maintainability:**
    - Code is **modular and easier to maintain**.
    - Changes in the class’s internal implementation do **not affect other parts** of the code.
5. **Enhances Flexibility & Reusability:**
    - The internal logic of a class can be changed without affecting other classes.
    - The same class can be **reused** across multiple programs.

---

## **3. How Encapsulation Works in Java?**

Encapsulation in Java is implemented using **four access modifiers**:

1. `private` → Data is accessible **only within the same class**.
2. `default` (no modifier) → Data is accessible **within the same package**.
3. `protected` → Data is accessible **within the same package and subclasses**.
4. `public` → Data is accessible **from anywhere**.

💡 **Encapsulation is best implemented using:**

- **Private variables (fields)**
- **Public getter and setter methods** to access and modify the private fields.

---

## **4. Encapsulation Example**

Let’s see an example of encapsulation in Java:

### **Example 1: Basic Encapsulation**

```java
class BankAccount {
    private double balance;  // Private variable (Data Hiding)
	
    // Constructor
    public BankAccount(double initialBalance) {
        if (initialBalance > 0) {
            balance = initialBalance;
        } else {
            balance = 0;
        }
    }
	
    // Getter method to access private balance
    public double getBalance() {
        return balance;
    }
	
    // Setter method to modify balance safely
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposited: " + amount);
        } else {
            System.out.println("Invalid deposit amount!");
        }
    }
	
    // Withdraw method with validation
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Withdrawn: " + amount);
        } else {
            System.out.println("Invalid withdrawal amount!");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount(1000); // Creating account with initial balance
        System.out.println("Initial Balance: " + account.getBalance());
		
        account.deposit(500);
        System.out.println("Updated Balance: " + account.getBalance());
		
        account.withdraw(300);
        System.out.println("Final Balance: " + account.getBalance());
    }
}

```

### **Explanation:**

1. `balance` is **private**, meaning it **cannot be accessed directly**.
2. `getBalance()` is a **getter** method to **view the balance**.
3. `deposit()` and `withdraw()` are **setter methods** to **modify the balance safely**.

🔹 **Without encapsulation**, we could directly modify `balance`, leading to **inconsistent or incorrect values**.

---

## **5. Advantages of Encapsulation**

### ✅ **1. Data Hiding & Security**

- Restricts direct access to critical data.
- Prevents accidental modifications or misuse.

### ✅ **2. Controlled Access to Data**

- Only **authorized methods** can modify the data.
- Allows **data validation** before modification.

### ✅ **3. Code Reusability & Maintainability**

- Changes in internal implementation **do not affect other classes**.
- Well-structured and modular code **improves debugging**.

### ✅ **4. Flexibility for Future Enhancements**

- Internal implementation can change **without affecting outside code**.
- Allows better **code optimization**.

---

## **6. Real-World Example of Encapsulation**

Encapsulation is widely used in real-world applications. Let’s see some real-world analogies and examples.

### **📌 Example 1: ATM Machine**

🔹 An **ATM machine** is a perfect example of encapsulation:

- **Your bank balance is private** (hidden from direct access).
- You access it **via getter methods** (Check Balance).
- You modify it using **setter methods** (Deposit & Withdraw).
- **Validation** ensures you can’t withdraw more than you have.

```java
class ATM {
    private double balance = 5000;  // Private balance
	
    public double checkBalance() {
        return balance;
    }
	
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposit successful! New balance: " + balance);
        } else {
            System.out.println("Invalid deposit amount.");
        }
    }
	
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Withdrawal successful! Remaining balance: " + balance);
        } else {
            System.out.println("Invalid withdrawal amount or insufficient funds.");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        ATM myAccount = new ATM();
        System.out.println("Balance: " + myAccount.checkBalance());
        myAccount.deposit(1000);
        myAccount.withdraw(2000);
    }
}
```

✅ **Encapsulation ensures the balance is safe and only modified through controlled methods.**

---

## **7. Common Mistakes While Implementing Encapsulation**

❌ **1. Making instance variables public**

```java
class WrongExample {
    public int age;  // ❌ Direct access to data
}
```

✅ **Fix:** Use private variables and provide controlled access.

```java
class CorrectExample {
    private int age;  // ✅ Private variable
	
    public int getAge() {
        return age;
    }
	
    public void setAge(int age) {
        if (age > 0) { // ✅ Validation
            this.age = age;
        }
    }
}
```

❌ **2. Allowing modifications without validation**

```java
class Account {
    private double balance;
	
    public void setBalance(double balance) {
        this.balance = balance;  // ❌ No validation
    }
}
```

✅ **Fix:** Always validate inputs.

```java
public void setBalance(double balance) {
    if (balance >= 0) {  // ✅ Validation
        this.balance = balance;
    }
}
```

---

## **8. Difference Between Encapsulation and Other OOP Concepts**

|Feature|Encapsulation|Abstraction|Inheritance|Polymorphism|
|---|---|---|---|---|
|**Definition**|Wrapping data and methods together|Hiding implementation details|Acquiring properties of another class|Multiple methods with the same name but different behavior|
|**Focus**|Data protection and controlled access|Hiding unnecessary details|Code reusability|Dynamic method behavior|
|**How?**|Using **private** fields and **getter/setter** methods|Using **abstract classes** and **interfaces**|Using `extends` and `implements`|Method Overloading & Overriding|

---

## **Conclusion**

🔹 **Encapsulation is a key concept in Java** that ensures data security, modularity, and maintainability.  
🔹 It **hides the implementation details** and **provides controlled access** using **getters & setters**.  
🔹 It is widely used in **real-world applications like ATM, Banking Systems, and Secure APIs**.