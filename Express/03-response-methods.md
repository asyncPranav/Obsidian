
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