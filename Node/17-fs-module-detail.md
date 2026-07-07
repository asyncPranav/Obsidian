
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


# appendFile() - FS module

Excellent. `appendFile()` is one of the most useful methods in the `fs` module. You'll use it for **logs, reports, chat history, audit trails, and adding data to existing files**.

---

# `fs.appendFile()` - Append (Add) Data to a File

## What is `appendFile()`?

`appendFile()` is used to **add new data at the end of a file without removing the existing content**.

### Important Rule

- ✅ If the file exists → new data is added to the end.
    
- ✅ If the file doesn't exist → Node.js creates the file automatically.
    

---

# Visual Example

Suppose `notes.txt` contains:

```text
Java
```

Now append:

```text
Node.js
```

Final file:

```text
Java
Node.js
```

Notice:

- Old data remains.
    
- New data is added at the end.
    

---

# Three Ways to Append

|Method|Type|Blocks Program?|Modern?|
|---|---|---|---|
|`fs.appendFile()`|Async (Callback)|❌ No|✅ Yes|
|`fs.appendFileSync()`|Sync|✅ Yes|Sometimes|
|`fs.promises.appendFile()`|Async (Promise)|❌ No|⭐ Recommended|

---

# 1. `fs.appendFile()`

## Syntax

```javascript
fs.appendFile(path, data[, options], callback)
```

---

## Parameters

### 1. `path`

**Type**

```text
string | Buffer | URL | File Descriptor
```

Location of the file.

Example

```text
"./notes.txt"
"./logs/server.log"
```

---

### 2. `data`

**Type**

```text
string | Buffer | TypedArray | DataView
```

Content to append.

Examples

```text
"Hello"
```

```text
"\nNew Line"
```

---

### 3. `options` (Optional)

Can be:

```javascript
"utf-8"
```

or

```javascript
{
    encoding: "utf-8",
    mode: 0o666,
    flag: "a"
}
```

---

### 4. `callback`

```javascript
(err) => {}
```

If successful

```text
err = null
```

If failed

```text
err = Error object
```

---

# Program 1 - Basic Append

Initial file:

`notes.txt`

```text
Java
```

Program:

```javascript
const fs = require("node:fs");

fs.appendFile(
    "./notes.txt",
    "\nNode.js",
    "utf-8",
    (err) => {

        if (err) {
            console.error(err);
            return;
        }

        console.log("Content appended successfully.");
    }
);
```

Output:

```text
Content appended successfully.
```

File becomes:

```text
Java
Node.js
```

---

# Program 2 - File Doesn't Exist

Suppose

```text
students.txt
```

doesn't exist.

Program:

```javascript
const fs = require("node:fs");

fs.appendFile(
    "./students.txt",
    "Pranav",
    "utf-8",
    (err) => {

        if (err) {
            console.error(err);
            return;
        }

        console.log("File created and data appended.");
    }
);
```

After execution:

```text
students.txt

Pranav
```

Node automatically created the file.

---

# Program 3 - Append Multiple Lines

```javascript
const fs = require("node:fs");

fs.appendFile(
    "./log.txt",
    "\nServer Started",
    (err) => {

        if(err){
            console.error(err);
            return;
        }

        console.log("Log Updated");
    }
);
```

Run multiple times:

```
Server Started
Server Started
Server Started
Server Started
```

This is how log files work.

---

# Options Object

Instead of

```javascript
"utf-8"
```

you can write

```javascript
{
    encoding: "utf-8"
}
```

or

```javascript
{
    encoding: "utf-8",
    flag: "a"
}
```

---

# Common Options

## encoding

Default

```text
utf8
```

Example

```javascript
{
    encoding: "utf-8"
}
```

---

## flag

Default

```text
a
```

Meaning

```text
Append Mode
```

Other useful flags:

|Flag|Meaning|
|---|---|
|`a`|Append. Create file if it doesn't exist.|
|`ax`|Append only if file doesn't exist (fails if file exists).|

> **Note:** `appendFile()` already uses `flag: "a"` by default, so you rarely need to specify it yourself.

---

## mode

Default

```text
0o666
```

File permissions.

Usually left unchanged.

---

# Return Value

```javascript
fs.appendFile(...)
```

Returns

```text
undefined
```

Result comes through the callback.

