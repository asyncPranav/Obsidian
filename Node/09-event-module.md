
---

# ⚡ Node.js `events` Module – Complete Detailed Notes

---

## 1️⃣ What is the `events` module?

The **`events` module** is a **built-in core module** that allows us to:

- Create **custom events**
    
- Listen to those events
    
- Trigger (emit) events
    
- Handle async workflows using events
    

👉 Node.js works on **event-driven architecture**

---

## 2️⃣ Why does Node.js need events?

Node.js is:

- Single-threaded
    
- Non-blocking
    
- Asynchronous
    

So instead of waiting, Node.js:

- Registers an event
    
- Continues execution
    
- Executes callback when event occurs
    

📌 Example:

- File read complete → `data` event
    
- Server request → `request` event
    

---

## 3️⃣ Real-life analogy (Very Easy)

🎓 **Teacher example**

- Teacher says: “If I say _start_, begin writing”
    
- Students **listen**
    
- Teacher says _start_
    
- Students act
    

👉 Teacher = **Emitter**  
👉 Students = **Listeners**  
👉 "start" = **Event**

---

## 4️⃣ Importing the events module

```js
const EventEmitter = require("events");
```

---

## 5️⃣ What is EventEmitter?

`EventEmitter` is a **class** provided by the `events` module.

We create objects from it to:

- Emit events
    
- Listen to events
    

---

### Creating an EventEmitter object

```js
const emitter = new EventEmitter();
```

---

## 6️⃣ Core Terminology (VERY IMPORTANT)

|Term|Meaning|
|---|---|
|Event|Something that happened|
|Emit|Trigger an event|
|Listener|Function waiting for event|
|Callback|Function executed on event|
|Emitter|Object that emits events|

---

# 🔥 CORE METHODS OF EVENTEMITTER (IN DETAIL)

---

## 🔹 1. `emitter.on()` ⭐ MOST IMPORTANT

### 🧠 What it does

Registers a **listener** for an event.

---

### Syntax

```js
emitter.on(eventName, callback)
```

---

### Example

```js
const EventEmitter = require("events");
const emitter = new EventEmitter();

emitter.on("greet", () => {
  console.log("Hello User!");
});

emitter.emit("greet");
```

---

### Output

```js
Hello User!
```

---

### Key points

- Can have **multiple listeners**
    
- Executes **every time** event is emitted
    

---

## 🔹 2. `emitter.emit()` ⭐

### 🧠 What it does

Triggers an event.

---

### Example with data passing

`emitter.on("sum", (a, b) => {   console.log(a + b); });  emitter.emit("sum", 10, 20);`

---

### Output

`30`

---

### Important

- Arguments passed to `emit` are received by listener
    

---

## 🔹 3. `emitter.once()`

### 🧠 What it does

Runs listener **only one time**.

---

### Example

`emitter.once("login", () => {   console.log("User logged in"); });  emitter.emit("login"); emitter.emit("login");`

---

### Output

`User logged in`

---

### Use case

- One-time events (initialization)
    

---

## 🔹 4. `emitter.off()` / `emitter.removeListener()`

### 🧠 What it does

Removes a specific listener.

---

### Example

`function handler() {   console.log("Event handled"); }  emitter.on("test", handler); emitter.off("test", handler);  emitter.emit("test");`

---

### Output

`(no output)`

---

### Use case

- Cleanup
    
- Avoid memory leaks
    

---

## 🔹 5. `emitter.removeAllListeners()`

### 🧠 What it does

Removes **all listeners** for an event.

---

### Example

`emitter.on("data", () => console.log("Listener 1")); emitter.on("data", () => console.log("Listener 2"));  emitter.removeAllListeners("data"); emitter.emit("data");`

---

### Output

`(no output)`

---

## 🔹 6. `emitter.listenerCount()`

### 🧠 What it does

Returns number of listeners.

---

### Example

`emitter.on("event", () => {}); emitter.on("event", () => {});  console.log(emitter.listenerCount("event"));`

---

### Output

`2`

---

## 🔹 7. `emitter.listeners()`

### 🧠 What it does

Returns array of listener functions.

---

### Example

`console.log(emitter.listeners("event"));`

---

### Use case

- Debugging
    
- Inspection
    

---

## 🔹 8. `emitter.eventNames()`

### 🧠 What it does

Returns list of registered event names.

---

### Example

`emitter.on("login", () => {}); emitter.on("logout", () => {});  console.log(emitter.eventNames());`

---

### Output

`[ 'login', 'logout' ]`

---

## 🔹 9. `emitter.setMaxListeners()`

### 🧠 What it does

Sets maximum listeners limit.

Default = **10**

---

### Example

`emitter.setMaxListeners(20);`

---

### Why?

Prevents **memory leaks**

---

## 🔹 10. `emitter.getMaxListeners()`

`console.log(emitter.getMaxListeners());`

---

# ⚙️ SPECIAL EVENTS

---

## 🔹 `error` event ⭐ VERY IMPORTANT

### 🧠 Rule

If `error` event is emitted and **no listener exists**, Node.js crashes.

---

### Example

`emitter.on("error", (err) => {   console.log("Error occurred:", err.message); });  emitter.emit("error", new Error("Something broke"));`

---

### Output

`Error occurred: Something broke`

---

### Without listener → ❌ crash

---

## 🔄 Event Execution Order

- Listeners run in **order of registration**
    
- Synchronous by default
    

---

### Example

`emitter.on("event", () => console.log("First")); emitter.on("event", () => console.log("Second"));  emitter.emit("event");`

---

### Output

`First Second`

---

# 🔀 Events + Async Code

`emitter.on("asyncEvent", async () => {   await new Promise(res => setTimeout(res, 1000));   console.log("Async done"); });  emitter.emit("asyncEvent");`

---

# 🔧 Inheriting from EventEmitter

### Why?

Many Node.js classes extend EventEmitter:

- http.Server
    
- streams
    
- fs streams
    

---

### Example

`const EventEmitter = require("events");  class MyEmitter extends EventEmitter {}  const myEmitter = new MyEmitter();  myEmitter.on("hello", () => console.log("Hello!")); myEmitter.emit("hello");`

---

# 🌍 Real-World Examples

---

## Example 1: Login System

``emitter.on("login", user => {   console.log(`${user} logged in`); });  emitter.emit("login", "Pranav");``

---

## Example 2: File Upload Progress

``emitter.on("progress", percent => {   console.log(`Uploaded ${percent}%`); });``

---

## Example 3: Order Processing

`orderPlaced → paymentDone → shipped → delivered`

Each step = event

---

# 🧠 How Node.js Internally Uses Events

|Module|Event|
|---|---|
|http|request|
|fs|data, end|
|streams|data, end|
|process|exit|

---

# 🔁 QUICK REVISION TABLE

|Method|Purpose|
|---|---|
|on|Listen to event|
|emit|Trigger event|
|once|One-time listener|
|off|Remove listener|
|removeAllListeners|Remove all|
|listenerCount|Count listeners|
|listeners|List listeners|
|eventNames|List events|
|setMaxListeners|Set limit|

---

# 🎯 INTERVIEW GOLD POINTS

- Node.js is **event-driven**
    
- `EventEmitter` is synchronous
    
- `error` event must be handled
    
- Many core modules extend EventEmitter
    

---

# 🏁 FINAL ONE-LINE MEMORY

> **Events = heart of Node.js architecture**