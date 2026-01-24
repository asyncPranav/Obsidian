
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

## **1️⃣ `$push` – Add element(s) to the end of an array**

- Adds **one element** or **multiple elements** using `$each`.
    
- Can also use `$position`, `$sort`, `$slice`.
    

```js
// Single element
db.students.updateOne(
  { name: "Bob" },
  { $push: { skills: "MongoDB" } }
)

// Multiple elements
db.students.updateOne(
  { name: "Bob" },
  { $push: { skills: { $each: ["React", "Express"] } } }
)

// Insert at specific position
db.students.updateOne(
  { name: "Bob" },
  { $push: { skills: { $each: ["Python"], $position: 0 } } } // Insert at index 0
)

// Limit array size and sort
db.students.updateOne(
  { name: "Bob" },
  { $push: { scores: { $each: [80, 90], $slice: -5, $sort: -1 } } }
)
```

- `$slice` → keeps only **last N elements**
    
- `$sort` → sort array after inserting
    

---

## **2️⃣ `$addToSet` – Add only unique elements**

- Ensures **no duplicates** in array
    

```js
db.students.updateOne(
  { name: "Bob" },
  { $addToSet: { skills: "Node.js" } }
)

// Multiple unique elements
db.students.updateOne(
  { name: "Bob" },
  { $addToSet: { skills: { $each: ["Java", "C++"] } } }
)
```

---

## **3️⃣ `$pop` – Remove first or last element**

- `$pop: 1` → remove last element
    
- `$pop: -1` → remove first element
    

```js
db.students.updateOne({ name: "Bob" }, { $pop: { skills: 1 } })
db.students.updateOne({ name: "Bob" }, { $pop: { skills: -1 } })
```

---

## **4️⃣ `$pull` – Remove matching element(s)**

- Removes **all elements that match condition**
    

```js
db.students.updateOne(
  { name: "Bob" },
  { $pull: { skills: "React" } }
)

// Remove all elements greater than 80
db.students.updateOne(
  { name: "Bob" },
  { $pull: { scores: { $gt: 80 } } }
)
```

- `$pull` works with:
    
    - Exact values
        
    - Query conditions (`$gt`, `$lt`, `$in`, `$nin`, `$regex`)
        

---

## **5️⃣ `$pullAll` – Remove multiple specific values**

- Remove **multiple exact values** at once
    

```js
db.students.updateOne(
  { name: "Bob" },
  { $pullAll: { skills: ["React", "Python"] } }
)
```

---

## **6️⃣ `$push` + `$each` + `$position` + `$sort` + `$slice` (Advanced)**

- Combine options to:
    
    - Insert multiple items
        
    - Insert at custom index
        
    - Sort array
        
    - Limit array length
        

```js
db.students.updateOne(
  { name: "Bob" },
  { $push: { scores: { $each: [70, 85, 95], $position: 1, $sort: -1, $slice: 3 } } }
)
```

- Keeps **top 3 scores** sorted descending
    

---

## **7️⃣ `$` Positional Operator – Update matched array element**

- Update **first element that matches query condition**
    

```js
db.students.updateOne(
  { name: "Bob", "scores": 85 },
  { $set: { "scores.$": 90 } }
)
```

- `$` → updates **first matching element** of the array
    

---

## **8️⃣ `$[]` – All Array Elements (Update Many Elements)**

- Update **all elements** in an array
    

```js
db.students.updateOne(
  { name: "Bob" },
  { $inc: { "scores.$[]": 5 } } // Add 5 to all scores
)
```

---

## **9️⃣ `$[<identifier>]` – Filtered Positional Operator**

- Update **specific elements matching condition** in array
    

```js
db.students.updateOne(
  { name: "Bob" },
  { $set: { "scores.$[elem]": 100 } },
  { arrayFilters: [{ "elem": { $lt: 50 } }] }
)
```

- Updates only elements `<50` to `100`
    

---

## **10️⃣ `$slice` with `$push`**

- Keep array length limited
    

```js
db.students.updateOne(
  { name: "Bob" },
  { $push: { scores: { $each: [60,70], $slice: -5 } } } 
)
```

- Keeps only last 5 elements
    

---

## **Summary Table: Array Update Operators**

|Operator|Use|Example|
|---|---|---|
|`$push`|Add element(s) to end|`{ $push: {skills: "MongoDB"} }`|
|`$each`|Add multiple elements|`{ $push: {skills: {$each: ["React","Node"]}} }`|
|`$position`|Insert at index|`{ $push: {skills: {$each:["Python"], $position:0}} }`|
|`$sort`|Sort array after push|`{ $push: {scores: {$each:[70,80], $sort:-1}} }`|
|`$slice`|Limit array size|`{ $push: {scores: {$each:[70,80], $slice:-3}} }`|
|`$addToSet`|Add unique only|`{ $addToSet: {skills: "Node.js"} }`|
|`$pop`|Remove first/last|`{ $pop: {skills: 1} }`|
|`$pull`|Remove matching elements|`{ $pull: {skills: "React"} }`|
|`$pullAll`|Remove multiple exact|`{ $pullAll: {skills: ["React","Python"]} }`|
|`$`|Update first matching array element|`{ "scores.$": 90 }`|
|`$[]`|Update all array elements|`{ "scores.$[]": 5 }`|
|`$[<id>]`|Update filtered elements|`{ "scores.$[elem]": 100 }` with `arrayFilters`|

---

💡 **Practice Tips:**

1. Try adding, removing, and updating array elements in **different ways**
    
2. Use `$push + $each + $slice + $sort` → emulate **leaderboards**
    
3. Use `$[]` and `$[<identifier>]` → update **all or filtered array elements**

---


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