---

# `appendFileSync()`

Synchronous version.

---

## Syntax

```javascript
fs.appendFileSync(path, data[, options])
```

---

Program

```javascript
const fs = require("node:fs");

fs.appendFileSync(
    "./sync.txt",
    "\nHello Sync",
    "utf-8"
);

console.log("Done");
```

File

```
Hello Sync
```

Run again

```
Hello Sync
Hello Sync
```

---

## Return Value

```text
undefined
```

If something goes wrong

↓

Throws Error.

---

# `fs.promises.appendFile()`

Modern Promise version.

---

## Syntax

```javascript
await fs.promises.appendFile(path, data[, options])
```

---

Program

```javascript
const fs = require("node:fs/promises");

async function appendData() {

    try {

        await fs.appendFile(
            "./promise.txt",
            "\nPromise Data",
            "utf-8"
        );

        console.log("Append Successful");

    } catch(err) {

        console.error(err);

    }

}

appendData();
```

---

# Return Value

Returns

```text
Promise<void>
```

---

# Difference Between `writeFile()` and `appendFile()`

Suppose file contains:

```
Java
```

---

Using

```javascript
fs.writeFile()
```

Write

```
Node.js
```

Final file

```
Node.js
```

Old content is deleted.

---

Using

```javascript
fs.appendFile()
```

Append

```
Node.js
```

Final file

```
Java
Node.js
```

Old content remains.

---

# Real-World Uses

### 1. Server Logs

```
10:00 Server Started
10:05 User Logged In
10:06 Product Added
10:10 Payment Success
```

Each event is appended.

---

### 2. Chat History

```
Alice : Hello
Bob : Hi
Alice : How are you?
```

Messages are appended.

---

### 3. Attendance

```
Rahul
Aman
Pranav
```

Every new student is appended.

---

### 4. Error Logs

```
404 Error
500 Error
Database Failed
```

Errors are appended.

---

# Common Mistakes

### ❌ Forgetting `\n`

```javascript
fs.appendFile("a.txt","Hello");
fs.appendFile("a.txt","World");
```

Output

```
HelloWorld
```

---

Correct

```javascript
fs.appendFile("a.txt","\nWorld");
```

Output

```
Hello
World
```

---

### ❌ Expecting Callback Data

Wrong

```javascript
(err, data) => {}
```

Correct

```javascript
(err) => {}
```

There is **no `data` parameter** because `appendFile()` only writes data; it doesn't return file content.

---

# Comparison

|Method|Async|Sync|Promise|Creates File?|Overwrites?|Appends?|
|---|---|---|---|---|---|---|
|`writeFile()`|✅|❌|❌|✅|✅|❌|
|`writeFileSync()`|❌|✅|❌|✅|✅|❌|
|`appendFile()`|✅|❌|❌|✅|❌|✅|
|`appendFileSync()`|❌|✅|❌|✅|❌|✅|
|`promises.appendFile()`|✅|❌|✅|✅|❌|✅|

---

# Quick Revision

```text
appendFile()

Purpose:
→ Add data at the end of a file.

If file exists:
→ Append content.

If file doesn't exist:
→ Create file and write content.

Methods:
→ appendFile()
→ appendFileSync()
→ fs.promises.appendFile()

Returns:
→ Callback version: undefined
→ Sync version: undefined
→ Promise version: Promise<void>

Default Flag:
→ "a" (append)

Default Encoding:
→ "utf8"
```

## Practice Exercises

1. Create a file `diary.txt` and append three diary entries, each on a new line.
    
2. Create a `server.log` file and append the current date/time each time you run the program.
    
3. Build a simple "to-do list" program where each new task is appended to `tasks.txt`.
    
4. Read the contents of `tasks.txt` after appending to verify that all tasks are preserved.
    

If you can complete these exercises, you'll have mastered one of the most commonly used file operations in Node.js.

---


# Node.js `fs` Module — Delete File and Rename File (Detailed Notes)

After learning:

- `readFile()` → Read data
    
- `writeFile()` → Create/overwrite data
    
- `appendFile()` → Add data
    

The next common file operations are:

1. **Delete a file**
    
2. **Rename a file**
    

Node.js provides:

