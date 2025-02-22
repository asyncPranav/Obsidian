
```java
package $08_Methods;

class Television {
    private int channel;
    private int volume;
	
    public void currentChannel() {
        System.out.println("current channel : " + channel);
    }
	
    public void currentVolume() {
        System.out.println("current volume : " + volume);
    }
}

public class _22_Class_and_object {
    public static void main(String[] args) {
        Television t = new Television();
        t.currentChannel();
        t.currentVolume();
    }
}
```


Alright! Let's make this as clear as possible by breaking it down into two main parts:

1. **Theory Explanation:** Detailed concepts on how classes, objects, and methods are stored in Java's memory model.
2. **Example Explanation:** Step-by-step analysis of the given Java code with memory tables at each stage.

---

# 1. **Theory Explanation: How Memory Works in Java**

Java uses three primary memory areas for program execution:

1. **Method Area**
2. **Heap**
3. **Stack**

---

## **1. Method Area**

- **What It Is:**
    - A shared memory space where class-level data is stored.
    - It stores class definitions, static variables, method code, and constants.
- **When It's Used:**
    - When a class is loaded into memory for the first time by the JVM.
- **What's Stored:**
    - Class information (`Television`, `_22_Class_and_object` in this example).
    - Method definitions (`currentChannel()`, `currentVolume()`, `main()` method).
    - Static variables (none in this example, but if there were, they'd be here).
- **Shared or Not?**
    - Yes, it's shared among all threads, meaning all parts of the program can access this area.

---

## **2. Heap**

- **What It Is:**
    - A memory area used for dynamic memory allocation.
    - All objects and their instance variables are stored here.
- **When It's Used:**
    - When you create an object using the `new` keyword.
- **What's Stored:**
    - Objects (`Television` object in this example).
    - Instance variables (`channel` and `volume` for each `Television` object).
- **Shared or Not?**
    - Yes, shared across all threads. Multiple parts of the program can reference the same object.
- **Garbage Collection:**
    - Objects without any references (like the `Television` object after `main()` ends) are eligible for garbage collection.

---

## **3. Stack**

- **What It Is:**
    - A memory area that manages method execution.
    - It works on the **Last In, First Out (LIFO)** principle.
- **When It's Used:**
    - Every time a method is called, a new "Stack Frame" is created for that method.
    - The frame is removed when the method finishes execution.
- **What's Stored:**
    - Local variables (e.g., reference `t` in `main()` method).
    - Method call information (return address, intermediate values).
- **Shared or Not?**
    - No, each thread gets its own stack. This keeps method calls isolated and prevents interference.
- **Stack Frames:**
    - A new frame is created for each method call.
    - Frame is removed when the method finishes.

---

## **Summary of Memory Model:**

- **Method Area:** Class-level data and method code (shared).
- **Heap:** Objects and instance variables (shared, managed by Garbage Collector).
- **Stack:** Method calls and local variables (not shared, thread-specific).

---

# 2. **Example Explanation: Memory Layout for the Code**

Let's go through the Java code step by step, explaining how memory is used at each stage.

```java
package $08_Methods;

class Television {
    private int channel;
    private int volume;
	
    public void currentChannel() {
        System.out.println("current channel : " + channel);
    }
	
    public void currentVolume() {
        System.out.println("current volume : " + volume);
    }
}

public class _22_Class_and_object {
    public static void main(String[] args) {
        Television t = new Television();
        t.currentChannel();
        t.currentVolume();
    }
}
```


---

## **Step 1: Class Loading**

- The JVM loads both classes: `Television` and `_22_Class_and_object`.
- **Stored in Method Area:**

|**Method Area**|**Heap**|**Stack**|
|---|---|---|
|Class `Television`|(Empty)|(Empty)|
|- `int channel`|||
|- `int volume`|||
|- `currentChannel()`|||
|- `currentVolume()`|||
|Class `_22_Class_and_object`|||
|- `main()`|||

- **Method Area Details:**
    - Class definitions for `Television` and `_22_Class_and_object`.
    - Methods are stored as bytecode, waiting to be executed.
- **Heap and Stack:**
    - Both are empty because no objects are created and no methods are running yet.

---

## **Step 2: Entering `main()` Method**

This line executes:

```java
Television t = new Television();
```

|**Method Area**|**Heap**|**Stack**|
|---|---|---|
|Class `Television`|`Television` Object|`main()` Frame|
|- `int channel`|- `channel = 0`|- Reference `t` → Heap Object|
|- `int volume`|- `volume = 0`||
|- `currentChannel()`|||
|- `currentVolume()`|||
|Class `_22_Class_and_object`|||
|- `main()`|||

- **Heap Details:**
    - A `Television` object is created with default values: `channel = 0` and `volume = 0`.
- **Stack Details:**
    - `main()` frame is created.
    - Local variable `t` stores the reference to the `Television` object on the Heap.

---

## **Step 3: Calling `t.currentChannel()`**

This line executes:

```java
t.currentChannel();
```

|**Method Area**|**Heap**|**Stack**|
|---|---|---|
|Class `Television`|`Television` Object|`main()` Frame|
|- `int channel`|- `channel = 0`|- Reference `t` → Heap Object|
|- `int volume`|- `volume = 0`|`currentChannel()` Frame|
|- `currentChannel()`||- `this` → Heap Object|
|- `currentVolume()`|||

- **Stack Details:**
    - A new frame for `currentChannel()` is created.
    - `this` points to the `Television` object on the Heap.
- **Output Produced:**
    
```shell
current channel : 0
```
    

---

## **Step 4: Calling `t.currentVolume()`**

This line executes:

```java
t.currentVolume();
```


- **Similar to `currentChannel()`**, but the frame is for `currentVolume()` this time.
- **Output Produced:**
    
```shell
current volume : 0
```
    

---

## **Step 5: End of `main()`**

- **Stack:** All frames are removed.
- **Heap:** `Television` object becomes eligible for **Garbage Collection**.
- **Output Produced:**

```shell
current channel : 0 
current volume : 0
```


---

## **Summary Recap:**

- `Method Area`: Stores class info and methods.
- `Heap`: Stores the `Television` object with `channel` and `volume`.
- `Stack`: Manages method calls using frames.
- `Garbage Collection`: Unreferenced objects are cleaned up.

