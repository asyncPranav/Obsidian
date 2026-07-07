
---


Great. Since you are learning `fs` (**File System**) module from scratch, we will build it step by step like a Node.js developer.

We will start with **Reading Files** and cover:

- What `fs` is
    
- Different ways to read files
    
- Syntax
    
- Parameters
    
- Return values
    
- Encoding
    
- Buffer
    
- Error handling
    
- Async vs Sync
    
- Real-world usage
    

---

# 1. What is `fs` module?

`fs` stands for **File System**.

Node.js provides the `fs` module to interact with the computer's file system.

Using `fs`, we can:

- Read files
    
- Create files
    
- Update files
    
- Delete files
    
- Rename files
    
- Create folders
    
- Remove folders
    
- Watch file changes
    

Example:

```
Node.js Application
        |
        |
        ↓
      fs Module
        |
        |
        ↓
 Computer File System
```

---

# 2. Importing fs module

## CommonJS (most Node.js tutorials use this)

```javascript
const fs = require("fs");
```

---

## ES Module

```javascript
import fs from "fs";
```

---

# 3. Reading a File

There are 3 ways:

## Method 1: Async Callback

```javascript
fs.readFile()
```

## Method 2: Promise Async

```javascript
fs.promises.readFile()
```

## Method 3: Sync

```javascript
fs.readFileSync()
```

We will understand each.

---

# PART 1: `fs.readFile()` (Callback)

This is the original asynchronous method.

---

## Syntax

```javascript
fs.readFile(path, options, callback)
```

Three arguments:

|Argument|Meaning|
|---|---|
|path|File location|
|options|Encoding/settings|
|callback|Function that runs after reading|

---

Example structure:

```javascript
fs.readFile(
    "file.txt",
    "utf-8",
    (error, data)=>{
        
    }
);
```

---

# Example

Folder:

```
project
│
├── app.js
└── data.txt
```

`data.txt`

```
Hello Node.js
```

---

`app.js`

```javascript
const fs = require("fs");


fs.readFile("data.txt", "utf-8", (err, data)=>{

    if(err){
        console.log(err);
        return;
    }

    console.log(data);

});


console.log("Program End");
```

---

# Execution Flow

Many beginners expect:

```
Read file
Print file
Program End
```

But Node.js is asynchronous.

Actual:

```
Program End

Hello Node.js
```

Why?

Because:

1. Node starts reading file
    
2. It does not wait
    
3. It continues executing next code
    
4. When file reading finishes, callback runs
    

---

# Callback Parameters

The callback receives:

```javascript
(err, data)
```

## 1. err

Contains error information.

Example:

File does not exist:

```
Error: ENOENT no such file
```

---

## 2. data

Contains file content.

Example:

```
Hello Node.js
```

---

# Without Encoding

Example:

```javascript
fs.readFile("data.txt",(err,data)=>{
    console.log(data);
});
```

Output:

```
<Buffer 48 65 6c 6c 6f>
```

Why?

Because Node reads files as **binary data** by default.

---

# What is Buffer?

A Buffer is temporary memory that stores raw binary data.

Computer stores:

```
H
e
l
l
o
```

as bytes:

```
48 65 6c 6c 6f
```

Node gives this as Buffer.

---

To convert Buffer:

```javascript
data.toString()
```

Example:

```javascript
fs.readFile("data.txt",(err,data)=>{

console.log(data.toString());

});
```

Output:

```
Hello Node.js
```

---

# Encoding Options

Most common:

```
utf-8
```

means:

> Convert binary data into readable text.

Example:

```javascript
fs.readFile(
"data.txt",
"utf-8",
callback
)
```

Now:

```
data = "Hello Node.js"
```

instead of:

```
data = <Buffer>
```

---

# Error Handling Pattern

Always write:

```javascript
fs.readFile(
"file.txt",
"utf-8",
(err,data)=>{

    if(err){
        console.log("File reading failed");
        return;
    }

    console.log(data);

});
```

Why return?

Without return:

```javascript
if(err){
 console.log(err);
}

console.log(data);
```

Even after error, next code may execute.

---

# PART 2: `fs.readFileSync()`

Synchronous version.

Syntax:

```javascript
fs.readFileSync(path, options)
```

Example:

```javascript
const data = fs.readFileSync(
"data.txt",
"utf-8"
);

console.log(data);
```

Output:

```
Hello Node.js
```

---

Difference:

Async:

```
Start reading
      |
      |
Continue program
      |
      |
File finished
      |
      |
Callback runs
```

Sync:

```
Start reading
      |
      |
Wait here
      |
      |
File finished
      |
      |
Continue
```

---

# PART 3: `fs.promises.readFile()`

Modern approach.

Example:

```javascript
const fs = require("fs/promises");


async function read(){

const data = await fs.readFile(
"data.txt",
"utf-8"
);

console.log(data);

}


read();
```

---

Why preferred?

Because async/await is cleaner:

Old:

```javascript
callback inside callback
```

Modern:

```javascript
await something()
```

---

# Comparison

|Feature|readFile|readFileSync|promises.readFile|
|---|---|---|---|
|Async|Yes|No|Yes|
|Blocks program|No|Yes|No|
|Uses callback|Yes|No|No|
|Uses await|No|No|Yes|
|Modern style|Medium|Low|High|

---

# Real Backend Example

Imagine Express server:

User requests:

```
GET /profile
```

Server needs:

```
user.json
```

Bad:

```javascript
fs.readFileSync("user.json")
```

Because every user waits.

Better:

```javascript
await fs.readFile("user.json");
```

---

# What you should remember today

```
fs.readFile()
        |
        → async
        → callback
        → does not block


fs.readFileSync()
        |
        → sync
        → blocks execution


fs.promises.readFile()
        |
        → async
        → await
        → modern Node.js
```

---

Next in `fs` module after reading files, learn in this order:

1. ✅ Reading files (`readFile`)
    
2. Writing files (`writeFile`)
    
3. Appending files (`appendFile`)
    
4. Deleting files (`unlink`)
    
5. Renaming files (`rename`)
    
6. Creating folders (`mkdir`)
    
7. Reading folders (`readdir`)
    
8. File information (`stat`)
    
9. Watching files (`watch`)
    

This order matches how Node.js developers actually use `fs`.