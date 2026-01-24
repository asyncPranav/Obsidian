
---

# **Chapter 3: Read / Query Documents (MongoDB)**

MongoDB allows you to **read documents** from a collection using **find commands** and **query operators**. This is equivalent to **SELECT in SQL**.

---

## **1️⃣ Find All Documents**

**Command:**

```js
db.collection_name.find()
```

**Example:**

```js
> db.students.find()
{ _id: ObjectId("650c3c9f3a1a9e0b1f4d2f5a"), name: "Alice", age: 21, grade: "A" }
{ _id: ObjectId("650c3d9f3a1a9e0b1f4d2f5b"), name: "Bob", age: 22, grade: "B" }
```

**Pretty Print:**

```js
db.students.find().pretty()
```

- Shows documents in a **readable format**.
    

---

## **2️⃣ Find One Document**

**Command:**

```js
db.collection_name.findOne(query)
```

**Example:**

```js
> db.students.findOne({name: "Alice"})
{ _id: ObjectId("650c3c9f3a1a9e0b1f4d2f5a"), name: "Alice", age: 21, grade: "A" }
```

- Returns **first matching document**.
    
- Faster when you only need **one document**.
    

---

## **3️⃣ Filter Documents with Query**

### **Basic Query**

```js
db.collection_name.find({field: value})
```

**Example:**

```js
db.students.find({grade: "A"})
```

- Returns all students with grade "A"
    

---

### **Comparison Operators**

|Operator|Meaning|Example|
|---|---|---|
|`$eq`|Equals|`{age: {$eq: 21}}`|
|`$ne`|Not equals|`{grade: {$ne: "A"}}`|
|`$gt`|Greater than|`{age: {$gt: 20}}`|
|`$gte`|Greater than or equal|`{age: {$gte: 22}}`|
|`$lt`|Less than|`{age: {$lt: 23}}`|
|`$lte`|Less than or equal|`{age: {$lte: 22}}`|

**Example:**

```js
db.students.find({age: {$gte: 22}})
```

---

### **Logical Operators**

|Operator|Meaning|Example|
|---|---|---|
|`$and`|AND|`{$and: [{age: {$gte: 20}}, {grade: "A"}]}`|
|`$or`|OR|`{$or: [{grade: "A"}, {grade: "B"}]}`|
|`$not`|NOT|`{grade: {$not: {$eq: "A"}}}`|
|`$nor`|NOR|`{$nor: [{grade: "A"}, {age: 22}]}`|

**Example:**

```js
db.students.find({$or: [{grade: "A"}, {age: 22}]})
```

---

### **Query Arrays**

|Operator|Meaning|Example|
|---|---|---|
|`$in`|Value exists in array|`{grade: {$in: ["A","B"]}}`|
|`$nin`|Value not in array|`{grade: {$nin: ["C"]}}`|
|`$all`|Must contain all array elements|`{skills: {$all: ["Node.js", "MongoDB"]}}`|
|`$size`|Array size|`{skills: {$size: 3}}`|

**Example:**

```js
db.students.find({skills: {$all: ["JavaScript", "MongoDB"]}})
```

---

### **Query Nested Documents**

```js
db.students.find({"address.city": "Delhi"})
```

- Dot notation `.` allows querying **nested fields**.
    

---

### **Query Using Regular Expressions**

```js
db.students.find({name: /^A/})  // Names starting with 'A'
db.students.find({name: /rahul/i}) // Case-insensitive search
```

---

### **Projection (Select Fields Only)**

- Use projection to **return only required fields**.
    

```js
db.students.find({grade: "A"}, {name: 1, age: 1, _id: 0})
```

Output:

`{ name: "Alice", age: 21 }`

---

### **Sort Results**

`db.students.find().sort({age: 1})   // Ascending db.students.find().sort({age: -1})  // Descending`

---

### **Limit & Skip (Pagination)**

`db.students.find().limit(3)   // First 3 documents db.students.find().skip(2)    // Skip first 2 documents db.students.find().skip(2).limit(3)  // Skip 2, next 3`

---

### **Count Documents**

`db.students.find({grade: "A"}).count()`

- Returns number of documents matching query
    

---

### **Explain Query Performance (Optional)**

`db.students.find({age: {$gte: 22}}).explain("executionStats")`

- Helps **analyze performance**, useful with large collections
    

---

### **Query on Dates**

`db.students.find({joinedAt: {$gte: ISODate("2025-01-01T00:00:00Z")}})`

- Works with **Date / ISODate** fields
    

---

## ✅ **Summary Table: Read / Query Commands**

|Task|Command|
|---|---|
|Find all documents|`db.collection.find()`|
|Find one document|`db.collection.findOne(query)`|
|Filter by field|`db.collection.find({field: value})`|
|Comparison operators|`$eq, $ne, $gt, $gte, $lt, $lte`|
|Logical operators|`$and, $or, $not, $nor`|
|Query arrays|`$in, $nin, $all, $size`|
|Query nested fields|`{"field.subfield": value}`|
|Regular expressions|`{field: /pattern/}`|
|Projection (select fields)|`{field1:1, field2:1, _id:0}`|
|Sort|`.sort({field:1/-1})`|
|Limit & Skip|`.limit(n)`, `.skip(n)`|
|Count|`.count()`|
|Query Dates|`{$gte: ISODate("...")}`|

---

💡 **Practice Tips:**

1. Create 10-15 documents with:
    
    - Nested objects (`address`, `contact`)
        
    - Arrays (`skills`, `subjects`)
        
    - Dates (`joinedAt`)
        
2. Try queries using:
    
    - Comparison + Logical operators
        
    - Array operators (`$in`, `$all`)
        
    - Projection & Sorting