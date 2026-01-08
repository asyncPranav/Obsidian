
---

# 🖥️ Node.js `os` Module – **Complete Detailed Notes**

---

## 1️⃣ What is the `os` module?

The **`os` module** is a **built-in Node.js core module** that provides information about the **Operating System** on which your Node.js program is running.

### 📌 What kind of information?

- CPU details
    
- Memory (RAM) info
    
- OS type and platform
    
- User info
    
- System uptime
    
- Network interfaces
    

---

## 2️⃣ Why do we need the `os` module?

In real applications, we often need to:

- Check available memory
    
- Decide number of worker threads
    
- Log system information
    
- Debug performance issues
    
- Write cross-platform code
    

📌 Example:

```js
if (os.platform() === "win32") {
  // Windows-specific logic
}
```

---

## 3️⃣ Importing the os module

```js
const os = require("os");
```

No installation needed.

---

## 4️⃣ Important Concepts (Before Methods)

### 🔹 CPU Core

A **CPU core** is an independent processing unit.

More cores → better parallelism.

---

### 🔹 Thread Pool (Node.js)

Node.js uses a **libuv thread pool** (default size = 4).

`os.cpus()` helps understand system capability.

---

# 🔥 OS MODULE METHODS (VERY DETAILED)

---

## 🔹 1. `os.platform()`

### 🧠 What it does

Returns the **platform** on which Node.js is running.

---

### Example

```js
const os = require("os");
console.log(os.platform());
```

### Output

```sh
win32    → Windows
linux    → Linux
darwin   → macOS
```

---

### ✅ Use case

- Platform-specific code
    
- OS-based conditions
    

---

## 🔹 2. `os.type()`

### 🧠 What it does

Returns the **OS name**.

---

### Example

```js
console.log(os.type());
```

### Output

```sh
Windows_NT
Linux
Darwin
```

---

### Difference: `platform` vs `type`

|Method|Meaning|
|---|---|
|platform|Node platform|
|type|OS name|

---

## 🔹 3. `os.release()`

### 🧠 What it does

Returns the **OS version**.

---

### Example

```js
console.log(os.release());
```

### Output

```sh
10.0.22631
```

---

### ✅ Use case

- Debugging OS-specific bugs
    
- Logging environment info
    

---

## 🔹 4. `os.arch()`

### 🧠 What it does

Returns the **CPU architecture**.

---

### Example

```js
console.log(os.arch());
```

### Output

```sh
x64
arm
arm64
```

---

### ✅ Use case

- Installing correct binaries
    
- Native module support
    

---

## 🔹 5. `os.cpus()` ⭐ VERY IMPORTANT

### 🧠 What it does

Returns **information about each CPU core**.

---

### Example

```js
const cpus = os.cpus();
console.log(cpus);
```

### Sample Output (simplified)

```sh
[
  {
    model: 'Intel(R) Core(TM) i7',
    speed: 2600,
    times: {
      user: 123456,
      nice: 0,
      sys: 34567,
      idle: 987654,
      irq: 0
    }
  },
  ...
]
```

---

### Useful properties

- `model` → CPU name
    
- `speed` → MHz
    
- `times` → CPU usage
    

---

### Example: Number of cores

```js
console.log(os.cpus().length);
```

### Output

```sh
8
```

---

### ✅ Use case

- Creating worker threads
    
- Load balancing
    
- Performance tuning
    

---

## 🔹 6. `os.totalmem()`

### 🧠 What it does

Returns total system memory in **bytes**.

---

### Example

```js
console.log(os.totalmem());
```

### Output

```sh
17179869184
```

---

### Convert to GB

`console.log((os.totalmem() / 1024 / 1024 / 1024).toFixed(2) + " GB");`

---

### ✅ Use case

- Resource monitoring
    
- Memory limits
    

---

## 🔹 7. `os.freemem()`

### 🧠 What it does

Returns available free memory in **bytes**.

---

### Example

`console.log(os.freemem());`

---

### Convert to GB

`console.log((os.freemem() / 1024 / 1024 / 1024).toFixed(2) + " GB");`

---

### ✅ Use case

- Memory warnings
    
- Avoid crashes
    

---

## 🔹 8. `os.uptime()`

### 🧠 What it does

Returns system uptime in **seconds**.

---

### Example

`console.log(os.uptime());`

---

### Convert to hours

`console.log((os.uptime() / 3600).toFixed(2) + " hours");`

---

### ✅ Use case

- Monitoring
    
- System health
    

---

## 🔹 9. `os.userInfo()`

### 🧠 What it does

Returns info about the **current user**.

---

### Example

`console.log(os.userInfo());`

### Output

`{   uid: -1,   gid: -1,   username: 'Admin',   homedir: 'C:\\Users\\Admin',   shell: null }`

---

### ✅ Use case

- User-specific files
    
- Permissions
    

---

## 🔹 10. `os.homedir()`

### 🧠 What it does

Returns home directory of current user.

---

### Example

`console.log(os.homedir());`

### Output

`C:\Users\Admin`

---

### ✅ Use case

- Storing configs
    
- User data
    

---

## 🔹 11. `os.hostname()`

### 🧠 What it does

Returns system hostname.

---

### Example

`console.log(os.hostname());`

---

### ✅ Use case

- Logging
    
- Distributed systems
    

---

## 🔹 12. `os.networkInterfaces()`

### 🧠 What it does

Returns network interface details.

---

### Example

`console.log(os.networkInterfaces());`

---

### Sample Output (simplified)

`{   Ethernet: [     {       address: '192.168.1.10',       family: 'IPv4',       internal: false     }   ] }`

---

### ✅ Use case

- Network debugging
    
- Server IP detection
    

---

## 🔹 13. `os.tmpdir()`

### 🧠 What it does

Returns default temp directory.

---

### Example

`console.log(os.tmpdir());`

---

### ✅ Use case

- Temporary files
    
- File uploads
    

---

## 🔹 14. `os.endianness()`

### 🧠 What it does

Returns CPU byte order.

---

### Example

`console.log(os.endianness());`

### Output

`LE`

---

### ✅ Use case

- Binary data processing
    
- Low-level systems
    

---

## 🔹 15. `os.constants`

### 🧠 What it does

Provides OS-specific constants.

---

### Example

`console.log(os.constants.signals.SIGTERM);`

---

### ✅ Use case

- Process control
    
- Signal handling
    

---

# ⭐ REAL-WORLD MINI PROJECT

`const os = require("os");  console.log("System Info"); console.log("OS:", os.type()); console.log("Platform:", os.platform()); console.log("CPU cores:", os.cpus().length); console.log("Total RAM:", (os.totalmem()/1e9).toFixed(2), "GB"); console.log("Free RAM:", (os.freemem()/1e9).toFixed(2), "GB");`

---

# 🧠 INTERVIEW GOLD LINES

- `os` module gives **system-level info**
    
- Used for **performance & monitoring**
    
- Helps write **cross-platform apps**
    

---

# 🔁 ONE-PAGE QUICK REVISION TABLE

|Method|Purpose|
|---|---|
|platform|OS platform|
|type|OS name|
|release|OS version|
|arch|CPU architecture|
|cpus|CPU info|
|totalmem|Total RAM|
|freemem|Free RAM|
|uptime|System uptime|
|userInfo|Current user|
|homedir|Home directory|
|hostname|Host name|
|networkInterfaces|Network info|
|tmpdir|Temp directory|
|endianness|Byte order|
|constants|OS constants|

---

# 🏁 FINAL MEMORY LINE

> **`path` → files & directories  
> `os` → system & hardware information**