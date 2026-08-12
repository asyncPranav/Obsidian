
---

Yes. Let's begin **Phase 2 from the mental model**, not code.

You already understand CRUD. Now we need to understand the problem authentication solves.

## 1. The problem with your current API

Right now, suppose you have:

```text
User A
_id = 111
```

and User A creates:

```text
Task
_id = 999
userId = 111
```

Your client sends:

```json
{
  "title": "Learn sessions",
  "description": "Understand session authentication",
  "userId": "111"
}
```

Your server basically says:

> "You gave me `userId = 111`, and that user exists, so I'll create the task."

### Problem 🚨

What stops another person from sending:

```json
{
  "title": "Steal someone's task",
  "description": "😈",
  "userId": "111"
}
```

Nothing.

The server has **no idea who sent the request**.

That's the problem we're solving.

---

# 2. Authentication answers one question

> **Who is making this request?**

For example:

```text
Pranav
   ↓
login
   ↓
email + password
   ↓
Server verifies credentials
   ↓
"Okay, this is User 111"
```

But there's a problem.

The HTTP protocol itself doesn't naturally remember:

> "This person already logged in 10 minutes ago."

Each request is essentially independent.

So we need a mechanism that lets the server **remember the authenticated client**.

That's where **sessions** come in.

---

# 3. What exactly is a session?

Think of a session as a server-side record saying:

```text
Session ID: abc123
        ↓
User ID: 111
```

Conceptually:

```text
SESSION STORE

┌─────────────────────┐
│ Session ID: abc123  │
│ User ID: 111        │
│ Created: ...        │
│ Expires: ...        │
└─────────────────────┘
```

The client doesn't need to send:

```text
"I am User 111"
```

Instead, it sends:

```text
"I have session abc123."
```

The server looks up:

```text
abc123
   ↓
User 111
```

Now the server knows who the client is.

---

# 4. But how does the client send the session ID?

This is where **cookies** enter.

After successful login:

```text
Client                         Server
  │                              │
  │ email + password             │
  │ ────────────────────────────►│
  │                              │
  │                         verify credentials
  │                              │
  │                         create session
  │                              │
  │◄─────────────────────────────│
  │       Set-Cookie             │
  │       session ID             │
  │                              │
```

The browser stores the cookie.

Then later:

```text
Client
  │
  │ GET /api/tasks
  │ Cookie: session=abc123
  ▼
Server
```

The browser automatically sends the cookie for matching requests.

---

# 5. The complete flow

This is the flow I want you to understand before we touch code.

### Registration

```text
POST /api/auth/register

        ↓

name
email
password

        ↓

Validate

        ↓

Hash password

        ↓

Create User

        ↓

MongoDB
```

At this point:

**The user exists.**

But they aren't necessarily authenticated yet.

---

# 6. Login

User sends:

```json
{
  "email": "user@example.com",
  "password": "secret123"
}
```

Server:

```text
Receive credentials
       ↓
Find user by email
       ↓
Get stored password hash
       ↓
Compare submitted password
       ↓
Correct?
    /      \
  NO        YES
  ↓          ↓
401       Create session
             ↓
        Generate session ID
             ↓
        Store session
             ↓
        Send cookie
```

---

# 7. What happens after login?

Suppose the server creates:

```text
session ID = abc123
```

and stores:

```text
abc123 → User 111
```

The response tells the browser:

```text
Set-Cookie: session=abc123
```

The browser remembers it.

---

# 8. Now comes the interesting part

User makes:

```http
GET /api/tasks
```

The browser automatically attaches:

```text
Cookie: session=abc123
```

Server receives it.

Our authentication middleware eventually does something conceptually like:

```text
Request
   ↓
Read session cookie
   ↓
Get session ID
   ↓
Find session
   ↓
Get user ID
   ↓
Find User
   ↓
req.user = User
   ↓
next()
```

So after middleware:

```js
req.user
```

might conceptually contain:

```js
{
  _id: "111",
  name: "Pranav",
  email: "user@example.com",
  role: "user"
}
```

Now your controller doesn't need:

```js
const { userId } = req.body;
```

Instead, the server already knows:

```js
req.user._id
```

---

# 9. This changes your Task API

### Phase 1

You currently have:

```text
POST /api/tasks

{
  title,
  description,
  userId
}
```

The client tells the server:

```text
"I want this task to belong to User 111."
```

---

### After authentication

It becomes conceptually:

```text
POST /api/tasks

{
  title,
  description
}
```

And:

```text
Cookie
   ↓
Session
   ↓
User 111
   ↓
req.user
   ↓
Task.userId = req.user._id
```

So the client doesn't control ownership anymore.

**The server determines ownership from authentication.**

That's a major security improvement.

---

# 10. Session vs Cookie — don't mix these up

This is one of the most important concepts.

### Session

Generally lives **server-side**.

Conceptually:

```text
Session Store

abc123 → User 111
```

### Cookie

Lives **client-side** and carries an identifier.

```text
Browser Cookie

session=abc123
```

So:

```text
Cookie
  ↓
contains session identifier
  ↓
Server uses identifier
  ↓
finds session
  ↓
finds user
```

Don't think:

> "The cookie is the session."

They're related, but they're not the same thing.

---

# 11. Where does authentication middleware fit?

Your existing architecture is:

```text
Request
   ↓
Router
   ↓
Validation
   ↓
Controller
   ↓
MongoDB
```

We're going to insert authentication into that pipeline:

```text
Request
   ↓
Router
   ↓
Authentication Middleware
   ↓
Validation
   ↓
Controller
   ↓
MongoDB
```

For example:

```text
GET /api/tasks
       ↓
authenticate
       ↓
Who is this?
       ↓
req.user
       ↓
getAllTasks
```

---

# 12. Authentication vs Authorization

Don't combine these yet.

### Authentication

```text
WHO ARE YOU?
```

Example:

```text
Session → User 111
```

### Authorization

```text
WHAT ARE YOU ALLOWED TO DO?
```

Example:

```text
User 111 owns Task 999
       ↓
Can delete Task 999? YES

User 222 tries to delete Task 999
       ↓
Can delete? NO
```

Phase 2 is primarily:

```text
Authentication
```

Authorization comes afterward.

---

# 13. Our Phase 2 flow

We're going to build this gradually:

```text
                 PHASE 2
                    │
                    ▼
             1. Password Hashing
                    │
                    ▼
             2. Registration
                    │
                    ▼
             3. Login
                    │
                    ▼
             4. Session Creation
                    │
                    ▼
             5. Session Cookie
                    │
                    ▼
             6. Authentication Middleware
                    │
                    ▼
                req.user
                    │
                    ▼
             7. Protected Routes
                    │
                    ▼
                8. Logout
```

And throughout the process we'll keep asking:

> **What is happening at the HTTP level?**

> **What is happening in Express?**

> **What is happening in MongoDB/session storage?**

> **What does the browser/client actually receive and send?**

That's the level of understanding I want you to have.

---

## First checkpoint 🧠

Before we write any code, make sure this chain makes sense:

```text
Login
  ↓
Verify password
  ↓
Create session
  ↓
Session gets an ID
  ↓
ID is sent to client through cookie
  ↓
Client sends cookie on future requests
  ↓
Server reads session ID
  ↓
Server finds associated user
  ↓
req.user
  ↓
Protected controller
```

**If this flow is clear, we're ready for Step 1: password hashing + why plain-text passwords must never be stored.**