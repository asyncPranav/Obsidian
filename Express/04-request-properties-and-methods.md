
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

app.get('/', (req,res)=>{  
  
 console.log(req.method)  
  
 res.send("ok")  
  
})

Output:

GET

---

# 6. req.url

## What is it?

Returns full URL requested

Example:

/user/10?page=2

---

## Example

app.get('/user/:id', (req,res)=>{  
  
 console.log(req.url)  
  
 res.send("ok")  
  
})

Output:

/user/10?page=2

---

# 7. req.path

## What is it?

Returns **only path**, without query

Example:

/user/10

---

## Example

console.log(req.path)

Output:

/user/10

---

# 8. req.hostname

## What is it?

Returns domain name

Example:

localhost

---

## Example

console.log(req.hostname)

---

# 9. req.ip

## What is it?

Returns client IP address

console.log(req.ip)

Example output:

127.0.0.1

---

# 10. req.protocol

## What is it?

Returns protocol used

http  
https

console.log(req.protocol)

---

# 11. req.secure

## What is it?

Returns true if HTTPS

console.log(req.secure)

Output:

false

---

# 12. req.get()

## What is it?

Used to get header value

## Syntax

req.get("header-name")

---

## Example

app.get('/', (req,res)=>{  
  
 console.log(req.get("User-Agent"))  
  
 res.send("ok")  
  
})

---

# 13. req.cookies

## What is it?

Gets cookies sent by client

Requires middleware:

npm install cookie-parser

app.use(require("cookie-parser")())

---

## Example

console.log(req.cookies)

---

# 14. req.originalUrl

## What is it?

Returns original full URL

console.log(req.originalUrl)

---

# 15. req.baseUrl

## What is it?

Returns router base URL

Used in router middleware

---

# Example

app.use('/admin', router)

Inside router:

req.baseUrl = /admin

---

# 16. req.xhr

## What is it?

Returns true if AJAX request

console.log(req.xhr)

---

# Request Methods

These are functions on req

---

# req.accepts()

Checks accepted response type

req.accepts("json")

---

# req.is()

Check request content type

req.is("application/json")

---

# Complete Example (All Together)

app.post('/user/:id', (req,res)=>{  
  
 console.log("params:", req.params)  
  
 console.log("query:", req.query)  
  
 console.log("body:", req.body)  
  
 console.log("method:", req.method)  
  
 console.log("url:", req.url)  
  
 console.log("headers:", req.headers)  
  
 res.send("done")  
  
})

Request:

POST /user/10?page=2

Body:

{name:"john"}

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

/user/5?sort=asc

req.params.id → 5  
req.query.sort → asc  
req.body → POST data