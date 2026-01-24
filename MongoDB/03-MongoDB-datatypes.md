

---


# **MongoDB Data Types (Detailed Notes)**

MongoDB stores data in **BSON (Binary JSON)** format.  
BSON extends JSON by adding **more data types**, better performance, and storage efficiency.

---

## **1️⃣ Why MongoDB Uses BSON?**

JSON limitations:

- No date type
    
- No binary type
    
- No distinction between int & float
    

BSON advantages:

- Faster traversal
    
- Supports more data types
    
- Efficient storage
    
- Supports indexing
    

---

## **2️⃣ Categories of MongoDB Data Types**

MongoDB data types are divided into:

1. **Primitive / Basic Types**
    
2. **Numeric Types**
    
3. **Date & Time Types**
    
4. **Complex / Structural Types**
    
5. **Special Types**
    
6. **Deprecated Types**
    

---

## **3️⃣ Primitive / Basic Data Types**

### **(A) String**

- Stores text values
    
- UTF-8 encoded
    

```js
{name: "Rahul"}
```

Used for:

- Names
    
- Emails
    
- Addresses
    

---

### **(B) Boolean**

- Stores true or false
    

```js
{isActive: true}
```

---

### **(C) Null**

- Represents missing or empty value
    

```js
{middleName: null}
```

---

## **4️⃣ Numeric Data Types (Very Important)**

### **(A) Integer (`int`)**

- 32-bit integer
    
- Range: **-2³¹ to 2³¹-1**
    

```js
{age: NumberInt(25)}
```

📌 Default numbers may be stored as **double**, so use `NumberInt()` when needed.

---

### **(B) Long (`long`)**

- 64-bit integer
    
- Used for large numbers
    

```js
{population: NumberLong("9000000000")}
```

---

### **(C) Double**

- Floating-point number
    
- Default numeric type
    

```js
{price: 99.99}
```

---

### **(D) Decimal128**

- High-precision decimal
    
- Used for **money, financial data**
    

```js
{salary: NumberDecimal("12345.67")}
```

✔ Prevents floating-point rounding issues

---

## **5️⃣ Date & Time Data Types**

### **(A) Date**

- Stores date & time (milliseconds since Unix epoch)
    

```js
{createdAt: new Date()}
```

Example:

```js
db.users.insertOne({name: "Amit", joinedAt: new Date()})
```

---

### **(B) Timestamp**

- Used internally for replication & oplog
    

```js
{ts: Timestamp()}
```

⚠ Rarely used in applications

---

## **6️⃣ Complex / Structural Data Types**

### **(A) Object (Embedded Document)**

```js
{
  address: {
    city: "Delhi",
    pincode: "110001"
  }
}
```

✔ Enables nested data  
✔ Avoids joins

---

### **(B) Array**

```js
{skills: ["JavaScript", "Node.js", "MongoDB"]}
```

Arrays can contain:

- Same type
    
- Mixed types
    
- Objects
    

```js
{items: [{name: "Pen"}, {name: "Book"}]}
```

---

## **7️⃣ Special Data Types**

### **(A) ObjectId**

- Default unique identifier for documents
    
- 12-byte value
    

```js
{_id: ObjectId("65a9c0c4e8f1a1b2c3d4e5f6")}
```

Structure:

- Timestamp
    
- Machine ID
    
- Process ID
    
- Counter
    

---

### **(B) Binary Data**

- Stores images, files, blobs
    

```js
{file: BinData(0, "U29tZUJpbmFyeURhdGE=")}
```

---

### **(C) Regular Expression**

```js
{name: /rahul/i}
```

Used in search queries.

---

### **(D) JavaScript Code**

```js
{calc: function() { return 2 + 2 }}
```

⚠ Rarely used (security & performance reasons)

---

### **(E) MinKey & MaxKey**

```js
{min: MinKey()}
{max: MaxKey()}
```

Used for:

- Comparison
    
- Sorting
    
- Index bounds
    

---

## **8️⃣ Deprecated / Rare Data Types**

### **(A) Symbol** (Deprecated)

```js
Symbol("abc")
```

---

### **(B) Undefined** (Deprecated)

```js
{value: undefined}
```

❌ Avoid using

---

## **9️⃣ Type Checking in MongoDB**

### **Check type using `$type`**

```js
db.users.find({age: {$type: "int"}})
```

OR numeric code:

```js
db.users.find({age: {$type: 16}})
```

---

## **🔟 MongoDB Type Codes (Important for Interviews)**

|Type|Code|
|---|---|
|Double|1|
|String|2|
|Object|3|
|Array|4|
|Binary|5|
|ObjectId|7|
|Boolean|8|
|Date|9|
|Null|10|
|Int|16|
|Long|18|
|Decimal|19|

---

## **1️⃣1️⃣ Use in JSON Schema Validation**

```js
age: { bsonType: "int" }
price: { bsonType: "decimal" }
createdAt: { bsonType: "date" }
```

---

## **1️⃣2️⃣ Best Practices**

✅ Use:

- `Decimal128` for money
    
- `Date` for timestamps
    
- `ObjectId` default `_id`
    

❌ Avoid:

- Mixing number types
    
- Deprecated types
    
- JavaScript code type
    

---

## **📌 Final Summary**

- MongoDB uses **BSON**
    
- Supports rich data types
    
- Numeric types matter a lot
    
- Data types directly affect:
    
    - Indexing
        
    - Performance
        
    - Validation
        
    - Query accuracy