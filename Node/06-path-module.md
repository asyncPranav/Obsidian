
---
# 📁 Node.js `path` Module – **Complete Detailed Notes**

---

## 1️⃣ What is the `path` module? (Deep Understanding)

The **`path` module** is a **built-in Node.js core module** that helps us **work with file and directory paths in a safe, OS-independent way**.

### ❓ Why do we need it?

Different operating systems handle paths differently:

|OS|Path Example|
|---|---|
|Windows|`C:\users\admin\file.txt`|
|Linux / Mac|`/home/admin/file.txt`|

If you hardcode paths:

```js
"user/docs/file.txt"
```

❌ It may break on Windows.

✔ `path` module **automatically uses the correct separator**.

---

## 2️⃣ Importing the path module

```js
const path = require("path");
```

No installation needed – it comes with Node.js.

---

## 3️⃣ Very Important Concepts

### 📌 Absolute Path

A full path starting from the root.

```js
C:\project\src\index.js
```

### 📌 Relative Path

Path relative to current working directory.

```js
src/index.js
```

---

# 🔥 PATH MODULE METHODS (DETAILED)

---

## 🔹 1. `path.basename()`

### 🧠 What it does

Returns the **last part of the path**, usually the **file name**.

---

### Syntax

```js
path.basename(path, [extension])
```

---

### Example 1: Basic usage

```js
const path = require("path");

const filePath = "/user/docs/file.txt";
console.log(path.basename(filePath));
```

### Output

```js
file.txt
```

---

### Example 2: Remove extension

```js
console.log(path.basename(filePath, ".txt"));
```

### Output

```js
file
```

---

### ✅ Use case

- Extract file name from upload path
    
- Logging file names
    
- Validation of file type
    

---

## 🔹 2. `path.dirname()`

### 🧠 What it does

Returns the **directory (folder) portion** of a path.


### Syntax

```js
path.extname(path)
```


---

### Example

```js
const dir = path.dirname("/user/docs/file.txt");
console.log(dir);
```
``

### Output

```sh
/user/docs
```

---

### ✅ Use case

- Get folder location
    
- Create related files in same directory
    

---

## 🔹 3. `path.extname()`

### 🧠 What it does

Returns the **extension of a file** (including `.`).

### Syntax

```js
path.extname(path)
```


---

### Example 1

```js
console.log(path.extname("image.png"));
```

### Output

```sh
.png
```

---

### Example 2: No extension

```js
console.log(path.extname("README"));
```

### Output

```sh
(empty string)
```

---

### ✅ Use case

- File type checking
    
- Allow/deny uploads
    
- MIME validation
    

---

## 🔹 4. `path.join()` ⭐ MOST USED

### 🧠 What it does

Safely **joins multiple path segments** into one clean path.

✔ Adds correct separator  
✔ Removes extra slashes  
✔ OS independent

---

### Syntax

```js
path.join([...paths])
```

**Example**

```js
path.join("folder", "file.txt");
```

**Output**

```sh
folder/file.txt
```

---

### Example 1: Simple join

```js
console.log(path.join("user", "docs", "file.txt"));
```

### Output

```sh
user\docs\file.txt   (Windows)
user/docs/file.txt  (Linux/Mac)
```

---

### Example 2: Messy input

```js
console.log(path.join("user/", "/docs", "//file.txt"));
```

### Output

```sh
user/docs/file.txt
```

---

### ✅ Use case

- Creating file paths
    
- Reading / writing files
    
- Best practice with `__dirname`
    

---

## 🔹 5. `path.resolve()`

### 🧠 What it does

Creates an **absolute path**.

---

### Syntax:

```js
path.resolve([...paths])
```

Example:

```js
path.resolve("folder", "file.txt");
```

Output:

```sh
/home/user/project/folder/file.txt
```


---

### Example 1: Single argument

```js
console.log(path.resolve("file.txt"));
```

### Output

```sh
C:\project\file.txt
```

---

### Example 2: Multiple arguments

```js
console.log(path.resolve("src", "utils", "app.js"));
```

### Output

```sh
C:\project\src\utils\app.js
```

---

### 🧠 Important rule

- If a segment starts with `/`, everything before it is ignored.
    

```js
path.resolve("a", "/b", "c");
```

Output:

```sh
\b\c
```

---

### ✅ Use case

- Getting absolute paths
    
- Config files
    
