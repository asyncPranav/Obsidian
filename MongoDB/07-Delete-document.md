
---

# **CRUD – DELETE (MongoDB / mongosh)**

In MongoDB, **DELETE** means **removing documents or collections or databases**.

MongoDB provides **3 levels of delete**:

1️⃣ **Delete documents**  
2️⃣ **Delete collections**  
3️⃣ **Delete databases**

---

## 🔹 **1️⃣ Delete Documents (Most Important)**

### **A) `deleteOne()`**

**Command:**

```js
db.collection_name.deleteOne(filter)
```

### 🔍 What it does

- Deletes **ONLY ONE document**
    
- Even if multiple documents match → deletes **first matched document**
    
- If nothing matches → deletes nothing
    

### ✅ Example

```js
db.students.deleteOne({ name: "Bob" })
```

**Result:**

```js
{ acknowledged: true, deletedCount: 1 }
```

---

### ❗ Important Points

- Safe for **single delete**
    
- Commonly used in **production**
    
- Does NOT throw error if no document found
    

---

## **B) `deleteMany()`**

**Command:**

```js
db.collection_name.deleteMany(filter)
```

### 🔍 What it does

- Deletes **ALL documents** matching filter
    

### ✅ Example

```js
db.students.deleteMany({ age: { $lt: 18 } })
```

Deletes all students whose age is less than 18.

**Result:**

```js
{ acknowledged: true, deletedCount: 5 }
```

---

### ❗ Important Warning ⚠️

```js
db.students.deleteMany({})
```

🚨 **Deletes ALL documents in collection**  
(Collection remains, documents gone)

---

## **C) Delete ALL documents but keep collection**

```js
db.students.deleteMany({})
```

- Collection stays
    
- Indexes stay
    
- Only documents removed
    

---

## 🔹 **2️⃣ `findOneAndDelete()` (Advanced & Useful)**

**Command:**

```js
db.collection_name.findOneAndDelete(filter)
```

### 🔍 What it does

- Finds **one document**
    
- Deletes it
    
- Returns the **deleted document**
    

### ✅ Example

```js
db.students.findOneAndDelete({ name: "Alice" })
```

**Output:**

```js
{
  _id: ObjectId("..."),
  name: "Alice",
  age: 21
}
```

📌 Useful when:

- You want to **delete + see deleted data**
    
- Logging
    
- Undo operations
    

---

## 🔹 **3️⃣ Delete with Conditions (Operators)**

You can delete using **query operators**:

### **Comparison Operators**

```js
$eq   $ne
$gt   $gte
$lt   $lte
```

Example:

```js
db.students.deleteMany({ marks: { $gte: 90 } })
```

---

### **Logical Operators**

```js
$and
$or
$not
$nor
```

Example:

```js
db.students.deleteMany({
  $or: [
    { age: { $lt: 18 } },
    { status: "inactive" }
  ]
})
```

---

### **Array Delete**

```js
db.students.deleteMany({ skills: "React" })
```

Deletes documents where `skills` array contains `"React"`.

---

## 🔹 **4️⃣ Delete using `_id` (Most Accurate)**

```js
db.students.deleteOne({
  _id: ObjectId("650c3c9f3a1a9e0b1f4d2f5a")
})
```

✔ Fast  
✔ Unique  
✔ Recommended

---

## 🔹 **5️⃣ Delete Collection**

### **A) Drop Collection**

```js
db.collection_name.drop()
```

### 🔍 What it does

- Deletes **entire collection**
    
- Deletes **documents + indexes + metadata**
    

### ✅ Example

`db.students.drop()`

**Result:**

`true`

🚨 **Cannot be undone**

---

### ❗ Difference

|Method|What happens|
|---|---|
|`deleteMany({})`|Deletes documents only|
|`drop()`|Deletes entire collection|

---

## 🔹 **6️⃣ Delete Database**

### **Drop Current Database**

`db.dropDatabase()`

### 🔍 What it does

- Deletes **entire database**
    
- Removes:
    
    - All collections
        
    - All documents
        
    - All indexes
        

### ✅ Example

`use myDB db.dropDatabase()`

**Result:**

`{ ok: 1 }`

🚨 **Dangerous command**

---

## 🔹 **7️⃣ Delete All Collections in a Database**

`db.getCollectionNames().forEach(c => db[c].drop())`

✔ Clears DB  
✔ Keeps database name

---

## 🔹 **8️⃣ Soft Delete (Industry Practice)**

❌ **Avoid hard delete in real apps**  
✅ Use **soft delete**

### **Soft Delete Example**

`db.users.updateOne(   { email: "test@gmail.com" },   { $set: { isDeleted: true, deletedAt: new Date() } } )`

Then query only active users:

`db.users.find({ isDeleted: false })`

📌 Used in:

- Banking
    
- E-commerce
    
- User systems
    

---

## 🔹 **9️⃣ Safe Delete Pattern (Best Practice)**

### **Step 1: Preview**

`db.students.find({ age: { $lt: 18 } })`

### **Step 2: Delete**

`db.students.deleteMany({ age: { $lt: 18 } })`

✔ Always preview before delete

---

## 🔹 **10️⃣ Deprecated Delete Methods (Know for Interviews)**

🚫 **DO NOT USE**

`db.students.remove()`

Reason:

- Deprecated
    
- Replaced by `deleteOne()` and `deleteMany()`
    

---

## 🔹 **11️⃣ Delete Return Values**

`{   acknowledged: true,   deletedCount: 3 }`

|Field|Meaning|
|---|---|
|acknowledged|Server accepted request|
|deletedCount|Number of deleted documents|

---

## ✅ **FINAL DELETE COMMAND CHEAT SHEET**

|Task|Command|
|---|---|
|Delete one document|`deleteOne()`|
|Delete multiple|`deleteMany()`|
|Delete all docs|`deleteMany({})`|
|Delete & return doc|`findOneAndDelete()`|
|Delete by `_id`|`deleteOne({_id: ObjectId()})`|
|Drop collection|`collection.drop()`|
|Drop database|`db.dropDatabase()`|
|Soft delete|`$set: {isDeleted:true}`|

---

## 🧠 **Interview Tips**

- Prefer `_id` delete
    
- Avoid `deleteMany({})` without checking
    
- Soft delete is **real-world standard**
    
- `drop()` is faster than `deleteMany({})`