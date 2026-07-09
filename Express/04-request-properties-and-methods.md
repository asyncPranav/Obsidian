
---

# What is `req` in Express?

`req` (request object) represents **everything the client sends to server**.

When user visits:

```
http://localhost:3000/user/10?sort=asc
```

Client sends:

- URL
- params
- query
- headers
- body
- method

All stored inside **req object**

```js
app.get('/', (req, res) => {  
    console.log(req);  
});
```

---

# app.use(express.json())

`app.use(express.json())` is a built-in middleware in **Express.js** that parses incoming requests with **JSON** data.

### Why do we use it?

When a client (browser, frontend, Postman, etc.) sends JSON data to your server, the data arrives as a raw stream of bytes. `express.json()` reads that stream and converts it into a JavaScript object.

```javascript
const express = require("express");
const app = express();

app.use(express.json());

app.post("/user", (req, res) => {
    console.log(req.body);
    res.send(req.body);
});

app.listen(3000);
```

If you send this JSON:

```json
{
  "name": "John",
  "age": 25
}
```

Then:

```javascript
req.body
```

becomes

```javascript
{
  name: "John",
  age: 25
}
```

---

## What happens if you don't use it?

Without:

```javascript
app.use(express.json());
```

`req.body` will usually be `undefined` for JSON requests.

Example:

```javascript
const express = require("express");
const app = express();

// app.use(express.json());  <-- Missing

app.post("/user", (req, res) => {
    console.log(req.body);
    res.send("Done");
});
```

If you send:

```json
{
  "name": "John"
}
```

Output:

```javascript
undefined
```

because Express doesn't automatically parse JSON request bodies.

---

## When should you use it?

Use it whenever your API receives JSON data.

For example:

- ✅ Login API
    

```json
{
  "email": "abc@gmail.com",
  "password": "123456"
}
```

- ✅ Register API
    
```json
{
  "username": "Sam",
  "age": 20
}
```

- ✅ Update profile API
    
```json
{
  "city": "Delhi"
}
```

---

## When is it NOT needed?

If your routes never receive JSON request bodies, you may not need it.

For example:

```javascript
app.get("/users", (req, res) => {
    res.send("Users");
});
```

A GET request typically doesn't have a JSON body, so `express.json()` isn't required for that endpoint.

---

## How it works internally

When a request like this arrives:

```
POST /user
Content-Type: application/json

{
  "name": "John"
}
```

The middleware:

1. Checks whether the `Content-Type` is `application/json`.
    
2. Reads the raw request body.
    
3. Parses it using `JSON.parse()`.
    
4. Stores the resulting object in `req.body`.
    
5. Calls `next()` so the next middleware or route handler runs.
    

---

### Summary

|With `express.json()`|Without `express.json()`|
|---|---|
|JSON is parsed automatically|JSON is not parsed|
|`req.body` contains a JavaScript object|`req.body` is usually `undefined`|
|Suitable for POST, PUT, PATCH APIs that accept JSON|JSON request bodies cannot be accessed directly|

So, `app.use(express.json())` is essential whenever your Express server needs to receive and work with JSON data from clients. Without it, Express doesn't know how to interpret the JSON payload, and `req.body` won't contain the parsed data.

---

#DOUBT - **how json is send and how will i get to know when req body will send json**


This is an excellent question. The key idea is that **`req.body` doesn't "send" JSON**. The **client** (browser, frontend, Postman, mobile app, etc.) sends JSON to the server, and Express receives it in `req.body` after `express.json()` parses it.

## How is JSON sent?

A client sends an HTTP request with:

- Method: `POST`, `PUT`, or `PATCH` (most commonly)
    
- Header:
    
    ```
    Content-Type: application/json
    ```
    
- Body:
    
    ```json
    {
      "name": "Alice",
      "age": 22
    }
    ```
    

For example, if you use Postman:

- Method: `POST`
    
- URL:
    
    ```
    http://localhost:3000/user
    ```
    
- Headers:
    
    ```
    Content-Type: application/json
    ```
    
- Body (raw → JSON):
    
    ```json
    {
      "name": "Alice",
      "age": 22
    }
    ```
    

Your Express server:

```javascript
const express = require("express");
const app = express();

app.use(express.json());

app.post("/user", (req, res) => {
    console.log(req.body);
    res.send("Received");
});

app.listen(3000);
```

Output:

```javascript
{ name: "Alice", age: 22 }
```

---

## How do you know when `req.body` contains JSON?

Look at the request's **`Content-Type`** header.

If the client sends:

```
Content-Type: application/json
```

and you've added:

```javascript
app.use(express.json());
```

then Express parses the JSON and stores it in:

```javascript
req.body
```

You can even check the content type:

```javascript
app.post("/user", (req, res) => {
    console.log(req.headers["content-type"]);
    console.log(req.body);
});
```

