
---

# 🌟 Java Multithreading - Very Detailed Notes

---

## 🔹 1. **What is Multithreading?**

**Multithreading** is a programming concept that allows the execution of two or more parts of a program (called threads) simultaneously.

✅ A **thread** is the smallest unit of a process that can be executed independently.

**Multithreading = Multiple threads running concurrently within the same process.**

> ✅ Example: In a text editor – typing, spell checking, and autosaving can happen in parallel using different threads.

---

## 🔹 2. **Benefits of Multithreading**

- ✔️ Efficient CPU Utilization
    
- ✔️ Simultaneous task execution
    
- ✔️ Better performance for resource-intensive apps
    
- ✔️ Useful for I/O operations (network, file I/O)
    

---

## 🔹 3. **Thread vs Process**

|Feature|Process|Thread|
|---|---|---|
|Definition|Independent program in execution|Lightweight sub-part of process|
|Memory|Separate memory per process|Shares memory of process|
|Communication|Complex (IPC needed)|Easy (shared memory)|
|Dependency|Independent|May depend on other threads|

---

## 🔹 4. **Creating Threads in Java**

There are **three main ways**:

### A. **By extending the `Thread` class**

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread running: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start();  // Do not use run()
    }
}
```

### B. **By implementing the `Runnable` interface**

```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Runnable thread running");
    }
}

public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread(new MyRunnable());
        t1.start();
    }
}
```

### C. **Using lambda expressions (Java 8+)**

```java
public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {
            System.out.println("Lambda thread running");
        });
        t1.start();
    }
}
```

---

## 🔹 5. **Thread Lifecycle**

A thread has the following states:

1. **New** – Thread created but not started
    
2. **Runnable** – Thread ready to run
    
3. **Running** – Thread currently executing
    
4. **Blocked/Waiting** – Waiting for resource or signal
    
5. **Terminated (Dead)** – Thread has finished execution
    

### Diagram:

```java
New → Runnable → Running → Terminated
           ↘
          Waiting / Blocked
           ↘
         Runnable
```


## 📘 Java Thread Lifecycle

A **Java thread** undergoes the following **6 main states** during its lifecycle:

```java
NEW → RUNNABLE → RUNNING → BLOCKED / WAITING / TIMED_WAITING → TERMINATED
```

---

### 🔹 1. NEW State

- Thread is created but **not started**.
    
- Constructor has been called, but `start()` hasn't.
    

```java
Thread t = new Thread();  // NEW state
```

---

### 🔹 2. RUNNABLE State

- Thread is **ready to run** and waiting for CPU.
    
- After calling `start()` method.
    

```java
t.start();  // Moves thread to RUNNABLE
```

---

### 🔹 3. RUNNING State

- Thread is **actively executing**.
    
- JVM **scheduler** picks it from RUNNABLE state.
    

> Note: JVM decides when a thread actually runs.

---

### 🔹 4. BLOCKED State

- Thread is **waiting to acquire a lock** on an object (used in synchronization).
    
- It enters BLOCKED if another thread holds the lock.
    

```java
synchronized(obj) {
    // If another thread is using obj, current thread becomes BLOCKED
}
```

---

### 🔹 5. WAITING State

- Thread is **waiting indefinitely** for another thread to perform a task.
    
- It remains here until another thread **notifies** it.
    

```java
t.wait();        // WAITING
t.join();        // WAITING
```

Use `notify()` or `notifyAll()` to wake it up.

---

### 🔹 6. TIMED_WAITING State

- Similar to WAITING, but waits for **a specific time**.
    
- After time expires, returns to RUNNABLE.
    

```java
Thread.sleep(1000);    // TIMED_WAITING
t.join(5000);          // TIMED_WAITING
wait(3000);            // TIMED_WAITING
```

---

### 🔹 7. TERMINATED (DEAD) State

- Thread has **finished execution** or has been **forcefully stopped**.
    
- It **cannot be restarted**.
    

---

## 🔄 Thread Lifecycle Diagram

```java
   NEW
    |
    v
 START() → RUNNABLE → RUNNING
                        |
    -------------------------------------------
    |                      |                  |
BLOCKED             WAITING         TIMED_WAITING
    \_____________________|_____________________/
                        |
                      RUNNABLE
                        |
                     TERMINATED
