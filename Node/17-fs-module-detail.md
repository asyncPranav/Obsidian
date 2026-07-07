
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


---
# ✍️ Write - FS Module

Let's learn **Writing Files** in detail.

---

# `fs.writeFile()` - Writing Files

## What is it?

`writeFile()` is used to **create a new file** or **overwrite an existing file**.

> **Important Rule**
> 
> - If the file **doesn't exist**, Node.js creates it.
>     
> - If the file **already exists**, its old content is **completely replaced**.
>     

---

# Three Ways to Write Files

|Method|Type|Modern?|
|---|---|---|
|`fs.writeFile()`|Async (Callback)|Yes|
|`fs.writeFileSync()`|Sync|Sometimes|
|`fs.promises.writeFile()`|Async (Promise)|⭐ Most Recommended|

---

# 1. `fs.writeFile()`

## Syntax

```javascript
fs.writeFile(file, data[, options], callback)
```

---

## Parameters

### 1. `file`

**Type:** `string | Buffer | URL | File Descriptor`

The location where the file should be created or updated.

Example:

```text
"./notes.txt"
"./data/user.json"
```

---

### 2. `data`

**Type:** `string | Buffer | TypedArray | DataView`

The content to write into the file.

Example:

```text
"Hello World"
```

or

```javascript
JSON.stringify(user)
```

---

### 3. `options` (Optional)

Can be either:

```javascript
"utf-8"
```

or

```javascript
{
    encoding: "utf-8",
    mode: 0o666,
    flag: "w"
}
```

---

### 4. `callback`

```javascript
(err) => {}
```

Runs after writing completes.

If successful:

```text
err = null
```

Otherwise:

```text
err = Error object
```

---

# Example 1

```javascript
const fs = require("node:fs");

fs.writeFile(
    "./hello.txt",
    "Hello Node.js",
    "utf-8",
    (err) => {

        if (err) {
            console.error(err);
            return;
        }

        console.log("File written successfully");
    }
);
```

---

# Execution

Before

```
project
│
└── app.js
```

Run program

↓

After

```
project
│
├── app.js
└── hello.txt
```

Content

```
Hello Node.js
```

---

# Example 2

Suppose

```
hello.txt
```

contains

```
ABC
```

Now run

```javascript
fs.writeFile(
"./hello.txt",
"XYZ",
callback
);
```

Result

```
hello.txt

XYZ
```

Old content

```
ABC
```

is gone.

---

# Why?

Because

```text
writeFile()

↓

Overwrite
```

It **does not append**.

Appending is a different method (`appendFile()`).

---

# Options Object

Instead of

```javascript
"utf-8"
```

You can write

```javascript
{
    encoding: "utf-8"
}
```

or

```javascript
{
    encoding: "utf-8",
    flag: "w"
}
```

---

# Important Options

## encoding

Default

```
utf8
```

Example

```javascript
{
    encoding:"utf-8"
}
```

---

## flag

Determines **how** the file is opened.

Default

```
w
```

Meaning

```
Write

If file exists
↓

Overwrite
```

---

Common flags

|Flag|Meaning|
|---|---|
|`w`|Write (overwrite or create)|
|`wx`|Write only if file doesn't exist|
|`a`|Append|
|`ax`|Append only if file doesn't exist|

We'll study all flags later.

---

## mode

Permissions

Default

```
0o666
```

Mostly ignored while learning.

---

# Callback

```javascript
(err)=>{
}
```

Only one parameter.

Unlike `readFile()`

there is **no data** returned.

Because writing a file doesn't produce file content.

---

# `writeFileSync()`

## Syntax

```javascript
fs.writeFileSync(
file,
data,
options
);
```

Returns

```
undefined
```

If successful.

If failure

↓

Throws Error.

---

Example

```javascript
fs.writeFileSync(
"./sync.txt",
"Hello Sync",
"utf-8"
);

console.log("Done");
```

Output

```
Done
```

---

# `fs.promises.writeFile()`

Modern version.

Example

```javascript
const fs = require("node:fs/promises");

async function write(){

    try{

        await fs.writeFile(
            "./promise.txt",
            "Hello Promise",
            "utf-8"
        );

        console.log("Done");

    }catch(err){

        console.error(err);

    }

}

write();
```

---

# Return Values

|Method|Returns|
|---|---|
|`writeFile()`|Nothing (uses callback)|
|`writeFileSync()`|`undefined`|
|`promises.writeFile()`|Promise|

---

# Interview Question

### Does `writeFile()` create a file?

✅ Yes.

If missing.

---

### Does `writeFile()` overwrite?

✅ Yes.

---

### Does `writeFile()` append?

❌ No.

Use

```
appendFile()
```

---

# Real Example

Imagine user registration.

```
User submits form

↓

Node receives data

↓

writeFile()

↓

users.json updated
```

---

# Common Mistakes

## Wrong

```javascript
fs.writeFile(
"./a.txt",
callback
);
```

Missing

```
data
```

---

## Wrong

Expecting

```javascript
callback(err,data)
```

There is

```
NO data
```

Only

```javascript
callback(err)
```

---

# Summary

|Method|Async|Sync|Modern|
|---|---|---|---|
|`writeFile()`|✅|❌|Good|
|`writeFileSync()`|❌|✅|Sometimes|
|`promises.writeFile()`|✅|❌|⭐ Best|

---

# Memory Trick

```
readFile()
        ↓
Returns file content

writeFile()
        ↓
Takes file content

appendFile()
        ↓
Adds new content

unlink()
        ↓
Deletes file
```

## Practice Challenge

Create a small project with these files:

```
project/
├── app.js
└── (no other files initially)
```

In `app.js`, practice:

1. Create a new file called `notes.txt` and write `"Learning Node.js FS Module"` into it using `fs.writeFile()`.
    
2. Run the program again with different content (for example, `"FS Module is awesome"`). Observe that the previous content is overwritten.
    
3. Repeat the same exercise using `fs.writeFileSync()`.
    
4. Repeat it using `fs.promises.writeFile()` with `async/await` and proper `try...catch`.
    

Once you're comfortable with `writeFile()`, the next topic is **`appendFile()`**, which is where you'll learn how to **add content without overwriting the existing file**—a very common requirement in logging, reports, and data storage.

---


# appendFile -
