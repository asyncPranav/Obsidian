

---

# **JSON Schema Validation in MongoDB (Complete Notes)**

## **1️⃣ What is JSON Schema Validation?**

MongoDB is **schema-less**, but sometimes we want **rules** for documents (like SQL table structure).

👉 **JSON Schema Validation** allows you to:

- Control **which fields are allowed**
    
- Enforce **data types**
    
- Make fields **required**
    
- Set **min/max values**
    
- Prevent wrong or invalid data insertion
    

MongoDB uses **JSON Schema (draft-07 style)** for validation.

---

## **2️⃣ Why do we need Schema Validation?**

Without validation:

`db.students.insertOne({name: "Aman", age: "twenty"})`

❌ `age` is string instead of number → MongoDB accepts it

With validation:

- MongoDB **rejects invalid documents**
    
- Ensures **data consistency**
    
- Reduces bugs in backend apps
    

---

## **3️⃣ Where Schema Validation is Applied?**

Schema validation is applied at:

- **Collection level**
    

You define rules:

- While **creating a collection**
    
- OR later using **`collMod`**
    

---

## **4️⃣ Create Collection with JSON Schema Validation**

### **Basic Syntax**

`db.createCollection("collection_name", {   validator: {     $jsonSchema: {       bsonType: "object",       required: [ "field1", "field2" ],       properties: {         field1: { bsonType: "string" },         field2: { bsonType: "int" }       }     }   } })`

---

### **Example: Students Collection**

`db.createCollection("students", {   validator: {     $jsonSchema: {       bsonType: "object",       required: ["name", "age"],       properties: {         name: {           bsonType: "string",           description: "must be a string and is required"         },         age: {           bsonType: "int",           minimum: 18,           maximum: 30,           description: "must be an integer between 18 and 30"         }       }     }   } })`

---

### ✅ Valid Insert

`db.students.insertOne({name: "Rahul", age: 22})`

### ❌ Invalid Insert

`db.students.insertOne({name: "Rahul", age: "22"})`

MongoDB Error:

`Document failed validation`

---

## **5️⃣ Important JSON Schema Keywords**

### **(A) `bsonType`**

Defines data type.

|Type|Meaning|
|---|---|
|`"string"`|String|
|`"int"`|Integer|
|`"double"`|Decimal|
|`"bool"`|Boolean|
|`"object"`|Document|
|`"array"`|Array|
|`"date"`|Date|
|`"objectId"`|ObjectId|

Example:

`age: { bsonType: "int" }`

---

### **(B) `required`**

Defines **mandatory fields**.

`required: ["name", "email"]`

❌ Missing required field → insert fails

---

### **(C) `properties`**

Defines rules for each field.

`properties: {   name: { bsonType: "string" },   age: { bsonType: "int" } }`

---

### **(D) `minimum` and `maximum`**

Used for numbers.

`age: {   bsonType: "int",   minimum: 18,   maximum: 60 }`

---

### **(E) `minLength` and `maxLength`**

Used for strings.

`username: {   bsonType: "string",   minLength: 3,   maxLength: 15 }`

---

### **(F) `enum`**

Restrict values to a fixed list.

`gender: {   enum: ["male", "female", "other"] }`

---

### **(G) `pattern`**

Used for regex validation (emails, phone numbers).

`email: {   bsonType: "string",   pattern: "^.+@.+\\..+$" }`

---

## **6️⃣ Validate Nested Documents**

### **Example: Address Object**

`address: {   bsonType: "object",   required: ["city", "pincode"],   properties: {     city: { bsonType: "string" },     pincode: { bsonType: "string" }   } }`

Insert:

`db.students.insertOne({   name: "Amit",   age: 21,   address: { city: "Delhi", pincode: "110001" } })`

---

## **7️⃣ Validate Arrays**

### **Example: Subjects Array**

`subjects: {   bsonType: "array",   items: {     bsonType: "string"   } }`

Valid:

`subjects: ["Math", "Physics"]`

Invalid:

`subjects: ["Math", 101]`

---

## **8️⃣ Full Real-World Schema Example**

`db.createCollection("users", {   validator: {     $jsonSchema: {       bsonType: "object",       required: ["name", "email", "age"],       properties: {         name: {           bsonType: "string",           minLength: 3         },         email: {           bsonType: "string",           pattern: "^.+@.+\\..+$"         },         age: {           bsonType: "int",           minimum: 18         },         isActive: {           bsonType: "bool"         },         roles: {           bsonType: "array",           items: { bsonType: "string" }         }       }     }   } })`

---

## **9️⃣ Modify Validation of Existing Collection**

Use **`collMod`**:

`db.runCommand({   collMod: "students",   validator: {     $jsonSchema: {       bsonType: "object",       required: ["name", "age"]     }   } })`

---

## **🔟 Validation Levels**

MongoDB provides **2 validation levels**:

### **(A) `validationLevel`**

|Level|Meaning|
|---|---|
|`strict`|Applies to all inserts & updates|
|`moderate`|Applies only to new documents|

`validationLevel: "strict"`

---

### **(B) `validationAction`**

|Action|Behavior|
|---|---|
|`error`|Reject invalid document|
|`warn`|Allow insert but log warning|

`validationAction: "error"`

---

## **1️⃣1️⃣ Complete Syntax with Level & Action**

`db.createCollection("products", {   validator: {     $jsonSchema: {       bsonType: "object",       required: ["name", "price"],       properties: {         name: { bsonType: "string" },         price: { bsonType: "double", minimum: 0 }       }     }   },   validationLevel: "strict",   validationAction: "error" })`

---

## ✅ **Advantages of JSON Schema Validation**

- Prevents invalid data
    
- Improves backend reliability
    
- Brings SQL-like discipline
    
- Easy to read & maintain
    
- Works directly in MongoDB
    

---

## ❌ **Limitations**

- Validation rules apply **only at DB level**
    
- More strict rules → less flexibility
    
- Complex schemas can become hard to manage
    

---

## 📌 **Chapter Summary**

You now know:

- What JSON Schema Validation is
    
- Why it’s needed
    
- How to apply it
    
- All important keywords
    
- Nested objects & arrays
    
- Modify existing schema
    
- Validation levels & actions