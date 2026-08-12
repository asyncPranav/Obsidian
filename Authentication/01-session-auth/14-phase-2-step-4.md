
---

Good. ✅ So far Phase 2 is:

```text
✅ Registration
   └── password hashing with bcrypt

✅ Login
   └── password verification with bcrypt.compare()
```

Now we reach the **actual session-authentication part**.

# Step 4 — Understanding Sessions

Before installing anything, I want you to understand **what we're about to build**.

Right now, after this:

```text
POST /api/auth/login
        ↓
email + password
        ↓
bcrypt.compare()
        ↓
✅ correct
        ↓
200 Login successful
```

the request ends.

The server has **no persistent memory** that says:

> "This client has authenticated as User X."

That's what sessions solve.

---

## 1. Imagine our server keeps a notebook

After successful login, we want something conceptually like:

```text
SESSION STORE

┌──────────────────────────────────┐
│ Session ID       User            │
│                                  │
│ abc123xyz       User A           │
│ def456xyz       User B           │
└──────────────────────────────────┘
```

So:

```text
abc123xyz → User A
```

The session ID is basically an identifier for that login session.

---

# 2. But how does the server know which session belongs to which request?

We need the client to send the session ID back.

That's where **cookies** come in.

After login:

```text
Client                         Server
  │                              │
  │ email + password             │
  │ ────────────────────────────►│
  │                              │
  │                         verify password
  │                              │
  │                         create session
  │                              │
  │◄─────────────────────────────│
  │       Set-Cookie             │
  │       sessionId=abc123       │
```

The client stores:

```text
sessionId=abc123
```

Then the next request:

```text
Client
   │
   │ GET /api/tasks
   │ Cookie: sessionId=abc123
   ▼
Server
```

The server sees:

```text
abc123
```

and looks it up:

```text
abc123
   ↓
User A
```

Now:

```js
req.user
```

can represent User A.

---

# 3. Session ≠ Cookie

This distinction is **very important**.

Think:

```text
SESSION
────────────────
Server-side memory/record

abc123 → User A
```

while:

```text
COOKIE
────────────────
Client-side information

sessionId=abc123
```

The cookie essentially helps the server identify the session.

So:

```text
Cookie
   ↓
Session ID
   ↓
Session
   ↓
User
```

---

# 4. Where should sessions be stored?

There are multiple options.

For example:

```text
Memory
Redis
MongoDB
Database-backed session store
```

For our project, we're using MongoDB already, so we'll use a **MongoDB-backed session store**.

That will make the architecture easier for you to inspect and understand.

We'll use:

```text
express-session
```

and a MongoDB session store.

---

# 5. What `express-session` does

Conceptually, `express-session` gives us:

```text
Request
   ↓
Session middleware
   ↓
req.session
```

So instead of manually managing session IDs ourselves, Express can manage the session lifecycle.

For example:

```js
req.session.userId = user._id;
```

Now the session contains the authenticated user's ID.

Conceptually:

```text
req.session
      │
      └── userId → 123456
```

---

# 6. The complete login flow we're building

Eventually:

```text
POST /api/auth/login
        │
        ▼
Find user
        │
        ▼
bcrypt.compare()
        │
        ▼
Password correct?
        │
       YES
        │
        ▼
req.session.userId = user._id
        │
        ▼
express-session creates/maintains session
        │
        ▼
Session cookie sent to client
        │
        ▼
Postman/browser stores cookie
```

Then:

```text
GET /api/tasks
        │
        ▼
Cookie automatically sent
        │
        ▼
express-session reads cookie
        │
        ▼
req.session
        │
        ▼
userId
        │
        ▼
Find user
        │
        ▼
req.user
```

---

# 7. Why `req.user`?

This is something we'll establish as our application's convention.

After authentication middleware runs:

```js
req.user
```

will represent:

> **The user who has been authenticated for this request.**

Then controllers can use:

```js
req.user._id
```

instead of trusting:

```js
req.body.userId
```

That's going to be a **major change to your Task API**.

Currently:

```json
{
  "title": "Learn sessions",
  "description": "Understand sessions",
  "userId": "some-id"
}
```

Eventually:

```json
{
  "title": "Learn sessions",
  "description": "Understand sessions"
}
```

The server determines:

```text
session
   ↓
authenticated user
   ↓
req.user._id
   ↓
Task.userId
```

The client doesn't get to choose whose tasks they're creating.

---

# 8. Install the session packages

Now we're ready to actually implement it.

From your project root:

```bash
npm install express-session connect-mongo
```

These have separate responsibilities:

```text
express-session
      ↓
Session management for Express


connect-mongo
      ↓
Stores sessions in MongoDB
```

So our architecture becomes:

```text
                Express
                   │
            express-session
                   │
                   ▼
              Session ID
                   │
                   ▼
              connect-mongo
                   │
                   ▼
               MongoDB
```

---

## 9. One thing I want you to notice

We already have:

```text
MongoDB
   ↓
User documents
   ↓
Task documents
```

After this:

```text
MongoDB
   ├── users
   ├── tasks
   └── sessions
```

So MongoDB will contain a separate collection for session data.

Conceptually:

```text
users
────────────────
User A
User B


tasks
────────────────
Task 1
Task 2


sessions
────────────────
Session ABC → User A
Session XYZ → User B
```

That's a useful mental model.

---

### Your task now

Run:

```bash
npm install express-session connect-mongo
```

**Don't change `app.js` yet.**

Once both packages install successfully, tell me and we'll configure the session middleware **line by line**, including:

- `secret`
    
- `resave`
    
- `saveUninitialized`
    
- `cookie`
    
- `connect-mongo`
    
- what exactly gets stored in MongoDB
    
- and why each setting exists.
    

That's where the session concept will become concrete.