|Operation|Async|Sync|Promise|
|---|---|---|---|
|Delete file|`fs.unlink()`|`fs.unlinkSync()`|`fs.promises.unlink()`|
|Rename file|`fs.rename()`|`fs.renameSync()`|`fs.promises.rename()`|

---

# PART 1: Delete File (`unlink`)

## What is `unlink()`?

`unlink()` is used to **remove/delete a file from the file system**.

Example:

Before:

```
project
│
├── app.js
└── notes.txt
```

Run:

```javascript
fs.unlink("notes.txt")
```

After:

```
project
│
└── app.js
```

`notes.txt` is deleted permanently.

---

# 1. `fs.unlink()` (Asynchronous)

## Syntax

```javascript
fs.unlink(path, callback)
```

---

## Parameters

### 1. path

The file path to delete.

Type:

```
string | Buffer | URL
```

Example:

```javascript
"./notes.txt"
```

---

### 2. callback

Runs after deletion.

Syntax:

```javascript
(err) => {}
```

Important:

`unlink()` does not return deleted data.

It only gives error information.

---

# Basic Example

Folder:

```
project
│
├── app.js
└── test.txt
```

`app.js`

```javascript
const fs = require("node:fs");


fs.unlink("./test.txt", (err)=>{

    if(err){
        console.log(err);
        return;
    }

    console.log("File deleted successfully");

});
```

Output:

```
File deleted successfully
```

Now:

```
project
│
└── app.js
```

---

# If File Does Not Exist

Example:

```javascript
fs.unlink("./abc.txt",(err)=>{

    if(err){
        console.log(err);
        return;
    }

});
```

Output:

```
Error: ENOENT: no such file or directory
```

---

## Checking Error Code

```javascript
fs.unlink("./abc.txt",(err)=>{

    if(err){

        console.log(err.code);

    }

});
```

Output:

```
ENOENT
```

Meaning:

```
File does not exist
```

---

# 2. `fs.unlinkSync()`

Synchronous delete.

## Syntax

```javascript
fs.unlinkSync(path)
```

---

Example:

```javascript
const fs = require("node:fs");


fs.unlinkSync("./test.txt");


console.log("Deleted");
```

Output:

```
Deleted
```

---

## Flow

Async:

```
Start delete
      |
      |
Continue program
      |
      |
Delete complete
      |
      |
Callback
```

Sync:

```
Start delete
      |
      |
Wait
      |
      |
Delete complete
      |
      |
Continue
```

---

## Error Handling

Sync methods use:

```javascript
try...catch
```

Example:

```javascript
try{

    fs.unlinkSync("./test.txt");

    console.log("Deleted");

}
catch(err){

    console.log(err);

}
```

---

# 3. `fs.promises.unlink()`

Modern Promise approach.

## Syntax

```javascript
await fs.promises.unlink(path)
```

---

Example:

```javascript
const fs = require("node:fs/promises");


async function deleteFile(){

    try{

        await fs.unlink("./test.txt");

        console.log("Deleted");

    }
    catch(err){

        console.log(err);

    }

}


deleteFile();
```

---

## Return Value

|Method|Return|
|---|---|
|`unlink()`|undefined (callback)|
|`unlinkSync()`|undefined|
|`promises.unlink()`|Promise|

---

# Real World Uses of unlink()

## 1. Delete Uploaded Files

Example:

```
User uploads profile image

↓

Image saved

↓

User deletes profile

↓

unlink(image.jpg)
```

---

## 2. Remove Temporary Files

Example:

```
temp.pdf
temp-image.png
cache.txt
```

Delete after processing.

---

# PART 2: Rename File (`rename`)

## What is `rename()`?

`rename()` is used to:

1. Change file name
    
2. Move file to another location
    

---

Example:

Before:

```
project

notes.txt
```

Code:

```javascript
fs.rename(
"notes.txt",
"diary.txt"
)
```

After:

```
project

diary.txt
```

---

# 1. `fs.rename()` (Async)

## Syntax

```javascript
fs.rename(oldPath, newPath, callback)
```

---

## Parameters

### oldPath

Current file location.

Example:

```javascript
"./old.txt"
```

---

### newPath

New name/location.

Example:

```javascript
"./new.txt"
```

---

### callback

```javascript
(err)=>{}
```

---

# Example: Rename File

Before:

