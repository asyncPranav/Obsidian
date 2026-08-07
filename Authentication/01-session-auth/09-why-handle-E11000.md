
---

Because the earlier `findOne()` check is **not enough by itself**.

You currently have two protections:

```text
1. Application-level check
2. Database-level unique constraint
```

### 1. The `findOne()` check

We do:

```js
const userWithSameEmail = await User.findOne({
  email,
  _id: { $ne: req.params.id },
});

if (userWithSameEmail) {
  return next(new ApiError(409, "Email is already registered"));
}
```

This gives us a **friendly error before attempting the update**.

But there is a small problem.

### Race condition

Imagine two requests arrive almost simultaneously:

```text
Request A                         Request B

findOne(email)
→ nothing                        findOne(email)
                                 → nothing

      ↓                                  ↓

findByIdAndUpdate()              findByIdAndUpdate()
      ↓                                  ↓
       └──────────────┬──────────────────┘
                      ↓
             MongoDB unique index
                      ↓
                 one succeeds
                 one fails
```

Both requests can pass the `findOne()` check because **neither has committed the email yet**.

But your schema has:

```js
email: {
  type: String,
  required: true,
  unique: true,
}
```

`unique: true` creates a **unique index** in MongoDB.

Therefore MongoDB ultimately says:

> "No. This email already exists."

Mongoose reports that as:

```text
error.code === 11000
```

So this:

```js
catch (error) {
  if (error.code === 11000) {
    return next(
      new ApiError(409, "Email is already registered")
    );
  }

  next(error);
}
```

converts the database error into your clean API error.

---

### Think of it as two layers

```text
                UPDATE USER
                     │
                     ▼
          Application-level check
                     │
             findOne({ email })
                     │
             ┌───────┴────────┐
             │                │
        email exists      doesn't exist
             │                │
            409                ▼
                         perform update
                              │
                              ▼
                       MongoDB unique index
                              │
                       ┌──────┴──────┐
                       │             │
                    success       duplicate
                       │             │
                      200        error 11000
                                     │
                                     ▼
                                    409
```

### Why not just rely on `11000`?

You **could** technically skip the `findOne()` and simply attempt the update.

The database would protect uniqueness.

But then your normal path is:

```text
try update
   ↓
MongoDB rejects
   ↓
catch 11000
   ↓
409
```

The explicit `findOne()` gives us a clearer application-level check and lets us handle the common case intentionally.

However, **the database unique constraint is the real guarantee**.

That's the key concept:

> **Application checks improve behavior; database constraints guarantee data integrity.**

So the `catch (error.code === 11000)` isn't redundant. It's the **fallback protection against the database rejecting a duplicate despite our earlier check**.