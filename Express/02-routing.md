
---


# **Express.js Routing – Super Detailed Beginner-Friendly Notes**

Routing in Express.js is **how the server knows what to do when someone visits a URL**.

Think of it like a **map**:

|URL|What happens|
|---|---|
|`/`|Show Home Page|
|`/about`|Show About Page|
|`/user/123`|Show info for user with ID 123|

---

## **1. What is a Route?**

A **route** is made of 3 things:

1. **HTTP method** – like GET, POST, PUT, DELETE.
2. **URL path** – the address you type in the browser.
3. **Callback function** – the code that runs when someone visits that URL.

### **Example:**

```js
const express = require('express');  
const app = express();  
const PORT = 3000;  
  
app.get('/', (req, res) => {  
    res.send('Hello! This is the Home Page.');  
});  
  
app.listen(PORT, () => {  
    console.log(`Server is running at http://localhost:${PORT}`);  
});
```

✅ **Explanation:**

- `app.get()` → listens for **GET requests** (usually when you visit a URL in a browser).
- `/` → means **home page**.
- `req` → contains request info (what the user sent to the server).
- `res` → contains response info (what the server sends back).

---

## **2. Route Parameters (Dynamic URLs)**

Sometimes URLs have **variable parts**, like a user ID.  
We use **:parameterName** to capture these.

### **Example:**

```js
app.get('/user/:id', (req, res) => {  
    const userId = req.params.id;  
    res.send(`You requested User ID: ${userId}`);  
});
```

- URL: `http://localhost:3000/user/101`
- Output: `You requested User ID: 101`
- **`req.params.id`** → gives the value in `:id`.

### **Multiple Parameters:**

```js
app.get('/user/:userId/book/:bookId', (req, res) => {  
    const { userId, bookId } = req.params;  
    res.send(`User ${userId} wants Book ${bookId}`);  
});
```

- URL: `/user/5/book/10` → Output: `User 5 wants Book 10`

---

## **3. Query Parameters**

Query parameters come **after `?` in a URL**.

- Used for **search, filters, sorting**.

### **Example:**

```js
app.get('/search', (req, res) => {  
    const searchTerm = req.query.q;   
    res.send(`You searched for: ${searchTerm}`);  
});
```

- URL: `http://localhost:3000/search?q=express`
- Output: `You searched for: express`
- Multiple queries: `/search?q=express&sort=asc` → `req.query = { q: 'express', sort: 'asc' }`

---

## **4. Handling Different HTTP Methods**

- GET → Read data
- POST → Send data
- PUT → Update data
- DELETE → Remove data

### **Example:**

```js
app.get('/book', (req, res) => res.send('Getting all books'));  
app.post('/book', (req, res) => res.send('Adding a new book'));  
app.put('/book', (req, res) => res.send('Updating book'));  
app.delete('/book', (req, res) => res.send('Deleting book'));
```

---

## **5. Chaining Routes**

If you want to use **multiple methods on the same URL**, use **route chaining**.

```js
app.route('/book')  
   .get((req, res) => res.send('Get all books'))  
   .post((req, res) => res.send('Add a new book'))  
   .put((req, res) => res.send('Update book info'));
```

✅ **Benefit:**  
Keeps your code **clean** instead of writing `app.get`, `app.post` separately.

---

## **6. Optional Route Parameters**

- Some route parts can be **optional**.
- Use `?` at the end.

```js
app.get('/user/:name?', (req, res) => {  
    const name = req.params.name || 'Guest';  
    res.send(`Hello, ${name}`);  
});
```

- `/user/John` → `Hello, John`
- `/user` → `Hello, Guest`

---

## **7. Using Router Module (Modular Routes)**

For bigger apps, we **split routes into separate files** using `express.Router()`.

### **Step 1 – Create router file**

`routes/user.js`

```js
const express = require('express');  
const router = express.Router();  
  
router.get('/profile', (req, res) => res.send('User Profile'));  
router.get('/settings', (req, res) => res.send('User Settings'));  
  
module.exports = router;
```

### **Step 2 – Use router in main file**

`app.js`

```js
const express = require('express');  
const app = express();  
const userRoutes = require('./routes/user');  
  
app.use('/user', userRoutes);  
  
app.listen(3000, () => console.log('Server running'));
```

- `/user/profile` → `User Profile`
- `/user/settings` → `User Settings`

✅ **Benefit:**

- Keeps code organized and easy to maintain.

---

## **8. Route Middleware**

Middleware can **run before a route** to do something like **check login, log requests, validate data**.

```js
function checkLogin(req, res, next) {  
    console.log('Checking if user is logged in...');  
    next(); // move to next middleware or route  
}  
  
app.get('/dashboard', checkLogin, (req, res) => {  
    res.send('Welcome to Dashboard');  
});
```

---

## **9. Nested Routes**

You can **nest routers inside routers**.

```js
const adminRouter = express.Router();  
  
adminRouter.get('/users', (req, res) => res.send('Admin Users'));  
adminRouter.get('/settings', (req, res) => res.send('Admin Settings'));  
  
app.use('/admin', adminRouter);
```

- `/admin/users` → Admin Users
- `/admin/settings` → Admin Settings

---

## **10. Summary – Easy Language**

1. **Route** = URL + HTTP method + code to run.
2. **Route parameters** → for dynamic parts like `/user/:id`.
3. **Query parameters** → for search or filters `/search?q=term`.
4. **HTTP methods** → GET, POST, PUT, DELETE.
5. **Chaining routes** → handle multiple methods on the same path.
6. **Optional params** → `/user/:name?` → sometimes it’s there, sometimes not.
7. **Router module** → split routes into files → clean code.
8. **Middleware** → run code before route → auth, logging, validation.
9. **Nested routes** → routers inside routers → big apps.