Output:

```
application/json
{ name: 'Alice', age: 22 }
```

---

## When does `req.body` exist?

`req.body` is used when the client sends data in the **request body**.

For example:

### POST

```http
POST /login
```

Body:

```json
{
  "email": "abc@gmail.com",
  "password": "1234"
}
```

Access it with:

```javascript
console.log(req.body.email);
```

---

### PUT

```http
PUT /user/1
```

Body:

```json
{
  "name": "Bob"
}
```

Access it with:

```javascript
console.log(req.body.name);
```

---

### PATCH

```http
PATCH /user/1
```

Body:

```json
{
  "age": 25
}
```

Access it with:

```javascript
console.log(req.body.age);
```

---

## What about GET requests?

Usually, **GET requests do not send a request body**.

Instead, they send data in the URL.

Example:

```
GET /user?id=10
```

Then you use:

```javascript
req.query.id
```

not

```javascript
req.body
```

---

## Quick comparison

|Client sends data in...|Example|Express reads it using|
|---|---|---|
|URL parameter|`/user/10`|`req.params.id`|
|Query string|`/user?id=10`|`req.query.id`|
|JSON request body|`{ "name": "Alice" }`|`req.body.name`|

### Easy way to remember

Imagine you're filling out a form:

- **URL parameters (`req.params`)** → Part of the address (e.g., `/users/5`)
    
- **Query parameters (`req.query`)** → Extra information after `?` (e.g., `?sort=name`)
    
- **Request body (`req.body`)** → The actual form data or payload sent to the server (often as JSON)
    

Whenever you're creating or updating data (like logging in, registering, or saving information), the client will typically send that data in the **request body**, and you'll access it through `req.body` after enabling `express.json()`.

---

# **✅Request Methods**



# 1. req.params

## What is it?

Used to get **route parameters** from URL.

Route parameters are dynamic values in URL like:

```
/user/:id
```

---

## Syntax

```js
req.params.parameterName
```

---

## Example

```js
app.get('/user/:id', (req, res) => {  
  
    console.log(req.params);  
  
    res.send("done");  
  
});
```

If user visits:

```
/user/10
```

Output:

```json
{ id: '10' }
```

Access specific value:

```js
req.params.id
```

---

## Multiple params

```js
app.get('/user/:id/book/:bookid', (req,res)=>{  
  
 console.log(req.params.id)  
 console.log(req.params.bookid)  
  
 res.send("ok")  
  
})
```

URL:

```
/user/5/book/20
```

Output:

```
5  
20
```

---

# 2. req.query

## What is it?

Used to get **query string values**

Query string appears after `?`

Example:

```
/search?q=node&page=2
```

---

## Syntax

```js
req.query.name
```

---

## Example

```js
app.get('/search', (req, res) => {  
  
    console.log(req.query);  
  
    res.send("done");  
  
});
```

URL:

```
/search?q=node&page=2
```

Output:

```json
{ q: 'node', page: '2' }
```

Access values:

```js
req.query.q  
req.query.page
```

---

## When to use

Use query for:

- search
- filtering
- sorting
- pagination

---

# 3. req.body

## What is it?

Used to get **data sent in request body**  
(mainly POST / PUT requests)

Example sending JSON:

```json
{  
"name":"john",  
"age":20  
}
```

---

## Important

You must enable middleware:

```js
app.use(express.json())  
app.use(express.urlencoded({extended:true}))
```

---

## Example

```js
app.post('/user', (req, res) => {  
  
    console.log(req.body);  
  
    res.send("done");  
  
});
```

Request body:

```json
{  
"name":"john",  
"age":20  
}
```

Output:

```json
{ name: 'john', age: 20 }
```

---

# 4. req.headers

## What is it?

Contains **all request headers** sent by client

Headers contain:

- browser info
- content type
- authorization
- cookies

---

## Example

```js
app.get('/', (req,res)=>{  
  
 console.log(req.headers)  
  
 res.send("ok")  
  
})
```

Output example:

```
{  
 host: 'localhost:3000',  
 'user-agent': 'chrome',  
 accept: '*/*'  
}
```

---

# 5. req.method

## What is it?

Tells which HTTP method used

GET  
POST  
PUT  
DELETE

---

## Example

```js
app.get('/', (req,res)=>{  
  
 console.log(req.method)  
  
 res.send("ok")  
  
})
```

Output:

```
GET
```

---

# 6. req.url

## What is it?

Returns full URL requested

Example:

```
/user/10?page=2
```

---

## Example

```js
app.get('/user/:id', (req,res)=>{  
  
 console.log(req.url)  
  
 res.send("ok")  
  
})
```

Output:

```
/user/10?page=2
```

---

# 7. req.path

## What is it?

Returns **only path**, without query

Example:

