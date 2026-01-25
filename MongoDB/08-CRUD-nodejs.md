
---

```js
const { MongoClient } = require("mongodb");
const uri =  "mongodb+srv://namasteNode:namasteNodePass@namastenode.jqcz9u9.mongodb.net/?appName=namasteNode";

const client = new MongoClient(uri);

async function main() {
  await client.connect();
  console.log("MongoDB connected");
  
  const db = client.db("HelloWorld");
  const users = db.collection("User");

  // create
  console.log("\n--------------Creating user--------------\n");
  const created = await users.insertOne({
    name: "Vageesh",
    age: 34,
    course: "MCA",
  });

  

  console.log("acknowledged : ", created.acknowledged);

  console.log("insertedId : ", created.insertedId);

  
  

  // read

  console.log("\n--------------Reading Users--------------\n");

  const all = await users.find().toArray();

  console.log(all);

  

  // update

  console.log("\n--------------Updating User--------------\n");

  // await users.updateOne({ name: "Atul" }, { $set: { name: "Ayush" } });

  const result = await users.updateOne(

    { name: "Vageesh" },

    { $set: { name: "Atul" } },

  );

  

  console.log("matchFound : ", result.matchedCount); // how many found

  console.log("modifiedCount : ", result.modifiedCount); // how many updated

  

  // print final all users

  console.log("\n--------------Final All Users--------------\n");

  console.log(await users.find().toArray());

  

  // delete

  console.log("\n--------------Deleting user--------------\n");

  const deleted = await users.deleteOne({ name: "Atul" });

  console.log("acknowledged : ", deleted.acknowledged);

  console.log("deletedCount :", deleted.deletedCount);

  

  client.close();

}

  

main();
```


## 1️⃣ Import MongoClient from mongodb package

```js
const { MongoClient } = require("mongodb");
```

### What is happening here?

#### Step 1: `require("mongodb")`

- Node.js looks inside `node_modules`
    
- Finds the **mongodb driver package**
    
- Loads its exported objects into memory
    

#### Step 2: `{ MongoClient }`

- This is **object destructuring**
    
- Equivalent to:
    
```js
const MongoClient = require("mongodb").MongoClient;
```
    

### What is `MongoClient`?

- A **class** provided by MongoDB
    
- Responsible for:
    
    - Opening network connections
        
    - Authentication
        
    - Connection pooling
        
    - Sending queries to MongoDB server
        

📌 Think of `MongoClient` as:

> 🧑‍💼 A manager that handles all communication with MongoDB

---

## 2️⃣ MongoDB Connection URI

```js
const uri =
"mongodb+srv://namasteNode:namasteNodePass@namastenode.jqcz9u9.mongodb.net/?appName=namasteNode";
```

### What is a URI?

URI = **Uniform Resource Identifier**

It tells Node:

- WHERE MongoDB is
    
- HOW to connect
    
- WHO is connecting
    

---

### Breaking this URI into parts

```js
mongodb+srv://
```

- Protocol
    
- `+srv` means:
    
    - DNS-based discovery
        
    - Multiple MongoDB servers (replica set)
        
    - Used for **MongoDB Atlas (cloud)**
        

---

```js
namasteNode
```

- MongoDB **username**
    

---

```js
namasteNodePass
```

- MongoDB **password**
    

⚠️ In real apps → NEVER hardcode credentials

---

```js
@namastenode.jqcz9u9.mongodb.net
```

- Cluster hostname
    
- Points to MongoDB Atlas servers
    

---

```js
/?appName=namasteNode
```

- Optional query parameters
    
- Used for logging & monitoring
    

---

📌 This URI is like:

> 🏠 Full home address + door key

---

## 3️⃣ Create MongoClient Instance

```js
const client = new MongoClient(uri);
```

### IMPORTANT: This does NOT connect yet ❌

What it does:

- Creates a **MongoClient object**
    
- Stores:
    
    - URI
        
    - Connection configuration
        
- Prepares for future connection
    

---

### Analogy

`new MongoClient(uri)`

is like:

> Buying a phone 📱  
> (not calling yet)

---

## 4️⃣ Async Function `main`

`async function main() {`

### Why async?

Because:

- MongoDB operations involve **network I/O**
    
- Network I/O is **asynchronous**
    
- MongoDB driver returns **Promises**
    

`async` allows:

`await client.connect();`

---

### What if we don’t use async?

You would need:

`client.connect().then(...).catch(...)`

---

## 5️⃣ Connect to MongoDB

`await client.connect();`

### This is the MOST IMPORTANT LINE

#### What happens internally (simplified):

1. DNS lookup for cluster
    
2. TCP socket opened
    
3. TLS encryption handshake
    
4. Authentication with MongoDB
    
5. Connection pool created
    

⏱ This can take **100–500 ms**

---

### Why `await`?

- Node pauses `main()` execution
    
- Does NOT block event loop
    
- Waits until connection succeeds
    

---

## 6️⃣ Log Confirmation

`console.log("MongoDB connected");`

This line runs **only after**:  
✔ Connection successful

If connection fails → this line never runs.

---

## 7️⃣ Select Database

`const db = client.db("HelloWorld");`

### IMPORTANT CONCEPT

- MongoDB does NOT require database creation
    
- Database is created **lazily**
    

Meaning:

- If `"HelloWorld"` does not exist
    
- MongoDB creates it on first write
    

---

### What `db` is:

- A **Database object**
    
- Contains methods to:
    
    - Access collections
        
    - Run commands
        

---

### Mental model

`client → MongoDB Server db     → One database inside server`

---

## 8️⃣ Select Collection

`const users = db.collection("User");`

### Again: NO collection created yet ❌

This only:

- Gets a reference to collection
    
- Prepares for CRUD operations
    

---

### When is collection actually created?

👉 When you insert the **first document**

---

### What `users` is:

- A **Collection object**
    
- Exposes:
    
    - `insertOne`
        
    - `find`
        
    - `updateOne`
        
    - `deleteOne`
        

---

## 🧠 FINAL EXECUTION FLOW (VERY IMPORTANT)

`require mongodb    ↓ create MongoClient object    ↓ connect to MongoDB server    ↓ select database    ↓ select collection`

---

## ✅ KEY TAKEAWAYS (REMEMBER THIS)

✔ `require()` loads code  
✔ `MongoClient` manages connections  
✔ URI defines server + credentials  
✔ `new MongoClient()` does NOT connect  
✔ `client.connect()` opens network connection  
✔ DB & collection are created lazily