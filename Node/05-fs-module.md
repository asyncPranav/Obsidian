
---

# 📁 Node.js `fs` Module – MOST IMPORTANT METHODS (FULL NOTES)

```js
const fs = require("fs");
```

---

## 1️⃣ `fs.readFile()`

### 📘 Theory

Reads the **entire content of a file asynchronously**.  
It does **not block the event loop**.  
If encoding is not provided, it returns a **Buffer**.

---

### 🧠 Syntax

```js
fs.readFile(path, [options], callback)
```

### Parameters

|Parameter|Required|Description|
|---|---|---|
|`path`|✅|File path|
|`options`|❌|Encoding or options object|
|`callback`|✅|`(err, data)`|

**options (optional):**

```js
"utf8"
```

OR

```js
{ encoding: "utf8", flag: "r" }
```

---

### 💻 Code Example

```js
fs.readFile("data.txt", "utf8", (err, data) => {
  if (err) {
    console.log(err);
    return;
  }
  console.log("File Content:", data);
});

console.log("Reading file...");
```

### 🖥 Output

```sh
Reading file...
File Content: Hello Node.js
```

---

## 2️⃣ `fs.writeFile()`

### 📘 Theory

Creates a **new file or overwrites an existing file** with given data.

---

### 🧠 Syntax

```js
fs.writeFile(file, data, [options], callback)
```

### Parameters

|Parameter|Required|
|---|---|
|`file`|✅|
|`data`|✅|
|`options`|❌|
|`callback`|✅|

**options (optional):**

```js
{
  encoding: "utf8",
  mode: 0o666,
  flag: "w"
}
```

---

### 💻 Code Example

```js
fs.writeFile("write.txt", "Hello FS Module", (err) => {
  if (err) throw err;
  console.log("File written successfully");
});
```

### 🖥 Output

```sh
File written successfully
```

---

## 3️⃣ `fs.appendFile()`

### 📘 Theory

Adds data to the **end of an existing file** without removing old content.  
Mostly used for **logs**.

---

### 🧠 Syntax

```js
fs.appendFile(path, data, [options], callback)
```

---

### 💻 Code Example

```js
fs.appendFile("write.txt", "\nNew log entry", (err) => {
  if (err) throw err;
  console.log("Data appended");
});
```

### 🖥 Output

```sh
Data appended
```

---

## 4️⃣ `fs.unlink()`

### 📘 Theory

Deletes a file **permanently** from the file system.

---

### 🧠 Syntax

```js
fs.unlink(path, callback)
```

---

### 💻 Code Example

```js
fs.unlink("delete.txt", (err) => {
  if (err) throw err;
  console.log("File deleted");
});
```

### 🖥 Output

```sh
File deleted
```

---

## 5️⃣ `fs.mkdir()`

### 📘 Theory

Creates a **new directory (folder)**.  
Can create **nested directories** using `recursive: true`.

---

### 🧠 Syntax

```js
fs.mkdir(path, [options], callback)
```

**options (optional):**

```js
{
  recursive: false,
  mode: 0o777
}
```

---

### 💻 Code Example

```js
fs.mkdir("parent/child", { recursive: true }, (err) => {
  if (err) throw err;
  console.log("Folder created");
});
```

### 🖥 Output

```

```

---

## 6️⃣ `fs.readdir()`

### 📘 Theory

Reads the contents of a directory and returns a **list of files/folders**.

---

### 🧠 Syntax

`fs.readdir(path, [options], callback)`

**options (optional):**

`{   encoding: "utf8",   withFileTypes: false }`

---

### 💻 Code Example

`fs.readdir("./", (err, files) => {   if (err) throw err;   console.log(files); });`

### 🖥 Output

`[ 'app.js', 'data.txt', 'folder' ]`

---

## 7️⃣ `fs.existsSync()`

### 📘 Theory

Checks **synchronously** whether a file or folder exists.  
Returns `true` or `false`.

---

### 🧠 Syntax

`fs.existsSync(path)`

---

### 💻 Code Example

`if (fs.existsSync("data.txt")) {   console.log("File exists"); } else {   console.log("File not found"); }`

### 🖥 Output

`File exists`

---

## 8️⃣ `fs.stat()`

### 📘 Theory

Returns **metadata information** about a file or directory.

---

### 🧠 Syntax

`fs.stat(path, callback)`

---

### 💻 Code Example

`fs.stat("data.txt", (err, stats) => {   if (err) throw err;   console.log("Is File:", stats.isFile());   console.log("Size:", stats.size); });`

### 🖥 Output

`Is File: true Size: 120`

---

## 9️⃣ `fs.createReadStream()`

### 📘 Theory

Reads file data **in chunks using streams**.  
Best for **large files**.

---

### 🧠 Syntax

`fs.createReadStream(path, [options])`

---

### 💻 Code Example

`const stream = fs.createReadStream("bigfile.txt", "utf8");  stream.on("data", (chunk) => {   console.log("Chunk received:", chunk); });`

### 🖥 Output

`Chunk received: Hello Chunk received: Node Chunk received: JS`

---

## 🔟 `fs.createWriteStream()`

### 📘 Theory

Writes data to a file **chunk by chunk** using streams.

---

### 🧠 Syntax

`fs.createWriteStream(path, [options])`

---

### 💻 Code Example

`const writeStream = fs.createWriteStream("stream.txt"); writeStream.write("Hello "); writeStream.write("Streams"); writeStream.end();`

### 🖥 Output

`stream.txt file created with content`

---

# 🧠 EXAM / INTERVIEW GOLD POINTS

- Async `fs` → non-blocking
    
- Sync `fs` → blocks event loop
    
- Encoding missing → returns **Buffer**
    
- Streams → best for large files
    

---

# ⚡ FINAL QUICK REVISION TABLE

|Method|Purpose|
|---|---|
|readFile|Read full file|
|writeFile|Create/overwrite|
|appendFile|Add data|
|unlink|Delete file|
|mkdir|Create folder|
|readdir|List directory|
|existsSync|Check existence|
|stat|File info|
|createReadStream|Read large file|
|createWriteStream|Write large file|