```

---

## 🔍 Code Example: Thread Lifecycle

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running...");
        try {
            Thread.sleep(2000); // TIMED_WAITING
        } catch (InterruptedException e) {
            System.out.println(e);
        }
        System.out.println("Thread finished.");
    }
}

public class ThreadLifecycle {
    public static void main(String[] args) {
        MyThread t = new MyThread(); // NEW
        System.out.println("State: " + t.getState());
        t.start();                   // RUNNABLE
        System.out.println("State after start: " + t.getState());
		
        try {
            Thread.sleep(100);      // Give time to enter sleep
            System.out.println("State while sleeping: " + t.getState());
            t.join();               // WAITING for t to finish
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
		
        System.out.println("State after finish: " + t.getState()); // TERMINATED
    }
}
```

---

## 📌 Summary Table

| State         | Trigger                  | Description              |
| ------------- | ------------------------ | ------------------------ |
| NEW           | Thread created           | Not yet started          |
| RUNNABLE      | `start()`                | Ready to run             |
| RUNNING       | CPU scheduler            | Actively running         |
| BLOCKED       | Locked resource          | Waiting for monitor/lock |
| WAITING       | `wait()`, `join()`       | Waiting indefinitely     |
| TIMED_WAITING | `sleep()`, `join(5000)`  | Waiting for fixed time   |
| TERMINATED    | Task finished or stopped | Dead, cannot restart     |


---

## 🔹 6. **Common Thread Methods**

|Method|Description|
|---|---|
|`start()`|Starts a thread|
|`run()`|Code executed by the thread|
|`sleep(ms)`|Pauses execution for milliseconds|
|`join()`|Waits for thread to finish|
|`isAlive()`|Checks if thread is still alive|
|`getName()`|Gets thread name|
|`setName()`|Sets thread name|
|`setPriority(int)`|Sets priority (1–10)|

---

## 🔹 7. **Thread Priority**

- `Thread.MIN_PRIORITY = 1`
    
- `Thread.NORM_PRIORITY = 5`
    
- `Thread.MAX_PRIORITY = 10`
    

But actual execution depends on OS thread scheduler.

---

## 🔹 8. **Thread Synchronization**

When **multiple threads** access shared data, inconsistency may occur. This is called a **race condition**.

### 🔐 `synchronized` Keyword:

```java
class Counter {
    int count = 0;

    synchronized void increment() {
        count++;
    }
}
```

### Ways to Synchronize:

- **Synchronized method**
    
- **Synchronized block**:
    

```java
synchronized(obj) {
    // code
}
```

---

## 🔹 9. **Inter-thread Communication**

Allows threads to **communicate and cooperate** with each other.

### Methods from `Object` class:

|Method|Description|
|---|---|
|`wait()`|Releases lock and waits|
|`notify()`|Wakes up one waiting thread|
|`notifyAll()`|Wakes up all waiting threads|

### Example:

```java
synchronized(obj) {
    obj.wait();    // waits
    obj.notify();  // notifies
}
```

---

## 🔹 10. **Deadlock**

A **deadlock** occurs when two or more threads are blocked forever, waiting on each other.

### Example:

```java
Thread-1: locks A → waits for B  
Thread-2: locks B → waits for A
```

💡 Solution: Always acquire locks in a fixed order.

---

## 🔹 11. **Thread Pooling (Executor Framework)**

Creating too many threads wastes resources. **Thread Pool** reuses a limited number of threads.

### Using `ExecutorService`:

```java
ExecutorService executor = Executors.newFixedThreadPool(5);
executor.submit(() -> {
    System.out.println("Thread from pool");
});
executor.shutdown();
```

---

## 🔹 12. **Volatile Keyword**

Used to mark a variable as **modified by multiple threads**. Prevents caching, always reads from main memory.

```java
volatile boolean running = true;
```

---

## 🔹 13. **Daemon Threads**

**Daemon Thread:** A thread that runs in the background and does not prevent the JVM from exiting. It dies automatically when the main thread or other user (non-daemon) threads finish.

### 🔹 Examples of Daemon Threads:

- **Garbage Collector**
    
- **Background logging**
    
- **Timer services**
    
- **Auto-save tasks**
    

---

### 🔹 Key Points:

|Feature|Details|
|---|---|
|Lifecycle|Dies when all user threads are finished|
|Creation|Use `Thread t = new Thread();`|
|Set as daemon|`t.setDaemon(true);` → **before** calling `t.start()`|
|Check if daemon|`t.isDaemon()` returns `true` or `false`|
|JVM behavior|JVM exits when only daemon threads are running|

---

### 🔹 Syntax:

```java
Thread t = new Thread();
t.setDaemon(true); // must be before start()
t.start();
```

---

### 🔹 Example Code:

