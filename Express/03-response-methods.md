
---


# What is Response in Express?

When client sends request:

Browser → request → Server  
Browser ← response ← Server

Server sends response using **res object**

```js
app.get('/', (req, res) => {  
    res.send("Hello");  
});
```

Here:

- `req` → incoming request
- `res` → outgoing response

---

# 1. res.send() (Most Important)

This method **sends response to client** and **ends the request**

It can send:

- string
- HTML
- object
- array
- buffer
- boolean
- number

---

## Send String

```js
app.get('/', (req, res) => {  
    res.send("Hello World");  
});
```

Browser shows:

```
Hello World
```

---

## Send HTML

```js
app.get('/html', (req, res) => {  
    res.send("<h1>Hello</h1>");  
});
```

Browser renders HTML.

---

## Send Object

```js
app.get('/user', (req, res) => {  
    res.send({  
        name: "John",  
        age: 20  
    });  
});

```
Express automatically converts object → JSON

---

## Send Array

```js
app.get('/users', (req, res) => {  
    res.send(["john", "sam", "alex"]);  
});
```

---

## Send Number

```js
res.send(200);
```

⚠️ This sends HTTP status instead — avoid this.

Use:

```js
res.status(200).send("OK");
```

---

## Important Behavior

res.send() automatically:

- sets content type
- sets status 200
- ends response

---

# 2. res.json()

This method sends **JSON only**

Used in **API development**

```js
app.get('/api', (req, res) => {  
    res.json({  
        success: true,  
        message: "Working"  
    });  
});
```

Difference:

res.send({}) → also JSON  
res.json({}) → explicitly JSON

Best practice → use **res.json()** for APIs

---

# 3. res.status()

Sets HTTP status code

Common status codes:

200 → success  
201 → created  
400 → bad request  
401 → unauthorized  
403 → forbidden  
404 → not found  
500 → server error

---

## Example

```js
app.get('/success', (req, res) => {  
    res.status(200).send("Success");  
});
```

---

## Error Example

```js
app.get('/error', (req, res) => {  
    res.status(500).send("Server error");  
});

```
---

## Chaining (Very Common)

```js
res.status(200).json({  
    message: "OK"  
});
```

---

# 4. res.sendStatus()

This method:

- sets status
- sends default message
- ends response

```js
res.sendStatus(404);
```

Output:

```
Not Found
```

Same as:

```js
res.status(404).send("Not Found");
```

---

# 5. res.redirect()

Redirects user to another route

```js
app.get('/home', (req, res) => {  
    res.redirect('/');  
});

```
User visits `/home` → redirected to `/`

---

## Redirect external site

```js
res.redirect("https://google.com");
```

---

## Redirect with status

```js
res.redirect(301, '/new-page');
```

301 → permanent redirect

---

# 6. res.sendFile()

Sends file to client

```js
res.sendFile(__dirname + "/index.html");
```

Example:

```js
app.get('/', (req, res) => {  
    res.sendFile(__dirname + "/index.html");  
});
```

Used for:

- HTML files
- PDFs
- images
- downloads

---

# 7. res.download()

Forces file download

```js
app.get('/download', (req, res) => {  
    res.download('report.pdf');  
});
```

Browser downloads instead of opening.

---

# 8. res.render()

Used with template engines (EJS, Pug)

```js
app.get('/profile', (req, res) => {  
    res.render('profile', {  
        name: "John"  
    });  
});
```

This renders:

views/profile.ejs

---

# 9. res.end()

Ends response manually

```js
app.get('/', (req, res) => {  
    res.write("Hello");  
    res.end();  
});
```

Usually not needed.

res.send() automatically ends.

---

# 10. res.set()

Sets response headers

```js
app.get('/', (req, res) => {  
    res.set('Content-Type', 'text/plain');  
    res.send("Hello");  
});
```

---

# 11. res.header()

Same as res.set()

```js
res.header("Content-Type", "application/json");
```

---

# 12. res.type()

Sets content type

```js
res.type("html");  
res.send("<h1>Hello</h1>");
```

---

# 13. res.location()

Sets location header

```js
res.location('/dashboard');  
res.send("Redirecting");
```

Usually used before redirect.

---

# 14. res.cookie()

Set cookie in browser

```js
res.cookie("user", "john");  
res.send("cookie set");
```

---

# 15. res.clearCookie()

Remove cookie

```js
res.clearCookie("user");  
res.send("cookie removed");
```

---

# Real API Example Using Multiple Response Methods

```js
app.get('/user/:id', (req, res) => {  
  
    const id = req.params.id;  
  
    if(id == 1){  
        res.status(200).json({  
            id: 1,  
            name: "John"  
        });  
    }  
  
    else{  
        res.status(404).json({  
            error: "User not found"  
        });  
    }  
  
});
```

---

# Important Rule

You can send **only one response**

Wrong:

```js
res.send("hello");  
res.send("again");
```

Correct:

```js
res.send("hello");
```

---

# Most Used in Real Projects

You will mostly use:

res.send()  
res.json()  
res.status()  
res.redirect()  
res.sendFile()  
res.download()

---

# Flow Example

```js
app.get('/login', (req, res) => {  
  
    const loggedIn = false;  
  
    if(!loggedIn){  
        return res.status(401).json({  
            error: "Not logged in"  
        });  
    }  
  
    res.send("Welcome");  
});
```