```
/user/10
```

---

## Example

```js
console.log(req.path)
```

Output:

```
/user/10
```

---

# 8. req.hostname

## What is it?

Returns domain name

Example:

```
localhost
```

---

## Example

```js
console.log(req.hostname)
```

---

# 9. req.ip

## What is it?

Returns client IP address

```js
console.log(req.ip)
```

Example output:

```
127.0.0.1
```

---

# 10. req.protocol

## What is it?

Returns protocol used

http  
https

```js
console.log(req.protocol)
```

---

# 11. req.secure

## What is it?

Returns true if HTTPS

```js
console.log(req.secure)
```

Output:

```
false
```

---

# 12. req.get()

## What is it?

Used to get header value

## Syntax

```js
req.get("header-name")
```

---

## Example

```js
app.get('/', (req,res)=>{  
  
 console.log(req.get("User-Agent"))  
  
 res.send("ok")  
  
})
```

---

# 13. req.cookies

## What is it?

Gets cookies sent by client

Requires middleware:

```js
npm install cookie-parser
```

```js
app.use(require("cookie-parser")())
```

---

## Example

```js
console.log(req.cookies)
```

---

# 14. req.originalUrl

## What is it?

Returns original full URL

```js
console.log(req.originalUrl)
```

---

# 15. req.baseUrl

## What is it?

Returns router base URL

Used in router middleware

---

# Example

```js
app.use('/admin', router)
```

Inside router:

```
req.baseUrl = /admin
```

---

# 16. req.xhr

## What is it?

Returns true if AJAX request

```js
console.log(req.xhr)
```

---

# Request Methods

These are functions on req

---

# 1. req.get(field)

### What it does

Gets value of a request header.

### Syntax

```js
req.get("Header-Name")
```

### Example

```js
app.get('/', (req,res)=>{  
    console.log(req.get("User-Agent"))  
    res.send("ok")  
})
```

Use: read headers like auth token, content-type, browser info.

---

# 2. req.header(field)

Same as `req.get()`

```js
req.header("Authorization")
```

---

# 3. req.accepts(type)

### What it does

Checks if client accepts given response type.

### Syntax

```js
req.accepts("json")
```

### Example

```js
app.get('/', (req,res)=>{  
  
    if(req.accepts("json")){  
        res.json({msg:"json"})  
    }else{  
        res.send("text")  
    }  
  
})
```

Use: content negotiation.

---

# 4. req.acceptsCharsets()

### What it does

Checks accepted charset

```js
req.acceptsCharsets("utf-8")
```

---

# 5. req.acceptsEncodings()

### What it does

Checks accepted encoding

```js
req.acceptsEncodings("gzip")
```

---

# 6. req.acceptsLanguages()

### What it does

Checks accepted language

```js
req.acceptsLanguages("en")
```

---

# 7. req.is(type)

### What it does

Checks request content-type

### Syntax

```js
req.is("application/json")
```

### Example

```js
app.post('/', (req,res)=>{  
  
    if(req.is("application/json")){  
        res.send("JSON data")  
    }else{  
        res.send("Other type")  
    }  
  
})
```

Use: validate body type.

---

# 8. req.param(name) ⚠ deprecated

### What it does

Gets value from params, body, or query.

```js
req.param("id")
```

Avoid using (deprecated)

---

# 9. req.range(size)

### What it does

Parses Range header

Used for file streaming.

```js
req.range(1000)
```

Rarely used.

---

# 10. req.app

Returns Express app instance

```js
req.app
```

Example:

```js
req.app.get("env")
```

---

# 11. req.res

Reference to response object

```js
req.res
```

Rarely used.

---

# 12. req.next

Reference to next middleware

```js
req.next
```

Rarely used directly.

---

# Most Important Request Methods (Real Projects)

You will mainly use:

-> req.get()  
-> req.header()  
-> req.accepts()  
-> req.is()

Others are rarely used.

---

# Example Using Multiple Methods

app.post('/test',(req,res)=>{  
  
 console.log(req.get("Content-Type"))  
  
 console.log(req.accepts("json"))  
  
 console.log(req.is("application/json"))  
  
 res.send("done")  
  
})

---

# Summary

Yes — Express request methods include:

req.get()  
req.header()  
req.accepts()  
req.acceptsCharsets()  
req.acceptsEncodings()  
req.acceptsLanguages()  
req.is()  
req.param() (deprecated)  
req.range()

But **only these are commonly used**:

req.get()  
req.accepts()  
req.is()

---

# Most Important for Beginners

req.params  
req.query  
req.body  
req.headers  
req.method  
req.url  
req.path  
req.get()  
req.cookies

---

# Difference (Very Important)

URL:

```
/user/5?sort=asc
```

req.params.id → 5  
req.query.sort → asc  
req.body → POST data