
---

```js
const readline = require("readline");
const { MongoClient } = require("mongodb");

const uri =
"mongodb+srv://namasteNode:namasteNodePass@namastenode.jqcz9u9.mongodb.net/?appName=namasteNode";
const client = new MongoClient(uri);

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

// Wrap rl.question in a promise to use await
function ask(questionText) {
  return new Promise((resolve) => {
    rl.question(questionText, (input) => resolve(input));
  });
}

// Add Todo
async function addTodo() {
  const text = await ask("Enter todo text: ");
  const db = client.db("todoApp");
  const todos = db.collection("todos");

  await todos.insertOne({
    text,
    completed: false,
    createdAt: new Date(),
  });

  console.log(`Added todo: ${text}`);
}

// List Todos
async function listTodo() {
  const db = client.db("todoApp");
  const todos = db.collection("todos");

  const all = await todos.find().toArray();

  if (all.length === 0) {
    console.log("No todos found");
  } else {
    console.log("\nYour Todos:");
    all.forEach((t, i) => {
      console.log(`${i + 1}. ${t.text} [${t.completed ? "✔" : "❌"}]`);
    });
  }
}

// Mark Todo as done
async function doneTodo() {
  const text = await ask("Enter todo text to mark as done: ");
  const db = client.db("todoApp");
  const todos = db.collection("todos");

  const result = await todos.updateOne({ text }, { $set: { completed: true } });

  if (result.matchedCount === 0) {
    console.log("Todo not found");
  } else {
    console.log(`Todo marked as done: ${text}`);
  }
}

// Delete Todo
async function deleteTodo() {
  const text = await ask("Enter todo text to delete: ");
  const db = client.db("todoApp"); // fixed DB name
  const todos = db.collection("todos");

  const result = await todos.deleteOne({ text });

  if (result.deletedCount === 0) {
    console.log("Todo not found");
  } else {
    console.log(`Todo deleted: ${text}`);
  }
}

// Show Menu
async function showMenu() {
  console.log("\n====== TODO APP ======");
  console.log("1. Add Todo");
  console.log("2. List Todos");
  console.log("3. Done Todo");
  console.log("4. Delete Todo");
  console.log("5. Exit");

  const choice = await ask("Enter your choice: ");

  switch (choice) {
    case "1":
      await addTodo();
      break;
    case "2":
      await listTodo();
      break;
    case "3":
      await doneTodo();
      break;
    case "4":
      await deleteTodo();
      break;
    case "5":
      console.log("Exiting...");
      rl.close();
      await client.close();
      return; // exit
    default:
      console.log("Invalid choice, try again");
  }

  showMenu(); // loop menu after operation
}

// Connect to DB and start app
async function main() {
  await client.connect();
  console.log("MongoDB connected");
  showMenu(); // start menu
}

main().catch(console.error);
```


## **Why Promisification**

Let’s go **very slow**, **very simple**, and **from absolute beginner level**.  
I’ll assume **you know only basic JS functions**, nothing about Promises or async yet.

---

# 🌱 What problem are we solving?

You want to do this:

```js
const text = ask("Enter your name: ");
console.log(text);
```

But **JavaScript cannot do this directly** because:

- User typing happens **later**
    
- JS does **not stop** and wait automatically
    

So we need a **way to wait** until the user types something.

---

# 🧠 Real-Life Example (Understand FIRST)

Imagine this situation:

> You ask your friend a question on WhatsApp  
> You **cannot continue the conversation** until your friend replies

So you:

1. Ask the question
    
2. Wait
    
3. Friend replies
    
4. Continue
    

JavaScript needs the **same waiting mechanism**.

---

# 🔴 Why `rl.question()` alone is not enough

Look at this code:

```js
rl.question("Enter name: ", (answer) => {
  console.log(answer);
});
console.log("Done");
```

### What happens?

```js
Done
(user types name)
name
```

❌ JavaScript **does NOT wait**  
Because `rl.question()` works in **callback style**

---

# 🧩 Solution Idea (Simple words)

We want:

> “Ask a question and **pause** until user answers”

JavaScript can pause **only if we use `await`**

But:

- `await` works **ONLY with Promises**
    

So we create our **own Promise**.

---

# 🔑 Now let’s understand the code STEP-BY-STEP

---

## 🟢 FULL CODE AGAIN

```js
function ask(questionText) {
  return new Promise((resolve) => {
    rl.question(questionText, (input) => resolve(input));
  });
}
```

---

## 🔹 Line 1

```js
function ask(questionText) {
```

### What this means:

- We are creating a function called `ask`
    
- It takes **one argument**
    
    - `questionText` → text shown to user
        

Example:

```js
ask("Enter your name: ");
```

---

## 🔹 Line 2

```js
return new Promise((resolve) => {
```

### Beginner meaning of Promise:

> A Promise is a **box** that will contain a value **in the future**

Right now:

- We don’t have the answer
    
- User hasn’t typed anything
    
- So Promise is **empty**
    

Later:

- User types something
    
- Promise gets filled
    

---

## 🔹 What is `resolve`?

Think of `resolve` like this:

> “Hey Promise, I got the value now!”

When `resolve(value)` is called:

- Promise is completed
    
- Value becomes available
    

---

## 🔹 Line 3

```js
rl.question(questionText, (input) => resolve(input));
```

Let’s break this VERY slowly 👇

---

### 🔸 `rl.question(...)`

- Shows the question in terminal
    
- Waits for user input
    
- When user presses ENTER → callback runs
    

---

### 🔸 `(input) => { ... }`

- `input` = whatever user typed
    
- Example:
    
    - User types: `Learn MongoDB`
        
    - `input = "Learn MongoDB"`
        

---

### 🔸 `resolve(input)`

This line means:

> “User has answered → store this value inside Promise”

So Promise becomes:

```js
Promise → "Learn MongoDB"
```

---

## 🔹 Line 4

```js
});
```

- Ends Promise
    
- Ends function
    

---

# 🧠 FINAL RESULT OF `ask()`

When you call:

```js
const text = await ask("Enter todo text: ");
```

What actually happens:

1. Question is shown
    
2. Program **pauses**
    
3. User types something
    
4. Promise resolves
    
5. `text` gets the value
    

---

# 📊 Execution Flow (VERY IMPORTANT)

```js
ask() called
│
├─ rl.question shown
│
├─ program WAITS
│
├─ user types input
│
├─ resolve(input) called
│
└─ await continues with value
```

---

# ❓ Why not return input directly?

❌ This will NOT work:

```js
function ask() {
  let value;
  rl.question("Enter:", (input) => {
    value = input;
  });
  return value; // ❌ undefined
}
```

Because:

- `return` runs **before** user types
    
- JavaScript does not wait
    

---

# ✅ Why Promise works

Because `await` tells JS:

> “Do not move forward until Promise is finished”

---

# 🧠 Super Simple Mental Model

```js
ask() = Ask question + WAIT
await ask() = Wait + Get answer
```

---

# 🧪 Tiny Example (Test Your Understanding)

```js
async function test() {
  const name = await ask("Enter your name: ");
  console.log("Hello", name);
}
```

🖥 Terminal:

```js
Enter your name: Vageesh
Hello Vageesh
```

---

# 🏁 Final One-Line Explanation

> We wrap `rl.question()` inside a Promise so that we can pause the program using `await` until the user provides input.