---

# Quick Cheat Sheet

res.send() → send anything  
res.json() → send JSON  
res.status() → set status code  
res.redirect() → redirect  
res.sendFile() → send file  
res.download() → download file  
res.render() → render template  
res.end() → end response  
res.set() → set headers  
res.cookie() → set cookie


---


# Few More Methods


# 1. res.jsonp()

`res.jsonp()` sends **JSON with JSONP support**

JSONP = **JSON with Padding**  
Used for **cross-domain requests** (old technique before CORS)

### Normal JSON

```js
res.json({name: "John"});
```

Response:

```json
{"name":"John"}
```

---

### JSONP response

```js
res.jsonp({name: "John"});
```

If client requests:

```
/api?callback=myFunc
```

Response becomes:

```
myFunc({"name":"John"})
```

Express automatically wraps JSON in callback.

---

### Example

app.get('/data', (req, res) => {  
    res.jsonp({  
        user: "john",  
        age: 25  
    });  
});

Used rarely today.  
Modern apps use **CORS instead**.

---

# 2. res.sendStatus()

Shortcut for:

res.status(code).send(message)

Example:

res.sendStatus(404);

Same as:

res.status(404).send("Not Found");

---

### Common sendStatus

res.sendStatus(200)  
res.sendStatus(201)  
res.sendStatus(400)  
res.sendStatus(401)  
res.sendStatus(403)  
res.sendStatus(404)  
res.sendStatus(500)

---

### Example

app.get('/user', (req, res) => {  
  
    const user = null;  
  
    if(!user){  
        return res.sendStatus(404);  
    }  
  
    res.send(user);  
});

---

# 3. res.headersSent

This is **NOT a function**  
It is a **boolean property**

It tells:

> Has response already been sent?

returns:

true  
false

---

### Example

app.get('/', (req, res) => {  
  
    res.send("Hello");  
  
    console.log(res.headersSent);  
  
});

Output:

true

Because response already sent.

---

### Why useful?

To prevent **sending multiple responses**

app.get('/', (req, res) => {  
  
    if(!res.headersSent){  
        res.send("Hello");  
    }  
  
});

---

# 4. res.append()

Adds header value

res.append("Warning", "199 misc warning");

Example:

app.get('/', (req, res) => {  
  
    res.append("Custom-Header", "hello");  
    res.send("done");  
  
});

---

# 5. res.set()

Sets response header

res.set("Content-Type", "text/plain");

Multiple headers:

res.set({  
    "Content-Type": "text/plain",  
    "X-Custom": "hello"  
});

---

# 6. res.get()

Get header value

res.get("Content-Type");

Example:

res.set("X-Test", "value");  
console.log(res.get("X-Test"));

---

# 7. res.type()

Sets content-type

res.type("json");  
res.send({name:"john"});

Same as:

Content-Type: application/json

---

# 8. res.format()

Used for **content negotiation**

Server sends response based on request type

app.get('/', (req, res) => {  
  
    res.format({  
  
        'text/plain': function(){  
            res.send('text response');  
        },  
  
        'text/html': function(){  
            res.send('<h1>html response</h1>');  
        },  
  
        'application/json': function(){  
            res.json({message: "json response"});  
        }  
  
    });  
  
});

Client decides format.

---

# 9. res.links()

Sets link header

res.links({  
  next: 'http://api.example.com?page=2',  
  last: 'http://api.example.com?page=5'  
});

Used in pagination APIs.

---

# 10. res.location()

Sets location header

res.location('/dashboard');  
res.send("redirecting");

Often used before redirect.

---

# 11. res.vary()

Adds Vary header

res.vary('User-Agent');

Used for caching control.

---

# 12. res.statusMessage

Set custom status message

res.statusMessage = "Everything OK";  
res.status(200).send("done");

---

# 13. res.write()

Write response manually

res.write("Hello ");  
res.write("World");  
res.end();

---

# 14. res.flushHeaders()

Send headers immediately

res.set("X-Test", "value");  
res.flushHeaders();

Used in streaming.

---

# 15. res.attachment()

Sets file as attachment

res.attachment("file.pdf");  
res.sendFile("file.pdf");

Forces download.

---

# 16. res.download()

Download file

res.download("report.pdf");

---

# 17. res.sendFile()

Send file

res.sendFile(__dirname + "/index.html");

---

# 18. res.cookie()

Set cookie

res.cookie("user", "john");

---

# 19. res.clearCookie()

Delete cookie

res.clearCookie("user");

---

# Complete List (All Response Methods)

res.send()  
res.json()  
res.jsonp()  
res.status()  
res.sendStatus()  
res.redirect()  
res.render()  
res.sendFile()  
res.download()  
res.end()  
res.set()  
res.get()  
res.append()  
res.type()  
res.format()  
res.links()  
res.location()  
res.vary()  
res.cookie()  
res.clearCookie()  
res.attachment()  
res.write()  
res.flushHeaders()

Property:

res.headersSent

---

# Most Important for Exams

These are usually asked:

res.send()  
res.json()  
res.jsonp()  
res.status()  
res.sendStatus()  
res.redirect()  
res.render()  
res.sendFile()  
res.headersSent

---

# Interview Style Example

app.get('/test', (req, res) => {  
  
    if(res.headersSent){  
        return;  
    }  
  
    res.status(200).json({  
        message: "Working"  
    });  
  
});