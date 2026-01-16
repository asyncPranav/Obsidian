
---

# 🌐 Node.js `http` / `https` Module — COMPLETE NOTES 

---

## 1️⃣ What is `http` / `https` module?

### 📌 Definition

The `http` and `https` modules allow Node.js to:

- **Create web servers**
    
- **Handle requests & responses**
    
- **Make API calls**
    

```txt
http  → Normal (not secure)
https → Secure (encrypted using SSL/TLS)
```

👉 Express, Fastify, NestJS are **built on top of `http`**.

---

## 2️⃣ How the Web Works (Simple)

```txt
Client (Browser / Postman)
	|
	| request
    ↓ 
Server (Node.js http)
	|
	| response
    ↓ 
Client
```

Every request has:

- Method (GET, POST, PUT, DELETE)
    
- URL
    
- Headers
    
- Body (optional)
    

---

## 3️⃣ Importing the Module

```js
const http = require("http");
const https = require("https");
```

---

## 4️⃣ Creating a Server (MOST IMPORTANT)

### 🔹 Syntax

`http.createServer((req, res) => {   // handle request });`

### 🔹 Simple Server Example

`const http = require("http");  const server = http.createServer((req, res) => {   res.end("Hello from Node Server"); });  server.listen(3000, () => {   console.log("Server running on port 3000"); });`

### 📤 Output (browser)

`Hello from Node Server`

---

## 5️⃣ `req` (Request Object)

### 📌 What is `req`?

`req` contains **client request data**.

---

### 🔹 Important `req` properties

|Property|Meaning|
|---|---|
|`req.url`|URL path|
|`req.method`|HTTP method|
|`req.headers`|Request headers|
|`req.socket`|Connection info|

---

### 🔹 Example: Access request info

`http.createServer((req, res) => {   console.log(req.url);   console.log(req.method);   console.log(req.headers);    res.end("Check console"); });`

---

## 6️⃣ `res` (Response Object)

### 📌 What is `res`?

Used to **send response back to client**.

---

### 🔹 Important `res` methods

|Method|Use|
|---|---|
|`res.write()`|Write response|
|`res.end()`|End response|
|`res.setHeader()`|Set headers|
|`res.statusCode`|Set status code|

---

### 🔹 Example: Sending JSON response

`http.createServer((req, res) => {   res.setHeader("Content-Type", "application/json");   res.statusCode = 200;    res.end(JSON.stringify({ msg: "Hello API" })); });`

---

## 7️⃣ Routing (Without Express)

### 📌 Routing = handling different URLs

---

### 🔹 Basic Routing Example

`http.createServer((req, res) => {   if (req.url === "/") {     res.end("Home Page");   } else if (req.url === "/about") {     res.end("About Page");   } else {     res.statusCode = 404;     res.end("Page Not Found");   } });`

---

## 8️⃣ HTTP Methods (Very Important)

|Method|Purpose|
|---|---|
|GET|Fetch data|
|POST|Send data|
|PUT|Update data|
|DELETE|Delete data|

---

### 🔹 Method Handling Example

`http.createServer((req, res) => {   if (req.method === "GET") {     res.end("GET request");   } else if (req.method === "POST") {     res.end("POST request");   } });`

---

## 9️⃣ Handling Request Body (POST Data)

### 📌 Important concept

Request body comes as **stream (chunks)**.

---

### 🔹 Reading POST body

`http.createServer((req, res) => {   let body = "";    req.on("data", chunk => {     body += chunk;   });    req.on("end", () => {     console.log(body);     res.end("Data received");   }); });`

---

## 🔟 Headers

### 📌 What are Headers?

Metadata about request/response.

Examples:

- Content-Type
    
- Authorization
    
- User-Agent
    

---

### 🔹 Setting headers

`res.setHeader("Content-Type", "text/plain");`

---

### 🔹 Reading headers

`console.log(req.headers["user-agent"]);`

---

## 1️⃣1️⃣ Status Codes

|Code|Meaning|
|---|---|
|200|OK|
|201|Created|
|400|Bad Request|
|401|Unauthorized|
|404|Not Found|
|500|Server Error|

---

### 🔹 Example

`res.statusCode = 404; res.end("Not Found");`

---

## 1️⃣2️⃣ Serving Files using Stream (BEST PRACTICE)

`const fs = require("fs");  http.createServer((req, res) => {   const stream = fs.createReadStream("file.txt");   stream.pipe(res); });`

✔ Efficient  
✔ Low memory  
✔ Fast

---

## 1️⃣3️⃣ Making HTTP Requests (Client Side)

### 🔹 `http.get()` (GET request)

`http.get("http://example.com", res => {   res.on("data", chunk => {     console.log(chunk.toString());   }); });`

---

### 🔹 `http.request()` (Custom request)

`const options = {   hostname: "example.com",   path: "/",   method: "GET" };  const req = http.request(options, res => {   res.on("data", chunk => {     console.log(chunk.toString());   }); });  req.end();`

---

## 1️⃣4️⃣ https Module (Secure Version)

### 📌 Difference from http

Uses **SSL/TLS encryption**.

`const https = require("https");  https.get("https://dummyjson.com/products/1", res => {   res.on("data", chunk => {     console.log(chunk.toString());   }); });`

---

## 1️⃣5️⃣ HTTP Streams (Very Important)

- `req` → Readable Stream
    
- `res` → Writable Stream
    

👉 That’s why we use:

`req.on("data") res.write()`

---

## 1️⃣6️⃣ Error Handling

`server.on("error", err => {   console.log("Server error:", err); });`

---

## 1️⃣7️⃣ http vs https (Interview Table)

|Feature|http|https|
|---|---|---|
|Security|❌|✅|
|Encryption|❌|✅|
|Port|80|443|

---

## 1️⃣8️⃣ Real-World Use Cases

- REST APIs
    
- File servers
    
- Microservices
    
- Reverse proxy
    
- Backend for React / Angular
    

---

## 🧠 One-Line Memory Tips

- `http` → create server
    
- `req` → client data
    
- `res` → send response
    
- `Streams` → efficient data handling
    
- `https` → secure communication
    

---

## 🎯 Interview One-Liners

- Node.js uses the http module to handle request-response cycles.
    
- Request and response are streams.
    
- Express is built on top of http module.