```java
class MyDaemonThread extends Thread {
    public void run() {
        for(int i = 0; i < 10; i++) {
            System.out.println("Daemon Thread running: " + i);
            try { Thread.sleep(500); } catch(Exception e) {}
        }
    }
}

public class DaemonExample {
    public static void main(String[] args) {
        MyDaemonThread t = new MyDaemonThread();
        t.setDaemon(true); // set as daemon before start
        t.start();
		
        // Main thread
        System.out.println("Main thread sleeping...");
        try { Thread.sleep(1000); } catch(Exception e) {}
        System.out.println("Main thread finished");
    }
}
```

**Output (may not print all daemon thread lines):**

```sh
Main thread sleeping...
Daemon Thread running: 0
Daemon Thread running: 1
Main thread finished
```

---

### 🔴 Common Mistakes:

1. ❌ Calling `setDaemon(true)` **after** `start()` – results in **`IllegalThreadStateException`**.
    
2. ❌ Assuming daemon threads will always finish – they **may not**.
    

---

### 🔹 Daemon vs User Thread

|Feature|Daemon Thread|User Thread|
|---|---|---|
|Purpose|Background services|Main logic of the application|
|JVM waits?|❌ No|✅ Yes|
|Termination|Automatically when no user thread|Must finish or be stopped manually|

---

### 🧠 Remember:

- Use daemon threads for **non-critical** tasks.
    
- Always set daemon **before starting** the thread.
    
- Main thread sleeping gives daemon time to work.


---


## 🔷 `join()` Method in Java

### 📌 Definition:

The `join()` method is used to **pause the execution of the current thread** until the thread on which `join()` was called **completes its execution**.

---

### 🔧 Syntax:

```java
thread.join();          // Waits indefinitely until thread finishes
thread.join(milliseconds);  // Waits at most specified time
```

---

### 📘 Example:

```java
class MyThread extends Thread {
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Child Thread: " + i);
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                System.out.println(e);
            }
        }
    }
}

public class JoinExample {
    public static void main(String[] args) {
        MyThread t = new MyThread();
        t.start();
		
        try {
            t.join();  // Main thread waits here until t finishes
        } catch (InterruptedException e) {
            System.out.println(e);
        }
		
        System.out.println("Main thread finished.");
    }
}
```

### 🧾 Output:

```sh
Child Thread: 1
Child Thread: 2
Child Thread: 3
Child Thread: 4
Child Thread: 5
Main thread finished.
```

---

### ⏱ `join(milliseconds)` Version:

```java
t.join(1000);  // Main thread waits at most 1 second
```

- If `t` finishes earlier, main resumes immediately.
    
- If not, main resumes after 1 second even if `t` is still running.
    

---

### 🧠 Why use `join()`?

- To ensure one thread finishes before another starts or continues.
    
- Used in **thread coordination**, especially in:
    
    - Loading data before display
        
    - Sequential task execution
        
    - Test frameworks
        

---

### ⚠️ Important Notes:

|Point|Explanation|
|---|---|
|`join()` throws `InterruptedException`|Must be handled using try-catch|
|Call only after `start()`|Else it has no effect|
|Main thread can use `join()`|To wait for child threads to finish|
|You can chain joins|Wait for multiple threads in order|

---

### 🔁 Multiple Thread Join Example:

```java
Thread t1 = new Thread(() -> {
    System.out.println("Thread 1 running...");
});
Thread t2 = new Thread(() -> {
    System.out.println("Thread 2 running...");
});

t1.start();
t1.join(); // main waits for t1
t2.start();
t2.join(); // main waits for t2
System.out.println("Main thread finished");
```

---

Here is a **text-based diagram** of how the `join()` method works in Java:

```java
Main Thread
    |
    v
t.start();  ----------------------------->  [Thread t starts execution]
    |                                           |
    v                                           v
t.join();                               [Thread t finishes execution]
    |                                           |
    v                                           |
[Main thread resumes after t completes] <-------
```

### 🔎 Explanation:

- The **main thread** starts thread `t`.
    
- It then calls `t.join();`, which makes it **wait** until `t` finishes.
    
- After `t` finishes, **main thread resumes** execution.
    

---


Here is a **combined text diagram** showing how `join()`, `sleep()`, and **daemon thread** behavior work together in Java:


### 🧵 Threads Involved:

- `Main Thread` (User thread)
    
- `Child Thread (t1)` using `.join()`
    
- `Daemon Thread (t2)`
    

---

### 🔁 Flow Diagram:

```java
Main Thread
   |
   |--> t1.start()   // User thread
   |--> t2.setDaemon(true)
   |--> t2.start()   // Daemon thread
   |
   |--> t1.join()    // Main waits until t1 finishes
   |       |
   |       v
   |   [t1 runs, maybe uses Thread.sleep()]
   |       |
   |   [t1 completes]
   |
   v
[Main resumes]
   |
   v
[Main thread ends] ---> [Daemon thread (t2) auto-terminates immediately if not finished]
```

