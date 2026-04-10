
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