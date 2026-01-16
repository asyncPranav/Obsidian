
---

# 📦 Node.js `stream` Module — COMPLETE NOTES 

---

## 1️⃣ What is a Stream? (Very Simple)

### 📌 Definition

A **stream** is a way to **read or write data piece by piece (chunks)** instead of loading everything at once.

### 🧠 Real-life example

Imagine watching a YouTube video:

❌ Bad way (no stream)  
→ Download whole video first  
→ Then play

✅ Good way (stream)  
→ Download small chunks  
→ Play while downloading

👉 Node.js streams work like this.

---

## 2️⃣ Why Streams are IMPORTANT?

### ❌ Without streams

```js
fs.readFile("bigfile.mp4", (err, data) => {
  // loads entire file into memory
});
```

Problems:

- High memory usage
    
- Slow
    
- App can crash for large files
    

### ✅ With streams

```js
fs.createReadStream("bigfile.mp4");
```

Benefits:

- Low memory usage
    
- Faster
    
- Efficient
    
- Scales well
    

---

## 3️⃣ Types of Streams in Node.js

Node.js has **4 types of streams** 👇

```txt
1. Readable  → Read data
2. Writable  → Write data
3. Duplex    → Read + Write
4. Transform → Modify data while reading/writing
```

---

## 4️⃣ Readable Stream (MOST IMPORTANT)

### 📌 What is Readable Stream?

Used to **read data chunk by chunk**.

### Examples:

- Reading a file
    
- Incoming HTTP request
    
- Reading data from DB
    

---

### 🔹 Create Readable Stream (File)

```js
const fs = require("fs");

const readStream = fs.createReadStream("file.txt");
```

---

### 🔹 Important Events of Readable Stream

#### 1️⃣ `data` event

Fires when a chunk is available.

```js
readStream.on("data", (chunk) => {
  console.log(chunk);
});
```

---

#### 2️⃣ `end` event

Fires when reading is finished.

```js
readStream.on("end", () => {
  console.log("File reading finished");
});
```

---

#### 3️⃣ `error` event

Fires if error occurs.

```js
readStream.on("error", (err) => {
  console.log(err);
});
```

---

### 🔹 Complete Readable Example

```js
const fs = require("fs");

const readStream = fs.createReadStream("file.txt", "utf8");

readStream.on("data", (chunk) => {
  console.log("Chunk:", chunk);
});

readStream.on("end", () => {
  console.log("Reading completed");
});
```

### 📤 Output (example)

```sh
Chunk: Hello
Chunk: World
Reading completed
```

---

## 5️⃣ Writable Stream

### 📌 What is Writable Stream?

Used to **write data chunk by chunk**.

### Examples:

- Writing to a file
    
- Sending HTTP response
    

---

### 🔹 Create Writable Stream

`const fs = require("fs");  const writeStream = fs.createWriteStream("output.txt");`

---

### 🔹 Writing Data

`writeStream.write("Hello "); writeStream.write("World");`

---

### 🔹 Ending the Stream

`writeStream.end();`

---

### 🔹 Events in Writable Stream

#### 1️⃣ `finish`

Triggered when writing is done.

`writeStream.on("finish", () => {   console.log("Writing finished"); });`

---

### 🔹 Complete Writable Example

`const fs = require("fs");  const writeStream = fs.createWriteStream("output.txt");  writeStream.write("Node "); writeStream.write("Streams");  writeStream.end();  writeStream.on("finish", () => {   console.log("Data written successfully"); });`

---

## 6️⃣ pipe() — MOST IMPORTANT METHOD ⭐⭐⭐⭐⭐

### 📌 What is `pipe()`?

`pipe()` connects **Readable → Writable** automatically.

### ❌ Without pipe

You manually handle chunks.

### ✅ With pipe

Node handles everything.

---

### 🔹 Syntax

`readableStream.pipe(writableStream);`

---

### 🔹 File Copy Example (BEST EXAMPLE)

`const fs = require("fs");  const readStream = fs.createReadStream("source.txt"); const writeStream = fs.createWriteStream("dest.txt");  readStream.pipe(writeStream);`

✔ Efficient  
✔ Fast  
✔ Low memory

---

## 7️⃣ Duplex Stream

### 📌 What is Duplex Stream?

Can **read and write both**.

### Examples:

- TCP socket
    
- WebSocket
    

---

### 🔹 Example (basic concept)

`const { Duplex } = require("stream");  const duplex = new Duplex({   read(size) {     this.push("Hello");     this.push(null);   },   write(chunk, encoding, callback) {     console.log(chunk.toString());     callback();   } });  duplex.on("data", data => console.log(data.toString())); duplex.write("World");`

---

## 8️⃣ Transform Stream (VERY IMPORTANT)

### 📌 What is Transform Stream?

A special Duplex stream that **modifies data**.

### Examples:

- Compression
    
- Encryption
    
- Uppercase / lowercase
    
- JSON parsing
    

---

### 🔹 Example: Convert to Uppercase

`const { Transform } = require("stream");  const upperCaseTransform = new Transform({   transform(chunk, encoding, callback) {     this.push(chunk.toString().toUpperCase());     callback();   } });  process.stdin.pipe(upperCaseTransform).pipe(process.stdout);`

---

### 🧠 Flow

`Input → Transform → Output`

---

## 9️⃣ Backpressure (IMPORTANT CONCEPT)

### 📌 What is Backpressure?

When **Writable stream is slow** and **Readable stream is fast**.

### Example:

- Fast file read
    
- Slow network write
    

Node handles this automatically when using `pipe()`.

👉 **This is why pipe() is recommended**

---

## 🔟 Stream States

Readable stream states:

- flowing
    
- paused
    
- ended
    

Writable stream states:

- writing
    
- finished
    

---

## 1️⃣1️⃣ Stream vs Buffer vs fs.readFile

|Feature|fs.readFile|Buffer|Stream|
|---|---|---|---|
|Memory|High|Medium|Low|
|Speed|Slow|Medium|Fast|
|Large files|❌|❌|✅|

---

## 1️⃣2️⃣ Streams in HTTP (IMPORTANT)

### Sending file using stream

`const http = require("http"); const fs = require("fs");  http.createServer((req, res) => {   const stream = fs.createReadStream("file.txt");   stream.pipe(res); }).listen(3000);`

✔ Fast  
✔ Efficient  
✔ Production ready

---

## 1️⃣3️⃣ Important Stream Methods Summary

### Readable

- `.on("data")`
    
- `.on("end")`
    
- `.pipe()`
    
- `.resume()`
    
- `.pause()`
    

### Writable

- `.write()`
    
- `.end()`
    
- `.on("finish")`
    

### Transform

- `transform(chunk, enc, cb)`
    

---

## 🧠 One-Line Memory Tricks

- **Stream** → Data in chunks
    
- **pipe()** → Best way to handle streams
    
- **Readable** → Read data
    
- **Writable** → Write data
    
- **Transform** → Change data
    
- **Backpressure** → Auto-handled by pipe()
    

---

## 🎯 Interview One-Liners

- Streams handle large data efficiently by processing chunks instead of loading whole data into memory.
    
- `pipe()` manages backpressure automatically.
    
- Transform streams modify data during streaming.