---

### 🔍 Explanation:

- `t1` is a **user thread**, started and waited on using `.join()`. `Main` pauses until `t1` is done.
    
- `t2` is a **daemon thread**, running in the background. It does not prevent JVM from exiting.
    
- Once **main thread finishes (after t1)**, the **JVM kills daemon thread t2**, even if it's not finished.
    

---

### ✅ Key Concepts Demonstrated:

|Concept|Behavior|
|---|---|
|`join()`|Main waits until the joined thread completes|
|`sleep()`|Temporarily pauses a thread (does **not release locks**)|
|Daemon Thread|Auto-terminated when no user threads remain|

---

## 🧵 `Thread.yield()` Method in Java

---

### 📘 Definition:

`Thread.yield()` is a **static method** of the `Thread` class that:

- Pauses the **currently executing thread** temporarily,
    
- Gives a **chance for other threads of equal priority** to execute.
    

> It’s a **hint** to the thread scheduler: "I'm willing to pause, please let someone else run."

---

### 🔧 Syntax:

```java
Thread.yield();
```

---

### ⚙️ Behavior:

- The currently running thread **goes from RUNNING → READY (Runnable)** state.
    
- The CPU scheduler may choose another thread of **equal or higher priority** to run.
    
- There's **no guarantee** that the current thread will pause.
    
- The **scheduler may ignore** the yield request.
    

---

### 🧠 Use Cases:

- When you want to **improve thread cooperation** or responsiveness.
    
- In **CPU-intensive tasks**, where fair sharing is desired.
    
- In demo/testing of thread behavior (e.g., simulating thread scheduling).
    

---

### 📌 Example:

java

Copy code

`class YieldDemo extends Thread {     public void run() {         for (int i = 1; i <= 5; i++) {             System.out.println(Thread.currentThread().getName() + ": " + i);             Thread.yield();  // Suggest giving up CPU         }     } }  public class YieldExample {     public static void main(String[] args) {         YieldDemo t1 = new YieldDemo();         YieldDemo t2 = new YieldDemo();          t1.start();         t2.start();     } }`

---

### 📝 Output (may vary):

makefile

Copy code

`Thread-0: 1 Thread-1: 1 Thread-0: 2 Thread-1: 2 ...`

- Output order is **not predictable** due to thread scheduler decisions.
    

---

### 🔍 Notes:

|Concept|Detail|
|---|---|
|Static method|Called as `Thread.yield()`, not on specific thread object|
|Non-blocking|Does not pause or sleep — just suggests yielding CPU|
|No guarantee|Scheduler may ignore it completely|
|Works with|Threads of **equal priority** (lower priority threads are not considered)|
|Opposite of|`sleep()` — `yield()` is cooperative, `sleep()` is forced delay|

---

### ⚠️ Limitations:

- Not reliable for **precise control** of thread execution.
    
- Should not be used for synchronization.
    
- Often **not used in real-world apps**, more for learning or fine-tuning performance.

---

## 🔹 14. **Thread-safe Collections**

For multithreaded applications:

- `Vector` (thread-safe)
    
- `Collections.synchronizedList()`
    
- `ConcurrentHashMap` (more efficient)
    

---

## 🔹 15. **Advanced Java Concurrency API**

From `java.util.concurrent` package:

|Class / Interface|Use|
|---|---|
|`Executor`, `ExecutorService`|Manage thread pools|
|`Future`, `Callable`|Return results from threads|
|`Semaphore`, `CountDownLatch`|Thread coordination|
|`ReentrantLock`|Advanced locking mechanism|
|`CyclicBarrier`|Sync threads at a common point|
|`BlockingQueue`|Safe queue for threads|

---

## 🔹 16. **Callable vs Runnable**

|Feature|Runnable|Callable|
|---|---|---|
|Return|No return value|Returns value|
|Exception|Cannot throw checked|Can throw checked|
|Interface|Runnable|Callable<T>|

---

## 🔹 17. **Best Practices in Multithreading**

- Use **thread pool** instead of creating threads manually
    
- Avoid **shared mutable state**
    
- Use **synchronization** wisely (too much causes bottlenecks)
    
- Avoid **deadlocks** – lock ordering or timeout
    
- Prefer **high-level concurrency tools** (`ExecutorService`, `Atomic`, etc.)
    

---

## ✅ Summary

Multithreading is powerful but **complex**. Start with basics → practice creating threads → learn synchronization → explore concurrent utilities.

---


