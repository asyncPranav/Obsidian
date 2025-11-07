
---

# 🧠 Hashing Concepts (Foundation)

### 🔹 What is Hashing?

- **Hashing** is a technique used to **store and access data quickly**.
    
- It converts a _key_ (like a name, roll number, or string) into a **fixed-size number**, known as a **hash code**.
    
- This hash code decides **where (index)** the data will be stored in memory.
    
- You can think of it as a **mapping function** — it maps a large set of data (keys) to a smaller range of memory indexes.
    

🧩 **Goal of Hashing:**  
To make searching, insertion, and deletion operations **very fast (O(1))** on average.

---

### 🔹 What is a Hash Function?

- A **hash function** takes an input key and produces an integer value (called hash code).
    
- This integer determines the **bucket index** where the data will be stored.
    
- For example:
    
    `index = hash(key) % table_size;`
    
- Example:  
    If key = "Amit" and `hash("Amit") = 12345`,  
    then if table_size = 10 → index = 12345 % 10 = 5 → data stored in bucket 5.
    

🧠 **A good hash function should:**

1. Produce different hash codes for different keys (to reduce collisions).
    
2. Distribute data evenly across all buckets.
    
3. Be fast to compute.
    

---

### 🔹 What is a Hash Code?

- A **hash code** is the integer value produced by the hash function.
    
- It is used to find the **index (bucket)** where the key–value pair will be stored.
    
- In Java, every object has a built-in `hashCode()` method (from the `Object` class).
    

Example:

`String name = "John"; int code = name.hashCode(); // generates hash code`

---

### 🔹 What is a Collision?

- A **collision** occurs when **two different keys** produce the **same hash index**.  
    Example:
    
    `hash("Amit") % 10 = 5   hash("Sumit") % 10 = 5`
    
    Both map to the same bucket → **collision happens.**
    

---

### ⚙️ Collision Handling Techniques

#### 1. **Chaining**

- Each index (bucket) stores a **linked list** of entries that have the same hash index.
    
- When a collision happens, the new key-value pair is simply added to the linked list at that index.
    

✅ **Pros:** Easy to implement, flexible.  
❌ **Cons:** Uses extra space (linked lists).

#### 2. **Open Addressing**

- All elements are stored **in the hash table itself** (no linked lists).
    
- If a collision happens, the algorithm searches for the **next empty slot**.
    

Types:

1. **Linear Probing:** Check the next index `(index + 1) % size`.
    
2. **Quadratic Probing:** Jump by squares `(index + i²)`.
    
3. **Double Hashing:** Use a second hash function to find the next slot.
    

✅ **Pros:** Saves space (no external linked list).  
❌ **Cons:** Can lead to clustering (continuous filled slots).

---

### 🧩 How Key–Value Pairs are Stored

- The hash function converts the key into a hash code → which decides the **bucket index**.
    
- The **key-value pair** is stored at that index in the hash table.
    
- Example (simplified):
    
```java
index = hash(key) % table_size
table[index] = (key, value)
```
    

---

# 2️⃣ HashMap (Modern, Most Used)

### 🔹 Introduction

- Introduced in **Java 1.2** (part of the _Collections Framework_).
    
- Stores data in the form of **key–value pairs**.
    
- Implements the **Map interface**.
    
- Internally uses **hashing** + **LinkedList** / **Tree (since Java 8)** to handle collisions.
    

### 🔹 Key Points

- **Allows one null key** and **multiple null values.**
    
- **Not synchronized** → faster but **not thread-safe.**
    
- Order of elements is **not guaranteed.**
    
- When collisions occur, entries are stored as a linked list in that bucket.  
    Since Java 8, if too many collisions occur, the linked list becomes a **balanced tree** for better performance (O(log n)).
    

---

### ⚙️ Internal Working (Important)

#### put(key, value)

1. Compute hash code of key → `hashCode()`.
    
2. Apply hash function → find bucket index.
    
3. If bucket empty → store key-value pair.
    
4. If key already exists → replace value.
    
