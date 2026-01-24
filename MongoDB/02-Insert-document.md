

---

# **Chapter 2: Insert Documents in MongoDB**

In MongoDB, a **document** is like a **row in SQL**, and it is stored in a **collection**. Documents are **JSON-like objects** (BSON) and can have flexible fields.

---

## **1️⃣ Insert a Single Document**

**Command:**

```js
db.collection_name.insertOne({field1: value1, field2: value2, ...})
```

**What it does:**

- Inserts **one document** into the collection.
    
- Automatically generates a unique `_id` if you don’t provide one.
    
- Creates the collection if it **does not exist yet**.
    

**Example:**

```js
> db.students.insertOne({name: "Alice", age: 21, grade: "A"})
{
  acknowledged: true,
  insertedId: ObjectId("650c3c9f3a1a9e0b1f4d2f5a")
}
```

**Notes:**

- `acknowledged: true` → confirms that the insert was successful.
    
- `_id` is automatically created (unique for each document).
    

---

## **2️⃣ Insert Multiple Documents at Once**

**Command:**

```js
db.collection_name.insertMany([
  {field1: value1, field2: value2},
  {field1: value1, field2: value2},
  ...
])
```

**What it does:**

- Inserts **multiple documents** in one command.
    
- More efficient than inserting one by one.
    

**Example:**

```js
> db.students.insertMany([
    {name: "Bob", age: 22, grade: "B"},
    {name: "Charlie", age: 23, grade: "A"},
    {name: "David", age: 24, grade: "C"}
])
```

**Output:**

```js
{
  acknowledged: true,
  insertedIds: [
    ObjectId("650c3d9f3a1a9e0b1f4d2f5b"),
    ObjectId("650c3d9f3a1a9e0b1f4d2f5c"),
    ObjectId("650c3d9f3a1a9e0b1f4d2f5d")
  ]
}
```

**Tip:**

- `insertMany()` also allows **ordered or unordered insert**:
    

```js
db.students.insertMany([...], {ordered: false})
```

- `ordered: false` → MongoDB continues inserting even if some documents fail.
    

---

## **3️⃣ Insert Document with Custom `_id`**

- MongoDB automatically creates `_id` if you don’t provide one, but you can set your own:
    

```js
db.students.insertOne({_id: 101, name: "Eva", age: 20})
```

**Note:**

- `_id` must be **unique**.
    
- Duplicate `_id` → error.
    

---

## **4️⃣ Insert Documents with Nested Objects**

- MongoDB supports **nested documents** and **arrays**.
    

**Example:**

```js
db.students.insertOne({
    name: "Frank",
    age: 25,
    address: { city: "Delhi", pincode: "110001" },
    subjects: ["Math", "Science", "English"]
})
```

- `address` → nested document
    
- `subjects` → array of values
    

---

## **5️⃣ Insert Documents Using Variables**

```js
let student = {name: "Grace", age: 22, grade: "B"}
db.students.insertOne(student)
```

- Useful for inserting **dynamic data** from your application.
    

---

## **6️⃣ Check Inserted Data**

- After inserting, always check:
    

```js
db.students.find()
db.students.find().pretty()  // formatted output
```

**Example:**

```js
> db.students.find().pretty()
{
  _id: ObjectId("650c3c9f3a1a9e0b1f4d2f5a"),
  name: "Alice",
  age: 21,
  grade: "A"
}
```

---

## **7️⃣ Insert Only If Not Exists (Avoid Duplicates)**

- Use **unique `_id`** or create a **unique index** on a field:
    

```js
db.students.createIndex({name: 1}, {unique: true})
```

- Then try to insert a document with the same `name` → MongoDB throws error.
    

---

## **8️⃣ Useful Tips for Inserts**

1. MongoDB is **schema-less** → documents can have different fields:
    

```js

```

2. Insert large amounts of data efficiently → **`insertMany()`**.
    
3. Always use **`find()`** to verify insert.
    
4. `_id` is mandatory and **unique**, even if you don’t provide it.
    

---

## ✅ **Summary Table: Insert Commands**

|Task|Command|Notes|
|---|---|---|
|Insert one document|`db.collection_name.insertOne({})`|Creates collection if not exists|
|Insert multiple|`db.collection_name.insertMany([{}, {}, ...])`|Ordered by default|
|Insert with custom `_id`|`db.collection_name.insertOne({_id: 101, ...})`|`_id` must be unique|
|Insert nested document|`db.collection_name.insertOne({field: {...}, array: [...]})`|Supports objects and arrays|
|Check inserted data|`db.collection_name.find()`|Use `.pretty()` for formatting|
|Insert only if unique|`db.collection_name.createIndex({field:1},{unique:true})`|Prevent duplicates|

---

💡 **Practice Tip:**

- Try inserting **at least 5-10 documents** with:
    
    - Different fields
        
    - Nested objects
        
    - Arrays
        
- Then use `find().pretty()` to see the results.