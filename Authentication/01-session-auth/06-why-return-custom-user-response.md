
---
```js
const createUser = async (req, res, next) => {
  try {
    const { name, email, password } = req.body;

    const existingUser = await User.findOne({ email });

    if (existingUser) {
      return next(new ApiError(409, "Email is already registered"));
    }

    const user = await User.create({ name, email, password });

    res.status(201).json({
      status: "success",
      data: {
        user: {
          id: user._id,
          name: user.name,
          email: user.email,
          role: user.role,
          createdAt: user.createdAt,
          updatedAt: user.updatedAt,
        },
      },
    });
  } catch (err) {
    if (err.code === 11000) {
      return next(new ApiError(409, "Email is already registered"));
    }

    next(err);
  }
};
```

Because you usually **don't want to send the entire MongoDB/Mongoose `user` document back to the client**.

This part:

```js
data: {
  user: {
    id: user._id,
    name: user.name,
    email: user.email,
    role: user.role,
    createdAt: user.createdAt,
    updatedAt: user.updatedAt,
  },
}
```

is creating a **controlled API response**.

### 1. `user` contains more than you may want to expose

Suppose your Mongoose user looks like:

```js
{
  _id: "...",
  name: "John",
  email: "john@gmail.com",
  password: "hashed-password",
  role: "user",
  createdAt: "...",
  updatedAt: "...",
  __v: 0
}
```

If you did:

```js
res.status(201).json({
  status: "success",
  data: {
    user
  }
});
```

you could accidentally send:

```json
{
  "status": "success",
  "data": {
    "user": {
      "_id": "...",
      "name": "John",
      "email": "john@gmail.com",
      "password": "hashed-password",
      "role": "user",
      "createdAt": "...",
      "updatedAt": "...",
      "__v": 0
    }
  }
}
```

**That's dangerous**, especially because of `password`.

Even though the password should be hashed, **you still don't want to send password hashes to the client.**

---

### 2. You're choosing what your API exposes

Instead, you explicitly select:

```js
user: {
  id: user._id,
  name: user.name,
  email: user.email,
  role: user.role,
  createdAt: user.createdAt,
  updatedAt: user.updatedAt,
}
```

Now your API response is:

```json
{
  "status": "success",
  "data": {
    "user": {
      "id": "...",
      "name": "John",
      "email": "john@gmail.com",
      "role": "user",
      "createdAt": "...",
      "updatedAt": "..."
    }
  }
}
```

Notice:

```text
password ❌
__v      ❌
```

You're saying:

> **"These are the fields I want the API consumer to know about."**

---

## 3. This is called response shaping

You're **shaping** the database object into an API response.

Think:

```text
MongoDB document
       ↓
Mongoose user
       ↓
Select only safe/useful fields
       ↓
API response
```

This is a good practice because your **database structure and API response structure don't have to be identical**.

---

## 4. Why not just remove password?

You could do:

```js
const user = await User.create(...);

user.password = undefined;
```

But explicitly constructing the response is often clearer:

```js
user: {
  id: user._id,
  name: user.name,
  email: user.email,
  role: user.role,
}
```

It gives you an explicit **allowlist**:

> "Only these fields are allowed to leave my server."

That's safer than saying:

> "Send everything except these fields."

---

## 5. This becomes even more important later

Imagine you add these fields to your User model:

```js
{
  name,
  email,
  password,
  role,
  resetPasswordToken,
  resetPasswordExpires,
  loginAttempts,
  internalNotes
}
```

If you return:

```js
res.json({ user });
```

you might accidentally expose something new later.

But if you use:

```js
res.json({
  user: {
    id: user._id,
    name: user.name,
    email: user.email,
    role: user.role,
  }
});
```

new database fields aren't automatically exposed.

---

## One more thing: your `password`

You should ideally make sure your schema also prevents passwords from being returned in queries, for example:

```js
password: {
  type: String,
  required: true,
  select: false,
}
```

Then you'd explicitly request it only when you need it for login.

For example, conceptually:

```js
User.findOne({ email }).select("+password");
```

This gives you another layer of protection.

---

### The simple mental model

Don't think:

```text
MongoDB user
     ↓
send directly to client
```

Think:

```text
MongoDB
   ↓
User document
   ↓
"What does the client actually need?"
   ↓
Safe response object
   ↓
Client
```

So the reason you're creating a custom user object is **not because the API requires it**. It's because it gives you **control over what information your API exposes**.