- Production-safe paths
    

---

## 🔹 6. `path.isAbsolute()`

### 🧠 What it does

Checks whether a path is **absolute**.

---

### Example

```js
console.log(path.isAbsolute("/user/docs"));
console.log(path.isAbsolute("docs/file.txt"));
```

### Output

```sh
true
false
```

---

### ✅ Use case

- Validate user input paths
    
- Security checks
    

---

## 🔹 7. `path.parse()`

### 🧠 What it does

Breaks a path into an **object with useful properties**.

---

### Syntax:

```js
path.parse(path)
```

Example:

```js
path.parse("/home/user/app.js");
```

Output:

```sh
{
  root: "/",
  dir: "/home/user",
  base: "app.js",
  ext: ".js",
  name: "app"
}
```

---

# 7. `path.format()`

### Purpose:

Creates a path string from an object.

### Syntax:

```js
path.format(pathObject)
```

Example:

```js
path.format({
  dir: "/home/user",
  name: "app",
  ext: ".js"
});
```

Output:

```sh
/home/user/app.js
```

---

# 8. `path.isAbsolute()`

### Purpose:

Checks whether a path is absolute.

### Syntax:

```js
path.isAbsolute(path)
```

Example:

```js
path.isAbsolute("/home/user/file.txt");
```

Output:

```sh
true
```

Example:

```js
path.isAbsolute("file.txt");
```

Output:

```sh
false
```

---

# 9. `path.normalize()`

### Purpose:

Normalizes a path by removing unnecessary parts.

### Syntax:

```js
path.normalize(path)
```

Example:

```js
path.normalize("/home/user/../app");
```

Output:

```sh
/home/app
```

---

# 10. `path.relative()`

### Purpose:

Finds the relative path from one location to another.

### Syntax:

```js
path.relative(from, to)
```

Example:

```js
path.relative(
  "/home/user/project",
  "/home/user/project/src/app.js"
);
```

Output:

```
src/app.js
```

---

# 11. `path.sep`

### Purpose:

Returns the operating system path separator.

### Syntax:

```js
path.sep
```

Windows:

```
\
```

Linux/Mac:

```
/
```

---

# 12. `path.delimiter`

### Purpose:

Returns the delimiter used in environment paths.

### Syntax:

```js
path.delimiter
```

Windows:

```
;
```

Linux/Mac:

```
:
```

---

# 13. `path.win32`

### Purpose:

Provides Windows path operations even on other systems.

### Syntax:

```js
path.win32.method()
```

Example:

```js
path.win32.basename("C:\\Users\\app.js");
```

Output:

```sh
app.js
```

---

# 14. `path.posix`

### Purpose:

Provides POSIX/Linux path operations.

### Syntax:

```js
path.posix.method()
```

Example:

```js
path.posix.basename("/home/app.js");
```

Output:

```
app.js
```

---

# Quick Revision Table

|Method|Use|
|---|---|
|`basename()`|Get filename|
|`dirname()`|Get folder name|
|`extname()`|Get extension|
|`join()`|Combine paths|
|`resolve()`|Create absolute path|
|`parse()`|Path → Object|
|`format()`|Object → Path|
|`isAbsolute()`|Check absolute path|
|`normalize()`|Clean path|
|`relative()`|Find relative path|
|`sep`|OS separator|
|`delimiter`|Environment separator|
|`win32`|Windows path handling|
|`posix`|Linux/Mac path handling|

Most used in real Node.js projects:

```
join()
resolve()
basename()
dirname()
extname()
parse()
format()
```

give more detailed syntax if there are options available mention that too

# Node.js `path` Module — Complete Syntax + Options

The `path` module provides utilities for working with **file and directory paths**.

Import:

### CommonJS

```
const path = require("path");
```

### ES Module

```
import path from "path";
```

---

# 1. `path.basename()`

### Purpose

Returns the **last portion of a path** (usually filename).

### Syntax

```
path.basename(path[, suffix])
```

### Parameters

|Parameter|Type|Required|Description|
|---|---|---|---|
|`path`|string|✅ Yes|File/directory path|
|`suffix`|string|❌ Optional|Removes this ending from result|

---

### Examples

```
path.basename("/home/user/app.js");
```

Output:

```
app.js
```

---

With suffix:

```
path.basename("/home/user/app.js", ".js");
```

Output:

```
app
```

---

