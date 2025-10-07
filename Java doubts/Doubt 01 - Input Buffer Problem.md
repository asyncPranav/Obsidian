```java
package com.pranav;  
import java.util.Scanner;  
  
public class Input {  
    public static void main(String[] args) {  
        Scanner input = new Scanner(System.in);  
		
		System.out.print("enter your roll no : ");  
        int rollno = input.nextInt();  
        System.out.println("Your roll no is " + rollno);
		
		System.out.print("enter your name : ");  
        String name = input.nextLine();  
        System.out.println("Name = " + name);  
    }
}
```

Your Java program is designed to take user input for a roll number and a name. However, there is an issue with the way the `Scanner` object handles the inputs, which could cause unexpected behavior. Let me explain:

### **Problem in the Code:**

When you call `input.nextInt()` to read the roll number, it consumes only the integer value but leaves the newline character (`\n`) in the input buffer. When you subsequently call `input.nextLine()` to read the name, it immediately consumes this leftover newline character instead of waiting for the user to input a name.

### **Solution:**

To fix this, you need to consume the leftover newline character after calling `nextInt()` before you call `nextLine()`.

```java
package com.pranav;
import java.util.Scanner;

public class Input {
    public static void main(String[] args) {
        System.out.print("Enter your roll no: ");
        Scanner input = new Scanner(System.in);
		
        int rollno = input.nextInt();
        input.nextLine(); // Consume the leftover newline character
		
        System.out.print("Enter your name: ");
        String name = input.nextLine();
        
        System.out.println("Your roll no is " + rollno);
        System.out.println("Name = " + name);
    }
}
```

### **Explanation of Fix:**

1. After `input.nextInt()`, add `input.nextLine()` to consume the newline character.
2. This ensures that the subsequent `input.nextLine()` reads the actual name input from the user.

### **Example Input and Output:**

#### Input:

```bash
Enter your roll no: 123
Enter your name: Pranav
```

#### Output:

```bash
Your roll no is 123
Name = Pranav
```

This change ensures the program works as intended.

---


## 🧠 Step 1: How `Scanner` reads input

Each `Scanner` method reads **only part** of the input line — not always the whole thing.

For example:

- `nextInt()`, `nextDouble()`, `nextFloat()` etc.  
    → read only the **number** and stop **right before the newline (`\n`)**.  
    So the newline remains in the input buffer.
    
- `next()`  
    → reads a **single word** (up to space).  
    Leaves the rest (including space/newline).
    
- `nextLine()`  
    → reads the **entire line**, including spaces, and **consumes the newline** at the end.
    

---

## ⚠️ Step 2: When newline `\n` remains

A newline remains when you use **any method other than `nextLine()`**, and then later try to use `nextLine()`.

Example 👇

```java
int n = sc.nextInt();  // user enters: 10\n
// "10" is read, but '\n' remains
String name = sc.nextLine(); // reads that leftover '\n'
```

So `name` becomes an empty string (`""`).

---

## ✅ Step 3: Fix → Clear the newline

You fix it by calling an extra `nextLine()` right after reading the number:

```java
int n = sc.nextInt();
sc.nextLine(); // consume the leftover newline
String name = sc.nextLine(); // now reads the actual input
```

---

## 🧩 Step 4: Quick Rule of Thumb

|What you used last|Does newline remain?|Need `sc.nextLine()` fix?|
|---|---|---|
|`nextInt()`|✅ Yes|✅ Yes|
|`nextDouble()`|✅ Yes|✅ Yes|
|`next()`|✅ Yes|✅ Yes (if using nextLine next)|
|`nextLine()`|❌ No|❌ No|

---

## 🧠 Step 5: Tip for detection (experience-based)

You’ll know newline is left if:

- The program **“skips” an input** unexpectedly (like name not taken after number input).
    
- Or when you expect a string but it reads **empty string ("")**.
    

That’s the classic sign that a newline was still sitting in the buffer.