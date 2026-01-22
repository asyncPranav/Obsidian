
---

# **1️⃣ EventEmitter in Node.js**

### 🔹 What it is:

- Node.js has a built-in module called `events`.
    
- `EventEmitter` is a **class** that allows objects to **emit events** and **listen for events**.
    
- It’s like **custom signals** your program can send and respond to.
    

---

### 🔹 Why it’s useful:

- Useful for async programming.
    
- Many Node APIs (like `fs`, `http`) internally use EventEmitter.
    
- You can trigger actions **when something happens**.
    

---

### 🔹 Basic example:

```js
const EventEmitter = require("events");

const myEmitter = new EventEmitter();

// Listen to an event
myEmitter.on("greet", (name) => {
  console.log(`Hello ${name}!`);
});

// Emit the event
myEmitter.emit("greet", "Alice");
```

✅ Output:

```sh
Hello Alice!
```

---

### 🔹 Explanation:

- `.on("eventName", callback)` → registers a **listener**.
    
- `.emit("eventName", args...)` → **triggers the event**.
    
- You can have **multiple listeners** for the same event.
    
- Listeners execute **synchronously** in the order they were added.
    

---

### 🔹 Extra methods:

|Method|Description|
|---|---|
|`.once()`|Listen to an event **only once**|
|`.removeListener()`|Remove a specific listener|
|`.removeAllListeners()`|Remove all listeners for an event|
|`.listenerCount()`|Get number of listeners|

---

# **2️⃣ Event Listener**

- A **listener** is the function you attach to an event using `.on()` or `.once()`.
    
- **Triggered automatically** when the event is emitted.
    
- Can accept **arguments** passed via `.emit`.
    

---

### 🔹 Example with multiple listeners:

```js
myEmitter.on("greet", (name) => console.log("Hi there!"));
myEmitter.on("greet", (name) => console.log(`Hello ${name}!`));

myEmitter.emit("greet", "Bob");
```

✅ Output:

```sh
Hi there!
Hello Bob!
```

---

# **3️⃣ Pipe in Node.js**

### 🔹 What it is:

- `pipe` is used **with streams**.
    
- Allows you to **take data from a readable stream and send it to a writable stream**.
    
- Automatically **handles buffering and flow control**.
    

---

### 🔹 Example: File copy

```js
const fs = require("fs");

const readStream = fs.createReadStream("input.txt");
const writeStream = fs.createWriteStream("output.txt");

readStream.pipe(writeStream);

console.log("File is being copied...");
```

- This copies `input.txt` → `output.txt`.
    
- Node handles **chunks** automatically.
    
- More efficient than reading the whole file into memory.
    

---

### 🔹 Pipes with EventEmitter:

- Streams are also **EventEmitters**.
    
- You can listen to events like:
    

```js
readStream.on("data", (chunk) => console.log("Received chunk", chunk.length));
readStream.on("end", () => console.log("Reading done"));
```

- So **streams + pipe** combine **EventEmitter + flow control**.
    

---

# ✅ Quick Summary Table

|Concept|Use|Example|
|---|---|---|
|EventEmitter|Create custom events|`myEmitter.emit("event")`|
|Listener|Function attached to an event|`myEmitter.on("event", callback)`|
|Pipe|Connect readable stream → writable stream|`readStream.pipe(writeStream)`|

---

# 🔑 Key Notes for Beginners

1. **EventEmitter** is everywhere in Node.js (fs, http, net, streams).
    
2. **Listener** reacts automatically when event happens.
    
3. **pipe** is for efficiently moving data between streams.
    
4. **Streams themselves are EventEmitters**, so you can listen to `data`, `end`, `error` events.