# 2. `path.dirname()`

### Purpose

Returns the directory part of a path.

### Syntax

```
path.dirname(path)
```

### Parameters

|Parameter|Type|Required|
|---|---|---|
|path|string|Yes|

---

Example:

```
path.dirname("/home/user/app.js");
```

Output:

```
/home/user
```

---

# 3. `path.extname()`

### Purpose

Returns the extension of a file.

### Syntax

```
path.extname(path)
```

### Parameters

|Parameter|Type|Required|
|---|---|---|
|path|string|Yes|

---

Example:

```
path.extname("index.html");
```

Output:

```
.html
```

---

Cases:

```
path.extname("app");
```

Output:

```
""
```

No extension.

---

```
path.extname(".env");
```

Output:

```
""
```

Because `.env` is treated as a filename, not an extension.

---

# 4. `path.join()`

### Purpose

Joins multiple path segments into one normalized path.

### Syntax

```
path.join([...paths])
```

### Parameters

|Parameter|Type|Required|
|---|---|---|
|paths|strings|Yes|

---

Example:

```
path.join("users", "pranav", "app.js");
```

Output:

```
users/pranav/app.js
```

---

Multiple segments:

```
path.join("folder", "subfolder", "../file.txt");
```

Output:

```
folder/file.txt
```

---

### Important

If no arguments:

```
path.join();
```

Output:

```
.
```

---

# 5. `path.resolve()`

### Purpose

Creates an **absolute path**.

### Syntax

```
path.resolve([...paths])
```

### Parameters

|Parameter|Type|Required|
|---|---|---|
|paths|strings|Optional|

---

Example:

```
path.resolve("app.js");
```

Output:

```
/current/location/app.js
```

---

Multiple paths:

```
path.resolve("folder", "app.js");
```

Output:

```
/current/location/folder/app.js
```

---

### Difference from join()

`join()`:

```
folder/app.js
```

(relative)

`resolve()`:

```
/home/user/project/folder/app.js
```

(absolute)

---

# 6. `path.parse()`

### Purpose

Converts a path string into an object.

### Syntax

```
path.parse(path)
```

### Returns:

Object:

```
{ root, dir, base, ext, name}
```

---

Example:

```
path.parse("/home/user/app.js");
```

Output:

```
{ root: "/", dir: "/home/user", base: "app.js", ext: ".js", name: "app"}
```

---

# 7. `path.format()`

### Purpose

Creates a path string from an object.

### Syntax

```
path.format(pathObject)
```

---

### Object properties:

|Property|Description|
|---|---|
|root|Root directory|
|dir|Directory|
|base|Full filename|
|ext|Extension|
|name|Filename without extension|

---

Example:

```
path.format({  dir: "/home/user",  name: "app",  ext: ".js"});
```

Output:

```
/home/user/app.js
```

---

# 8. `path.isAbsolute()`

### Purpose

Checks whether a path is absolute.

### Syntax

```
path.isAbsolute(path)
```

Returns:

```
true / false
```

---

Example:

```
path.isAbsolute("/home/app.js");
```

Output:

```
true
```

---

```
path.isAbsolute("app.js");
```

Output:

```
false
```

---

# 9. `path.normalize()`

### Purpose

Cleans and normalizes a path.

### Syntax

```
path.normalize(path)
```

---

Example:

```
path.normalize("/home/user//app/../file.js");
```

Output:

```
/home/user/file.js
```

It:

- removes duplicate separators
- resolves `..`
- resolves `.`

---

# 10. `path.relative()`

### Purpose

Finds the relative path between two locations.

### Syntax

```
path.relative(from, to)
```

### Parameters

|Parameter|Description|
|---|---|
|from|Starting path|
|to|Destination path|

---

Example:

```
path.relative( "/home/user", "/home/user/project/app.js");
```

Output:

```
project/app.js
```

---

# 11. `path.sep`

### Purpose

Returns OS-specific separator.

### Syntax

```
path.sep
```

Linux/Mac:

```
/
```

Windows:

```
\
```

---

# 12. `path.delimiter`

### Purpose

Returns environment variable path separator.

### Syntax

```
path.delimiter
```

Linux:

```
:
```

Windows:

```
;
```

---

# 13. `path.win32`

### Purpose

Windows path operations.

### Syntax

```
path.win32.method()
```

Example:

```
path.win32.basename("C:\\Users\\app.js");
```