```
data.txt
```

Code:

```javascript
const fs = require("node:fs");


fs.rename(
    "./data.txt",
    "./newData.txt",
    (err)=>{

        if(err){
            console.log(err);
            return;
        }


        console.log("File renamed");

    }
);
```

After:

```
newData.txt
```

---

# Rename + Move File

Suppose:

Before:

```
project

file.txt

backup/
```

Code:

```javascript
fs.rename(
"./file.txt",
"./backup/file.txt",
(err)=>{

    if(err)
        console.log(err);

});
```

After:

```
project

backup
   |
   file.txt
```

So `rename()` can move files too.

---

# 2. `fs.renameSync()`

## Syntax

```javascript
fs.renameSync(oldPath,newPath)
```

---

Example:

```javascript
const fs = require("node:fs");


fs.renameSync(
    "./old.txt",
    "./new.txt"
);


console.log("Done");
```

---

# Error Handling

```javascript
try{

    fs.renameSync(
        "./old.txt",
        "./new.txt"
    );

}
catch(err){

    console.log(err);

}
```

---

# 3. `fs.promises.rename()`

Modern approach.

## Syntax

```javascript
await fs.promises.rename(oldPath,newPath)
```

---

Example:

```javascript
const fs = require("node:fs/promises");


async function renameFile(){

    try{

        await fs.rename(
            "./old.txt",
            "./new.txt"
        );

        console.log("Renamed");

    }
    catch(err){

        console.log(err);

    }

}


renameFile();
```

---

# Difference Between Delete and Rename

|Operation|Method|
|---|---|
|Delete file|`unlink()`|
|Rename file|`rename()`|
|Move file|`rename()`|

---

# Important Interview Questions

## Q1. Does `unlink()` delete folders?

❌ No.

Only files.

For folders use:

```javascript
fs.rmdir()
```

or

```javascript
fs.rm()
```

---

## Q2. Can `rename()` move a file?

✅ Yes.

Example:

```javascript
fs.rename(
"a.txt",
"folder/a.txt"
)
```

---

## Q3. What happens if rename destination exists?

Depends on OS.

Usually it replaces the destination file.

---

## Q4. Does unlink return deleted data?

❌ No.

It only confirms success or error.

---

# Complete File Operation Summary

```
READ
 |
 ├── readFile()
 ├── readFileSync()
 └── promises.readFile()


CREATE / WRITE
 |
 ├── writeFile()
 ├── writeFileSync()
 └── promises.writeFile()


APPEND
 |
 ├── appendFile()
 ├── appendFileSync()
 └── promises.appendFile()


DELETE
 |
 ├── unlink()
 ├── unlinkSync()
 └── promises.unlink()


RENAME / MOVE
 |
 ├── rename()
 ├── renameSync()
 └── promises.rename()
```

---

# Real Project Example

A simple file manager:

```
Upload File
      |
      ↓
writeFile()

Update File
      |
      ↓
appendFile()

Rename File
      |
      ↓
rename()

Delete File
      |
      ↓
unlink()
```

These four operations (`writeFile`, `appendFile`, `rename`, `unlink`) form the foundation of file handling in Node.js.


---


# Node.js `fs` Module — Creating Folders (`mkdir`) and Reading Folders (`readdir`) Detailed Notes

After learning file operations:

```
readFile()       → Read files
writeFile()      → Create/overwrite files
appendFile()     → Add data
unlink()         → Delete files
rename()         → Rename/move files
```

Now we learn **directory (folder) operations**:

```
mkdir()          → Create folders
readdir()        → Read folder contents
```

---

# PART 1: Creating Folders (`fs.mkdir()`)

## What is `mkdir()`?

`mkdir` stands for:

```
make directory
```

It is used to **create a new folder/directory**.

Example:

Before:

```
project
│
└── app.js
```

Code:

```javascript
fs.mkdir("./images")
```

After:

```
project
│
├── app.js
│
└── images
```

---

# Three Ways to Create Folder

|Method|Type|Usage|
|---|---|---|
|`fs.mkdir()`|Async callback|Common|
|`fs.mkdirSync()`|Sync|Small scripts|
|`fs.promises.mkdir()`|Promise|Modern ⭐|

---

# 1. `fs.mkdir()` (Asynchronous)

