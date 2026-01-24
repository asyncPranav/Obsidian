
---

## **Chapter 1: Create Database and Collections**

### **1️⃣ Show current database**

**Command:**

```js
db
```

**What it does:**

- Shows the **name of the current database** you are working on.
    

**Example:**

```js
> db
myDB
```

---

### **2️⃣ Drop a database**

**Command:**

```js
db.dropDatabase()
```

**What it does:**

- Deletes the **current database** permanently. Be careful!
    

**Example:**

```js
> db.dropDatabase()
{ ok: 1 }
```

- After this, `show dbs` will no longer show this database.

---


### **1️⃣ Show all databases**

**Command:**

```js
show dbs
```

**What it does:**

- Lists all databases in your MongoDB server.
    
- Only databases with some data in them will appear.
    

**Example:**

```js
> show dbs
admin    0.000GB
local    0.000GB
myDB     0.001GB
```

---

### **2️⃣ Switch to a database / Create a new database**

**Command:**

```js
use <database_name>
```

**What it does:**

- If the database exists → switches to it.
    
- If the database does **not exist** → it **creates it** in memory (it will appear in `show dbs` only after you insert some data).
    

**Example:**

```js
> use myDB
switched to db myDB
```

- At this point, `myDB` is created in memory. If you run `show dbs`, you might **not see it yet** until you create a collection or insert data.
    

---

### **3️⃣ Create a collection**

**Command:**

```js
db.createCollection("collection_name")
```

**What it does:**

- Creates a **collection** (similar to a table in SQL) in the current database.
    

**Example:**

```js
> db.createCollection("students")
{ ok: 1 }
```

- `students` is now a collection inside `myDB`.
    

**Tip:**  
You **don’t always need `db.createCollection()`**. MongoDB creates a collection automatically when you **insert the first document**.

```js
> db.students.insertOne({name: "John", age: 20})
```

- After this, the `students` collection is created automatically.
    

---

### **4️⃣ Show all collections**

**Command:**

```js
show collections
```

**What it does:**

- Lists all collections in the current database.
    

**Example:**

```js
> show collections
students
```

---

✅ **Quick Recap Flow:**

1. `show dbs` → see databases
    
2. `use myDB` → switch/create database
    
3. `db.createCollection("students")` → create collection
    
4. `show collections` → see collections


---

### **3️⃣ Insert a document (creates collection automatically if not exists)**

**Command:**

```js
db.collection_name.insertOne({key: "value"})
```

**What it does:**

- Adds a **document** (like a row in SQL) to a collection.
    
- If the collection doesn’t exist, MongoDB **creates it automatically**.
    

**Example:**

```js
> db.students.insertOne({name: "Alice", age: 21})
{
  acknowledged: true,
  insertedId: ObjectId("650c3c9f3a1a9e0b1f4d2f5a")
}
```

**Insert multiple documents at once:**

```js
db.students.insertMany([
  {name: "Bob", age: 22},
  {name: "Charlie", age: 23}
])
```

---

### **4️⃣ Find / Show documents**

**Command:**

```js
db.collection_name.find()
```

**What it does:**

- Shows all documents in the collection.
    

**Example:**

```js
> db.students.find()
{ _id: ObjectId("650c3c9f3a1a9e0b1f4d2f5a"), name: "Alice", age: 21 }
```

**Optional:** Make it pretty for easy reading:

```js
db.students.find().pretty()
```

---

### **5️⃣ Drop a collection**

**Command:**

```js
db.collection_name.drop()
```

**What it does:**

- Deletes the **entire collection** permanently.
    

**Example:**

```js
> db.students.drop()
true
```

---

### **6️⃣ Count documents in a collection**

**Command:**

```js
db.collection_name.countDocuments()
```

**Example:**

```js
> db.students.countDocuments()
3
```

---

### **7️⃣ Show stats of database**

**Command:**

```js
db.stats()
```

**What it does:**

- Shows useful info about the current database (collections count, size, etc.)
    

**Example:**

```js
> db.stats()
{
  db: "myDB",
  collections: 1,
  objects: 3,
  avgObjSize: 45,
  dataSize: 135,
  storageSize: 8192,
  ...
}
```

---

### ✅ **Summary of Commands for Chapter 1**

|Task|Command|
|---|---|
|Show all databases|`show dbs`|
|Switch/create database|`use dbName`|
|Show current database|`db`|
|Create collection|`db.createCollection("collection_name")`|
|Show collections|`show collections`|
|Insert document|`db.collection_name.insertOne({})`|
|Insert multiple|`db.collection_name.insertMany([{},{},...])`|
|Show documents|`db.collection_name.find()`|
|Pretty print documents|`db.collection_name.find().pretty()`|
|Drop collection|`db.collection_name.drop()`|
|Drop database|`db.dropDatabase()`|
|Count documents|`db.collection_name.countDocuments()`|
|Database stats|`db.stats()`|


---

## **1️⃣ Rename a Collection**

**Command:**

```js
db.collection_name.renameCollection("new_collection_name")
```

**What it does:**

- Changes the **name of a collection** in the current database.
    

**Example:**

```js
> db.students.renameCollection("learners")
{ ok: 1 }
```

- Now, instead of `students`, the collection is called `learners`.
    
- `show collections` will reflect the new name.
    

**Tip:**

- The **new name must not already exist** in the same database.
    
- Works even if the collection has documents inside.

---

### **4️⃣ Drop all collections (clear DB)**

- Sometimes you want to **empty a database** but keep it:
    

```js
db.getCollectionNames().forEach(function(c) { db[c].drop() })
```


---