Output:

```
app.js
```

---

# 14. `path.posix`

### Purpose

POSIX/Linux style path operations.

### Syntax

```
path.posix.method()
```

Example:

```
path.posix.join("home","user","app.js");
```

Output:

```
home/user/app.js
```

---

# Most Important Methods in Real Projects

## File handling

```
path.basename()path.dirname()path.extname()
```

Used with:

- `fs.readFile()`
- `fs.writeFile()`

---

## Creating paths safely

```
path.join()path.resolve()
```

Used for:

- uploads folder
- static files
- database paths

Example:

```
const filePath = path.join(__dirname,"uploads","image.png");
```

---

## Converting paths

```
path.parse()path.format()
```

Used when manipulating filenames.

---

# Quick Cheat Sheet

```
basename()  → filenamedirname()   → folderextname()   → extensionjoin()      → combine pathsresolve()   → absolute pathparse()     → path to objectformat()    → object to pathnormalize() → clean pathrelative()  → path differenceisAbsolute() → check absolutesep         → / or \delimiter   → : or ;
```

These are the complete practical APIs of Node.js `path` module.

---

### Example

```js
const result = path.parse("/user/docs/file.txt");
console.log(result);
```

### Output

```sh
{
  root: '/',
  dir: '/user/docs',
  base: 'file.txt',
  ext: '.txt',
  name: 'file'
}
```

---

### ✅ Use case

- Extract everything in one step
    
- File renaming
    
- File organization logic
    

---

## 🔹 8. `path.format()`

### 🧠 What it does

Builds a path **from an object**.

---

### Example

```js
const obj = {
  dir: "/user/docs",
  name: "file",
  ext: ".txt"
};

console.log(path.format(obj));
```

### Output

```sh
/user/docs/file.txt
```

---

### ✅ Use case

- Rebuild paths after modification
    
- Renaming extensions
    

---

## 🔹 9. `path.normalize()`

### 🧠 What it does

Cleans up a path by:

- Removing extra slashes
    
- Resolving `..` and `.`
    

---

### Example

```js
console.log(path.normalize("/user//docs/../file.txt"));
```

### Output

```sh
/user/file.txt
```

---

### ✅ Use case

- Cleaning user input paths
    
- Avoid path traversal bugs
    

---

## 🔹 10. `path.relative()`

### 🧠 What it does

Returns a **relative path** from one location to another.

---

### Example

```js
console.log(path.relative("/user/docs", "/user/docs/file.txt"));
```

### Output

```sh
file.txt
```

---

### Example 2

```js
console.log(path.relative("/user/docs", "/user/images/photo.png"));
```

### Output

```sh
../images/photo.png
```

---

### ✅ Use case

- Linking files
    
- Creating relative imports
    

---

## 🔹 11. `path.sep`

### 🧠 What it does

Returns OS-specific path separator.

---

```js
console.log(path.sep);
```

### Output

```sh
\   (Windows)
/   (Linux/Mac)
```

---

### ✅ Use case

- Rare, but useful for low-level tooling
    

---

## 🔹 12. `path.delimiter`

### 🧠 What it does

Separator used in environment variables.

---

```js
console.log(path.delimiter);
```

### Output

```sh
;  (Windows)
:  (Linux/Mac)
```

---

### ✅ Use case

- PATH environment variable parsing
    

---

# ⭐ BEST PRACTICE EXAMPLE

```js
const fs = require("fs");
const path = require("path");

const filePath = path.join(__dirname, "data", "info.txt");

fs.readFile(filePath, "utf8", (err, data) => {
  console.log(data);
});
```

✔ Works on all OS  
✔ Production safe

---

# 🧠 INTERVIEW GOLD LINES

- `path.join()` → combine paths safely
    
- `path.resolve()` → get absolute path
    
- `path.parse()` → path → object
    
- `path.format()` → object → path
    
- Never hardcode `/` or `\`
    

---

# 🔁 ONE-PAGE REVISION TABLE

|Method|Purpose|
|---|---|
|basename|Get file name|
|dirname|Get folder|
|extname|Get extension|
|join|Join paths|
|resolve|Absolute path|
|isAbsolute|Check absolute|
|parse|Path → object|
|format|Object → path|
|normalize|Clean path|
|relative|Relative path|
|sep|Path separator|
|delimiter|Env delimiter|
