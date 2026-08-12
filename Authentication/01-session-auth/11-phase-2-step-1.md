
----

Good. Let's start **Phase 2 — Step 1: Password Hashing**.

We will **not** jump into sessions yet. First, we need to fix the most fundamental authentication problem in your Phase 1 code: **password storage**.

### 1. What you currently have

Your `User` model currently accepts:

```js
password: {
  type: String,
  required: true,
}
```

And your controller does:

```js
const user = await User.create({
  name,
  email,
  password,
});
```

So if the user registers with:

```json
{
  "name": "Pranav",
  "email": "pranav@example.com",
  "password": "Pranav@123"
}
```

the dangerous flow is:

```text
Pranav@123
    ↓
User.create()
    ↓
MongoDB
    ↓
password: "Pranav@123"   ❌
```

We never want the actual password stored in the database.

---

# 2. What we want instead

We want:

```text
User enters password
        ↓
"Pranav@123"
        ↓
Hashing algorithm
        ↓
"$2b$10$......"
        ↓
MongoDB
```

The database stores a **password hash**, not the original password.

For example, conceptually:

```text
password entered:
Pranav@123

stored:
$2b$10$someVeryLongHash...
```

The important property is:

> You should not be able to take the stored hash and simply turn it back into the original password.

---

# 3. Hashing ≠ encryption

This distinction is important.

### Encryption

```text
original
   ↓
encrypt
   ↓
ciphertext
   ↓
decrypt
   ↓
original
```

Encryption is designed to be reversible with the appropriate key.

### Password hashing

```text
password
   ↓
hash
   ↓
hash
```

There isn't supposed to be a normal "decrypt this hash" operation.

During login, we don't decrypt the stored hash.

Instead:

```text
Login password
      ↓
compare with stored hash
      ↓
match?
```

---

# 4. Which hashing algorithm?

For this project, we'll use **bcrypt**.

Conceptually:

```text
bcrypt
  ├── takes password
  ├── generates salt
  ├── performs expensive hashing
  └── produces password hash
```

The "expensive" part is intentional.

Password attacks often involve trying huge numbers of guesses. A password-hashing algorithm is designed to make each guess computationally costly.

---

# 5. Where should hashing happen?

This is an important design decision.

We could put hashing directly in:

```text
auth.controller.js
```

For example:

```js
const hashedPassword = await bcrypt.hash(password, 10);
```

and then create the user.

That's perfectly understandable for learning.

But there's another approach:

### Mongoose middleware

You can tell the User model:

> Whenever a password is about to be saved, hash it first.

Conceptually:

```text
User.create()
     ↓
Mongoose middleware
     ↓
hash password
     ↓
save document
```

For **our learning project**, I want us to initially understand the explicit controller flow rather than hiding the important authentication behavior inside a Mongoose hook.

That makes the authentication process easier to see.

---

# 6. Install bcrypt

From your project root:

```bash
npm install bcrypt
```

Then we'll import it into the authentication controller.

But **don't modify your existing `createUser` controller yet**.

There's a reason.

Your existing endpoint:

```text
POST /api/users
```

is currently a generic CRUD endpoint.

In Phase 2, we're going to introduce:

```text
POST /api/auth/register
```

Authentication should have its own responsibility.

So we'll eventually have:

```text
/api/users
    ↓
user resource management

/api/auth
    ↓
authentication
```

That distinction matters.

---

# 7. The new registration flow

We're going to build:

```text
POST /api/auth/register
          │
          ▼
      validation
          │
          ▼
   check existing email
          │
          ▼
    hash password
          │
          ▼
     create User
          │
          ▼
    safe response
```

And the password transformation is:

```text
req.body.password
       │
       ▼
bcrypt.hash()
       │
       ▼
hashedPassword
       │
       ▼
User.create()
```

---

# 8. One important thing about your existing database

You already created users during Phase 1.

Those users may currently have **plain-text passwords**.

After we introduce hashing, those existing records don't magically become hashes.

So for our learning project, I'd recommend:

**Delete the old test users from your database** before testing the new registration flow.

Then we'll create fresh users through the new authentication system.

---

## Your first task

For now, only do these two things:

```bash
npm install bcrypt
```

and confirm it installs successfully.

**Don't write the auth controller yet.**

Once `bcrypt` is installed, we'll build the first actual Phase 2 feature:

> `POST /api/auth/register`

and I'll make you understand exactly what happens to the password from the moment it enters `req.body` until the hashed value reaches MongoDB.