5. If collision → add to linked list or tree.
    
6. If load factor (default 0.75) exceeded → **rehashing** (table resized to double).
    

#### get(key)

1. Compute hash code and index.
    
2. Traverse bucket list / tree to find matching key.
    
3. Return value if found.
    

#### remove(key)

1. Find key’s index.
    
2. Remove from bucket’s linked list / tree.
    

---

### ⏱️ Time Complexity

|Operation|Average|Worst Case|
|---|---|---|
|put()|O(1)|O(n)|
|get()|O(1)|O(n)|
|remove()|O(1)|O(n)|

---

### 💻 Simple Example

```java
import java.util.HashMap;

class Example {
    public static void main(String[] args) {
        HashMap<Integer, String> map = new HashMap<>();

        map.put(1, "Amit");
        map.put(2, "Sumit");
        map.put(3, "Neha");

        System.out.println(map.get(2)); // Output: Sumit
        map.remove(1);
        System.out.println(map);
    }
}

```

---

# 3️⃣ HashTable (Legacy Version of HashMap)

### 🔹 Introduction

- Introduced in **Java 1.0** (before Collections Framework).
    
- Works similar to HashMap internally (array + linked list).
    
- **Synchronized** → Thread-safe but slower.
    
- **Does not allow null** keys or values.
    

### 🔹 Key Points

|Feature|HashMap|HashTable|
|---|---|---|
|Introduced|Java 1.2|Java 1.0|
|Synchronization|Not synchronized|Synchronized|
|Null key/value|1 null key, many null values|Not allowed|
|Performance|Faster|Slower|
|Legacy|Modern|Legacy|

---

### 💻 Example

```java
import java.util.Hashtable;

class Example {
    public static void main(String[] args) {
        Hashtable<Integer, String> table = new Hashtable<>();

        table.put(1, "Amit");
        table.put(2, "Sumit");
        // table.put(null, "Neha"); // ❌ Not allowed

        System.out.println(table.get(1)); // Output: Amit
    }
}
```

---

# 4️⃣ HashSet (Built on Top of HashMap)

### 🔹 Introduction

- Introduced in **Java 1.2**, part of _Collections Framework_.
    
- **Stores only unique elements.**
    
- Internally uses a **HashMap** to store elements as **keys**, with dummy values.
    

### 🔹 How It Works

- When you call `add(element)` → internally calls `map.put(element, DUMMY_VALUE)`.
    
- Because keys in a HashMap are unique → duplicates are not allowed.
    

---

### 🔹 Key Points

- Allows **null** element (only one).
    
- **Order not guaranteed.**
    
- Backed by a HashMap.
    
- Operations (add, remove, contains) are **O(1)** on average.
    

---

### 💻 Example

```java
import java.util.HashSet;

class Example {
    public static void main(String[] args) {
        HashSet<String> set = new HashSet<>();

        set.add("Amit");
        set.add("Sumit");
        set.add("Amit"); // duplicate ignored

        System.out.println(set); // [Amit, Sumit]
        System.out.println(set.contains("Amit")); // true
    }
}
```

---

# 🧾 Summary Table

|Feature|HashMap|HashTable|HashSet|
|---|---|---|---|
|Type|Map|Map|Set|
|Stores|Key–Value pairs|Key–Value pairs|Unique elements|
|Allows null|Yes (1 key)|No|Yes (1 element)|
|Synchronized|No|Yes|No|
|Performance|Fast|Slower|Fast|
|Internal Structure|Array + LinkedList/Tree|Array + LinkedList|Backed by HashMap|

---

# 💬 Common Interview Questions

1. What is hashing and how does it work in Java?
    
2. Difference between HashMap and HashTable.
    
3. How does Java handle collisions in HashMap?
    
4. What is the load factor and rehashing?
    
5. How does HashSet ensure uniqueness?
    
6. What is the difference between chaining and open addressing?
    
7. Why are hash functions important in hashing?
    
8. How does `hashCode()` and `equals()` work together in HashMap?
