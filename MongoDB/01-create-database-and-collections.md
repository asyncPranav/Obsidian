
---

## **Chapter 1: Create Database and Collections**


### **1️⃣ Show all databases**

**Command:**

```mongo
show dbs
```

**What it does:**

- Lists all databases in your MongoDB server.
    
- Only databases with some data in them will appear.
    

**Example:**

`> show dbs admin    0.000GB local    0.000GB myDB     0.001GB`

---

### **2️⃣ Switch to a database / Create a new database**

**Command:**

`use <database_name>`

**What it does:**

- If the database exists → switches to it.
    
- If the database does **not exist** → it **creates it** in memory (it will appear in `show dbs` only after you insert some data).
    

**Example:**

`> use myDB switched to db myDB`

- At this point, `myDB` is created in memory. If you run `show dbs`, you might **not see it yet** until you create a collection or insert data.
    

---

### **3️⃣ Create a collection**

**Command:**

`db.createCollection("collection_name")`

**What it does:**

- Creates a **collection** (similar to a table in SQL) in the current database.
    

**Example:**

`> db.createCollection("students") { ok: 1 }`

- `students` is now a collection inside `myDB`.
    

**Tip:**  
You **don’t always need `db.createCollection()`**. MongoDB creates a collection automatically when you **insert the first document**.

`> db.students.insertOne({name: "John", age: 20})`

- After this, the `students` collection is created automatically.
    

---

### **4️⃣ Show all collections**

**Command:**

`show collections`

**What it does:**

- Lists all collections in the current database.
    

**Example:**

`> show collections students`

---

✅ **Quick Recap Flow:**

1. `show dbs` → see databases
    
2. `use myDB` → switch/create database
    
3. `db.createCollection("students")` → create collection
    
4. `show collections` → see collections