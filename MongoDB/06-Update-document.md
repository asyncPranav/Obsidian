
---

# **Chapter 4: Update Documents in MongoDB (U in CRUD)**

MongoDB provides **flexible update operations** to modify documents in a collection. Updates can be **single**, **multiple**, **nested**, or **array-specific**.

---

## **1️⃣ Update One Document (`updateOne`)**

**Syntax:**

```js
db.collection_name.updateOne(
  { <filter> },
  { <update_operator> }
)
```

- Updates **first document matching the filter**.
    
- Requires **update operator** like `$set`, `$inc`, etc.
    

**Example:**

```js
db.students.updateOne(
  { name: "Alice" },
  { $set: { age: 22 } }
)
```

- Changes Alice’s `age` to 22.
    
- Only **first matching document** is updated.
    

---

## **2️⃣ Update Multiple Documents (`updateMany`)**

**Syntax:**

```js
db.collection_name.updateMany(
  { <filter> },
  { <update_operator> }
)
```

**Example:**

```js
db.students.updateMany(
  { grade: "B" },
  { $set: { grade: "B+" } }
)
```

- Updates **all students with grade B** to B+.
    

---

## **3️⃣ Replace a Document (`replaceOne`)**

**Syntax:**

```js
db.collection_name.replaceOne(
  { <filter> },
  { <new_document> }
)
```

**Example:**

```js
db.students.replaceOne(
  { name: "Alice" },
  { name: "Alice", age: 23, grade: "A+" }
)
```

- Replaces the **entire document**.
    
- Fields not specified are **removed**.
    

> ⚠ Difference from `$set`: `$set` updates specific fields, `replaceOne` replaces the whole document.

---

## **4️⃣ Update Operators**

### **(A) `$set`**

- Sets value of a field, creates it if not exists
    

```js
db.students.updateOne({name: "Bob"}, {$set: {age: 23, city: "Delhi"}})
```

---

### **(B) `$inc`**

- Increments (or decrements) a numeric field
    

```js
db.students.updateOne({name: "Bob"}, {$inc: {age: 1}})
```

- Decreases: `{$inc: {age: -1}}`
    

---

### **(C) `$mul`**

- Multiplies numeric field
    

```js
db.students.updateOne({name: "Charlie"}, {$mul: {score: 2}})
```

---

### **(D) `$rename`**

- Rename a field
    

```js
db.students.updateOne({name: "Alice"}, {$rename: {"grade": "class"}})
```

---

### **(E) `$unset`**

- Removes a field
    

```js
db.students.updateOne({name: "Alice"}, {$unset: {city: ""}})
```

---

### **(F) `$min` and `$max`**

- Updates field **only if new value is smaller/larger**
    

```js
db.students.updateOne({name: "Bob"}, {$min: {age: 20}})
db.students.updateOne({name: "Bob"}, {$max: {age: 25}})
```

---

### **(G) `$currentDate`**

- Updates a field to **current date/time**
    

```js
db.students.updateOne({name: "Alice"}, {$currentDate: {lastModified: true}})
```

- Creates `lastModified` as `Date`.
    

---

## **5️⃣ Upsert (Update or Insert)**

- If **document doesn’t exist**, insert a new one
    

```js
db.students.updateOne(
  { name: "Zara" },
  { $set: {age: 20, grade: "A"} },
  { upsert: true }
)
```

- Creates Zara if she doesn’t exist.
    

---

## **6️⃣ Update Nested Documents**

- Use **dot notation** to update nested fields:
    

```js
db.students.updateOne(
  { name: "Alice" },
  { $set: {"address.city": "Mumbai"} }
)
```

- If `address` object exists → updates `city`.
    
- If not → may create it if parent object exists.
    

---

## **7️⃣ Update Arrays**

### **(A) Update element by index**

```js
db.students.updateOne(
  { name: "Bob" },
  { $set: {"skills.0": "Node.js"} }
)
```

- Updates **first element** of `skills` array.
    

---

### **(B) Add element to array**

```js
db.students.updateOne(
  { name: "Bob" },
  { $push: {skills: "MongoDB"} }
)
```

- Adds element to **end of array**
    

---

### **(C) Add multiple elements**

```js
db.students.updateOne(
  { name: "Bob" },
  { $push: {skills: {$each: ["React", "Express"]}} }
)
```

---

### **(D) Add unique element**

```js
db.students.updateOne(
  { name: "Bob" },
  { $addToSet: {skills: "Node.js"} }
)
```

- Adds only if it doesn’t exist
    

---

### **(E) Remove elements from array**

```js
db.students.updateOne(
  { name: "Bob" },
  { $pull: {skills: "React"} }
)
```

- Removes all occurrences of `React` from `skills`.
    

---

### **(F) Pop first or last element**

```js
db.students.updateOne(
  { name: "Bob" },
  { $pop: {skills: 1} }   // 1 = last element
)
db.students.updateOne(
  { name: "Bob" },
  { $pop: {skills: -1} }  // -1 = first element
)
```

---

## **8️⃣ Update Many Arrays (Multiple Docs)**

```js
db.students.updateMany(
  { grade: "B" },
  { $set: {"status": "Active"} }
)
```

- Updates **all matching documents**.
    

---

## **9️⃣ Upsert + Nested + Array Example**

```js
db.students.updateOne(
  { name: "Zara" },
  { 
    $set: {"age": 20, "address.city": "Delhi"},
    $push: {skills: {$each: ["JavaScript","MongoDB"]}}
  },
  { upsert: true }
)
```

- Combines **nested updates, array updates, and upsert**.
    

---

## ✅ **Summary Table: Update Operations**

|Task|Command / Operator|Notes|
|---|---|---|
|Update first match|`updateOne(filter, update)`|Only first matching document|
|Update multiple matches|`updateMany(filter, update)`|Updates all matches|
|Replace document|`replaceOne(filter, newDoc)`|Replaces entire document|
|Set / Create field|`$set`|Creates field if not exists|
|Increment / Decrement|`$inc`|Numeric field|
|Multiply|`$mul`|Multiply numeric field|
|Remove field|`$unset`|Deletes field|
|Rename field|`$rename`|Renames existing field|
|Min / Max|`$min` / `$max`|Conditional update|
|Current date|`$currentDate`|Sets field to current date/time|
|Upsert|`{upsert: true}`|Insert if not exists|
|Update nested|`dot notation`|`{"address.city": "Mumbai"}`|
|Push to array|`$push`|Add element(s)|
|Add unique to array|`$addToSet`|Adds only if not exists|
|Pull from array|`$pull`|Removes matching element|
|Pop array element|`$pop`|Remove first / last element|

---

💡 **Practice Tips:**

1. Try updating **nested fields** (`address`, `contact`)
    
2. Practice **array updates**: push, pull, addToSet
    
3. Use **upsert** to handle missing documents
    
4. Use **updateMany** for batch updates