## Syntax

```javascript
fs.mkdir(path[, options], callback)
```

---

## Parameters

## 1. path

Folder location.

Type:

```
string | Buffer | URL
```

Example:

```javascript
"./uploads"
```

---

## 2. options (Optional)

Object containing:

- recursive
    
- mode
    

Syntax:

```javascript
{
    recursive: true,
    mode: 0o777
}
```

---

## 3. callback

Runs after folder creation.

Syntax:

```javascript
(err)=>{
}
```

---

# Basic Example

```javascript
const fs = require("node:fs");


fs.mkdir("./images",(err)=>{

    if(err){
        console.log(err);
        return;
    }

    console.log("Folder created");

});
```

Output:

```
Folder created
```

Folder:

```
project

images/
```

---

# Important: Creating Nested Folders

Suppose you want:

```
project
 |
 └── uploads
        |
        └── images
              |
              └── profile
```

Without recursive:

```javascript
fs.mkdir("./uploads/images/profile")
```

Error:

```
ENOENT
```

because parent folders don't exist.

---

## Using `recursive:true`

```javascript
const fs = require("node:fs");


fs.mkdir(
    "./uploads/images/profile",
    {
        recursive:true
    },
    (err)=>{

        if(err){
            console.log(err);
            return;
        }

        console.log("Folders created");

    }
);
```

Now Node creates:

```
uploads
 |
 images
 |
 profile
```

---

# What does recursive do?

Without:

```
Create profile

❌ uploads does not exist
```

With:

```
Create uploads
        |
        ↓
Create images
        |
        ↓
Create profile
```

---

# `mkdir()` with mode

Mode controls permissions.

Example:

```javascript
fs.mkdir(
"./private",
{
    mode:0o700
},
callback
);
```

Usually you don't modify this in normal applications.

---

# 2. `fs.mkdirSync()`

Synchronous folder creation.

## Syntax

```javascript
fs.mkdirSync(path[, options])
```

---

Example:

```javascript
const fs = require("node:fs");


fs.mkdirSync("./logs");


console.log("Folder created");
```

Output:

```
Folder created
```

---

## With recursive

```javascript
fs.mkdirSync(
"./data/users/profile",
{
    recursive:true
}
);
```

Creates:

```
data
 |
 users
    |
    profile
```

---

# Error Handling

Sync methods use:

```javascript
try...catch
```

Example:

```javascript
try{

    fs.mkdirSync("./images");

}
catch(err){

    console.log(err);

}
```

---

# 3. `fs.promises.mkdir()`

Modern async approach.

## Syntax

```javascript
await fs.promises.mkdir(path[, options])
```

---

Example:

```javascript
const fs = require("node:fs/promises");


async function createFolder(){

    try{

        await fs.mkdir(
            "./uploads",
            {
                recursive:true
            }
        );

        console.log("Folder created");

    }
    catch(err){

        console.log(err);

    }

}


createFolder();
```

---

# Return Value of mkdir

|Method|Returns|
|---|---|
|`mkdir()`|callback result|
|`mkdirSync()`|undefined|
|`promises.mkdir()`|Promise|

---

# PART 2: Reading Folders (`fs.readdir()`)

## What is `readdir()`?

`readdir()` is used to **read the contents of a directory**.

It returns:

- files
    
- subfolders
    

inside a folder.

---

Example:

Folder:

```
project

├── app.js
├── package.json
└── images
```

Code:

```javascript
fs.readdir("./project")
```

Returns:

```
[
 "app.js",
 "package.json",
 "images"
]
```

---

# Three Ways to Read Folder

|Method|Type|
|---|---|
|`fs.readdir()`|Async callback|
|`fs.readdirSync()`|Sync|
|`fs.promises.readdir()`|Promise|

---

# 1. `fs.readdir()` (Async)

## Syntax

```javascript
fs.readdir(path[, options], callback)
```

---

## Parameters

## path

Folder path.

Example:

```javascript
"./"
```

---

## options

Optional.

Can specify:

- encoding
    
- withFileTypes
    

Example:

```javascript
{
 encoding:"utf-8",
 withFileTypes:true
}
```

---

## callback

Syntax:

```javascript
(err, files)=>{
}
```

---

# Basic Example

Folder:

```
project

├── app.js
├── data.txt
└── images
```

Code:

```javascript
const fs = require("node:fs");


fs.readdir("./",(err,files)=>{

    if(err){
        console.log(err);
        return;
    }


    console.log(files);

});
```

Output:

```
[
'app.js',
'data.txt',
'images'
]
```

---

# Reading with File Types

Normally:

```javascript
fs.readdir("./",(err,files)=>{
});
```

returns only names:

```
[
'app.js',
'images'
]
```

You don't know:

```
app.js → file
images → folder
```

---

Use:

```javascript
withFileTypes:true
```

Example:

```javascript
fs.readdir(
"./",
{
    withFileTypes:true
},
(err,files)=>{

    files.forEach(file=>{

        console.log(
            file.name
        );

    });

});
```

---

Output:

```
app.js
images
```

But now each item is a `Dirent` object.

---

# Dirent Object

Example:

```javascript
{
 name:"app.js"
}
```

It provides methods:

```
isFile()
isDirectory()
isSymbolicLink()
```

---

Example:

```javascript
fs.readdir(
"./",
{
withFileTypes:true
},
(err,files)=>{


files.forEach(file=>{


if(file.isFile()){

console.log(
"File:",
file.name
);

}


if(file.isDirectory()){

console.log(
"Folder:",
file.name
);

}


});


});
```

Output:

```
File: app.js
Folder: images
```

---

# 2. `fs.readdirSync()`

Synchronous version.

## Syntax

```javascript
fs.readdirSync(path[, options])
```

---

Example:

```javascript
const fs = require("node:fs");


const files = fs.readdirSync("./");


console.log(files);
```

Output:

```
[
'app.js',
'package.json'
]
```

---

With types:

```javascript
const files =
fs.readdirSync(
"./",
{
withFileTypes:true
}
);
```

---

# 3. `fs.promises.readdir()`

Modern Promise version.

## Syntax

```javascript
await fs.promises.readdir(path[, options])
```

---

Example:

```javascript
const fs = require("node:fs/promises");


async function readFolder(){

try{

const files =
await fs.readdir("./");


console.log(files);


}
catch(err){

console.log(err);

}

}


readFolder();
```

---

# Real Example: List Only Files

```javascript
const fs = require("node:fs");


fs.readdir(
"./",
{
withFileTypes:true
},
(err,items)=>{


items.forEach(item=>{


if(item.isFile()){

console.log(item.name);

}


});


});
```

Output:

```
app.js
package.json
```

---

# Difference Between `readdir()` and `readFile()`

|Method|Works On|Returns|
|---|---|---|
|`readFile()`|File|File content|
|`readdir()`|Folder|Files/folders list|

Example:

```
readFile()

notes.txt
    |
    ↓
"Hello Node"


readdir()

project/
    |
    ↓
[
app.js,
notes.txt
]
```

---

# Complete Directory Operations Summary

```
CREATE FOLDER
|
├── mkdir()
├── mkdirSync()
└── promises.mkdir()


READ FOLDER
|
├── readdir()
├── readdirSync()
└── promises.readdir()
```

---

# Real Backend Examples

## Upload System

Create folder:

```javascript
fs.mkdir("./uploads")
```

Store:

```
uploads
 |
 images
 |
 videos
```

---

## File Manager

Read folder:

```javascript
fs.readdir("./uploads")
```

Show:

```
photo.jpg
video.mp4
document.pdf
```

---

## Logging System

Create:

```
logs/
```

Store:

```
error.log
server.log
```

---

# Quick Revision

### mkdir()

```
Create folder

Syntax:
mkdir(path, options, callback)

Important option:
recursive:true

Example:

fs.mkdir(
"./a/b/c",
{recursive:true},
callback
)
```

---

### readdir()

```
Read folder contents

Syntax:
readdir(path, options, callback)

Important option:
withFileTypes:true

Example:

fs.readdir(
"./",
{withFileTypes:true},
callback
)
```

---

Next important `fs` topics after this:

1. `fs.stat()` → Get file/folder information
    
2. `fs.access()` → Check permission/existence
    
3. `fs.copyFile()` → Copy files
    
4. `fs.watch()` → Monitor file changes
    
5. Streams (`createReadStream`, `createWriteStream`) → Handle large files efficiently


---

