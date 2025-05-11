
---

```java
package $19_MultiThreading;  
  
class JoinMeth extends Thread{  
    @Override  
    public void run() {  
        for(int i=0; i<=100; i++){  
            System.out.println(i);  
        }  
    }  
}  
public class _06_JoinMethod {  
    public static void main(String[] args) {  
        JoinMeth t = new JoinMeth();  
        t.setDaemon(true); //set daemon true -> main thread will not wait  
        t.start();  
		  
        Thread mainThread =  Thread.currentThread();  
        try {  
            mainThread.join();  
        } catch (Exception e){  
            System.out.println(e);  
        }  
		  
    }  
}
```


This program appears to **misuse the `join()` method**, which leads to **unexpected or incorrect behavior**.

Let’s break it down and explain **what it does, what the goal might be, and what's wrong.**

---

### ✅ **Code Summary:**

```java
JoinMeth t = new JoinMeth();
t.setDaemon(true);   // Daemon thread
t.start();

Thread mainThread = Thread.currentThread();
mainThread.join();   // Main thread waiting for itself
```

---

### 🧠 **What is happening:**

1. A daemon thread `t` is started — it prints numbers from 0 to 100.
    
2. The main thread calls `mainThread.join();` — i.e., **main thread is telling itself to wait for itself to finish**, which is a **deadlock** (infinite wait).
    
3. Because the main thread is now stuck forever, the JVM does not exit.
    
4. But the only other thread is a **daemon thread**, which will **only run as long as a user thread is running**.
    

---

### ⚠️ **Result:**

- **The program hangs (runs forever)** — main thread is waiting for itself, so it never finishes.
    
- **Daemon thread may or may not complete** depending on timing, but it’s **meaningless** here since JVM is stuck.
    

---

### 🎯 **What might the original goal have been?**

Possibly one of these:

- The author wanted to **demonstrate daemon threads**, but mistakenly used `mainThread.join()` instead of `t.join()`.
    
- Or the author intended to show **that setting a thread as daemon and calling join incorrectly causes issues**.
    

---

### ✅ **Correct version (if the goal is to demonstrate daemon thread being terminated early):**

java

Copy code

`class JoinMeth extends Thread {     public void run() {         for (int i = 0; i <= 100; i++) {             System.out.println("Daemon: " + i);         }     } }  public class _06_JoinMethod {     public static void main(String[] args) {         JoinMeth t = new JoinMeth();         t.setDaemon(true);  // Daemon thread         t.start();          // Let main sleep for a bit to observe daemon thread before main ends         try {             Thread.sleep(10);  // main thread will exit soon, killing daemon         } catch (InterruptedException e) {             System.out.println(e);         }          System.out.println("Main thread finished.");     } }`

---

### ✅ **Correct version (if goal is to demonstrate `join()` properly):**

java

Copy code

`class JoinMeth extends Thread {     public void run() {         for (int i = 0; i <= 100; i++) {             System.out.println("Thread: " + i);         }     } }  public class _06_JoinMethod {     public static void main(String[] args) {         JoinMeth t = new JoinMeth();         t.start();          try {             t.join();  // Wait for t to finish before main ends         } catch (InterruptedException e) {             System.out.println(e);         }          System.out.println("Main thread finished.");     } }`

---

### 📌 Final Note:

> ❌ **Never call `mainThread.join()` from within the main thread itself** — it will cause the program to **wait on itself forever**, leading to deadlock.
