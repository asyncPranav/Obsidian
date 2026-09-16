
----
**1. `status` = what stage/state the connection request is in**  
**2. `index` = a database rule that makes certain duplicate data impossible**  
**3. `pre("save")` = a model-level safety check**

---

# ConnectionRequest Schema — Complete Notes

## 1. What is a ConnectionRequest?

A `ConnectionRequest` document represents a request from one user to another.

Example:

```text
A  ─────── request ───────>  B
```

In the database:

```js
{
  fromUserId: A,
  toUserId: B,
  status: "interested"
}
```

This means:

> User A is interested in connecting with User B.

The request can later change its status:

```text
interested
    │
    ├── accepted
    │
    └── rejected
```

There is also:

```text
ignored
```

So the possible statuses are:

```text
interested
accepted
rejected
ignored
```

---

# 2. Why do we need a `status` field?

Without `status`, the database would only know:

```text
A sent a request to B
```

But it would not know what happened afterward.

For example:

```text
A → B
```

Could mean:

- request is waiting
    
- request was accepted
    
- request was rejected
    
- request was ignored
    

Therefore we store the current state:

```js
status: "interested"
```

or

```js
status: "accepted"
```

etc.

## Think of status as the "state" of the request

A request behaves like a small state machine.

```text
                ┌─────────────┐
                │ interested  │
                └──────┬──────┘
                       │
              ┌────────┴────────┐
              ↓                 ↓
        ┌──────────┐      ┌──────────┐
        │ accepted │      │ rejected │
        └──────────┘      └──────────┘

Sender can also directly create:

        ┌──────────┐
        │  ignored │
        └──────────┘
```

---

# 3. Why use `enum` for status?

Your schema has:

```js
status: {
  type: String,
  enum: {
    values: ["interested", "ignored", "accepted", "rejected"],
    message: "{VALUE} is not a valid connection request status",
  },
  required: [true, "status is required"],
}
```

This says:

> `status` must be a string and it can only contain one of these four values.

Valid:

```js
"interested"
"ignored"
"accepted"
"rejected"
```

Invalid:

```js
"pending"
"blocked"
"hello"
"abc"
```

Mongoose will reject invalid values during validation/save.

## Why is this useful?

Without enum, someone could accidentally create:

```js
{
  status: "intersted"
}
```

Typo.

Or:

```js
{
  status: "completed"
}
```

which your application does not understand.

`enum` keeps your database values controlled.

---

# 4. What does each status mean?

## `interested`

The sender wants to connect with the receiver.

Example:

```text
A → B
status = interested
```

The request is waiting for B's decision.

This is an **active** request.

---

## `accepted`

B accepted A's request.

```text
A → B
status = accepted
```

This is also considered **active** because A and B are now connected.

---

## `rejected`

B rejected the request.

```text
A → B
status = rejected
```

This is no longer active.

The old document is kept as history.

---

## `ignored`

A chose to send an ignored-type request/state according to your application's design.

```text
A → B
status = ignored
```

This is also not active.

---

# 5. Very important: status is NOT the same as HTTP status

Do not confuse:

```js
status: "accepted"
```

with:

```text
HTTP 200
HTTP 400
HTTP 404
HTTP 409
```

They are two completely different things.

### HTTP status

Describes the result of an API request.

```text
200 → success
400 → bad request
404 → not found
409 → conflict
```

### Connection status

Describes the state of a connection request.

```text
interested
accepted
rejected
ignored
```

---

# 6. How the API uses status

Your API might have:

```text
POST /request/send/interested/:toUserId
```

Then:

```js
{
  fromUserId: A,
  toUserId: B,
  status: "interested"
}
```

Later B reviews it:

```text
POST /request/review/accepted/:requestId
```

Then the document changes:

```js
status: "accepted"
```

or:

```text
POST /request/review/rejected/:requestId
```

Then:

```js
status: "rejected"
```

So the controller uses `status` to represent the request's current state.

---

# 7. What is a Mongoose index?

An **index** is a database structure used mainly to:

1. Make queries faster.
    
2. Enforce uniqueness in certain cases.
    

You can think of it like an index in a book.

Without an index:

```text
Database
↓
Check document 1
Check document 2
Check document 3
Check document 4
...
```

With an index:

```text
Database
↓
Index
↓
Find matching records quickly
```

MongoDB uses indexes to avoid scanning every document for many queries.

---

# 8. Simple index example

Suppose you frequently query:

```js
ConnectionRequest.findOne({
  fromUserId: A,
  toUserId: B,
});
```

You can create:

```js
connectionRequestSchema.index({
  fromUserId: 1,
  toUserId: 1,
});
```

This is a **compound index**.

It indexes two fields together.

---

# 9. What does `1` mean?

In:

```js
{
  fromUserId: 1,
  toUserId: 1
}
```

`1` means ascending index order.

You can also see:

```js
-1
```

which means descending order.

For many equality queries, the important idea is simply:

```text
1 = ascending
-1 = descending
```

Do not overfocus on the sorting meaning right now.

The important part is:

```js
{ fromUserId: 1, toUserId: 1 }
```

means MongoDB creates an index involving both fields.

---

# 10. What is a compound index?

An index containing multiple fields.

Example:

```js
schema.index({
  firstName: 1,
  age: 1,
});
```

This is a compound index.

Your schema has:

```js
schema.index({
  fromUserId: 1,
  toUserId: 1,
});
```

Therefore:

```text
fromUserId + toUserId
```

form the indexed combination.

---

# 11. What does `unique: true` mean?

Normally this:

```js
schema.index(
  {
    fromUserId: 1,
    toUserId: 1,
  },
  {
    unique: true,
  }
);
```

means:

> MongoDB should not allow two documents with the same `(fromUserId, toUserId)` pair.

For example, if this already exists:

```js
{
  fromUserId: A,
  toUserId: B
}
```

another identical pair:

```js
{
  fromUserId: A,
  toUserId: B
}
```

would cause a duplicate-key error.

---

# 12. But your index has something more advanced: `partialFilterExpression`

Your actual index is:

```js
connectionRequestSchema.index(
  { fromUserId: 1, toUserId: 1 },
  {
    unique: true,
    partialFilterExpression: {
      status: {
        $in: ["interested", "accepted"],
      },
    },
  },
);
```

This is the important part.

`partialFilterExpression` means:

> Apply this index only to documents matching this condition.

Your condition is:

```js
status: {
  $in: ["interested", "accepted"],
}
```

`$in` means:

> value must be one of these values.

Therefore this index only applies to:

```text
interested
accepted
```

It does NOT apply to:

```text
rejected
ignored
```

---

# 13. Why do we need a partial unique index?

This is easiest to understand through examples.

Suppose A sends B a request:

```js
{
  fromUserId: A,
  toUserId: B,
  status: "interested"
}
```

This is active.

Now A sends another request to B:

```js
{
  fromUserId: A,
  toUserId: B,
  status: "interested"
}
```

We do NOT want two active requests.

The unique index blocks this.

---

# 14. What happens after rejection?

Suppose:

```js
A → B
status = rejected
```

Now later A wants to send B a new request.

You want:

```text
Old:
A → B
rejected

New:
A → B
interested
```

Both documents can exist.

Why?

Because `rejected` is not included in:

```js
$in: ["interested", "accepted"]
```

So the unique index ignores the rejected document.

That allows a new active request.

---

# 15. Why not simply use a normal unique index?

Suppose you used:

```js
schema.index(
  {
    fromUserId: 1,
    toUserId: 1,
  },
  {
    unique: true,
  }
);
```

Then this would happen:

```text
A → B rejected
```

and later:

```text
A → B interested
```

MongoDB would say:

```text
Duplicate key
```

because both have the same:

```text
fromUserId = A
toUserId   = B
```

Even though the old request is already finished.

That would prevent users from sending another request later.

Therefore the partial index is useful.

---

# 16. What does "active" mean here?

In your project:

```text
ACTIVE
------
interested
accepted
```

These states block another active relationship for the same ordered pair.

```text
NOT ACTIVE
----------
rejected
ignored
```

These don't block a new request.

Think:

```text
interested = request waiting
accepted   = connection exists

rejected   = old request finished
ignored    = old request finished
```

---

# 17. Important limitation of this index

The index:

```js
{
  fromUserId: 1,
  toUserId: 1
}
```

is directional.

These are different keys:

```text
A → B
B → A
```

So MongoDB considers:

```js
{ fromUserId: A, toUserId: B }
```

different from:

```js
{ fromUserId: B, toUserId: A }
```

Therefore the index alone cannot prevent:

```text
A → B interested
B → A interested
```

from both existing.

---

# 18. Why does the controller need another check?

Your business rule is:

> There should not be an active request/relationship between two users in either direction.

So before creating a request, the controller can check:

```text
A → B
OR
B → A
```

with active states:

```text
interested
accepted
```

Conceptually:

```js
{
  $or: [
    {
      fromUserId: A,
      toUserId: B,
    },
    {
      fromUserId: B,
      toUserId: A,
    },
  ],
  status: {
    $in: ["interested", "accepted"],
  },
}
```

This catches both directions.

---

# 19. Why isn't the controller check enough?

Because of a race condition.

Imagine two requests arrive almost simultaneously.

```text
Request 1                 Request 2
    ↓                         ↓
Check database             Check database
    ↓                         ↓
No active request          No active request
    ↓                         ↓
Save                       Save
```

Both may pass the application-level check before either one is saved.

Now you could accidentally have duplicates.

Therefore you want:

```text
Controller check
       +
Database unique index
```

The controller handles business logic.

The database index provides a final database-level guarantee for the exact ordered pair.

---

# 20. Application rule vs database rule

This distinction is extremely important.

## Controller

Responsible for:

```text
Business logic
```

Example:

```text
Can A send B a request?
```

It can check:

```text
A → B active?
B → A active?
Are they the same user?
```

## Database

Responsible for:

```text
Data integrity
```

Example:

```text
Can two active documents with the same
(fromUserId, toUserId) pair exist?
```

The unique index says:

```text
NO
```

This is defense in depth.

---

# 21. What is `pre("save")`?

You have:

```js
connectionRequestSchema.pre("save", function (next) {
  if (this.fromUserId.equals(this.toUserId)) {
    return next(
      new ApiError(400, "You cannot send a connection request to yourself"),
    );
  }
  next();
});
```

This is a Mongoose **pre-save middleware/hook**.

It runs before:

```js
document.save()
```

Example:

```js
const request = new ConnectionRequest({
  fromUserId: A,
  toUserId: B,
  status: "interested",
});

await request.save();
```

Before MongoDB saves it, the `pre("save")` function executes.

---

# 22. Why check `fromUserId === toUserId`?

Without the check, this could be created:

```text
A → A
```

which makes no sense for a connection request.

So:

```js
this.fromUserId.equals(this.toUserId)
```

checks:

```text
Does sender ID equal receiver ID?
```

If yes:

```js
new ApiError(400, ...)
```

is passed to:

```js
next(...)
```

That stops the save and sends the error through your error-handling system.

---

# 23. Why use `.equals()` instead of `===`?

MongoDB IDs are usually Mongoose `ObjectId` objects.

For ObjectIds, prefer:

```js
id1.equals(id2)
```

instead of:

```js
id1 === id2
```

because two ObjectId objects can represent the same database ID while being different JavaScript objects.

Example conceptually:

```text
ObjectId("123")
ObjectId("123")
```

may be different objects in memory.

`.equals()` compares their actual ObjectId values.

---

# 24. Complete lifecycle of your ConnectionRequest

Imagine:

```text
User A
  |
  | send request
  ↓
User B
```

Database:

```js
{
  fromUserId: A,
  toUserId: B,
  status: "interested"
}
```

Then B accepts:

```js
{
  fromUserId: A,
  toUserId: B,
  status: "accepted"
}
```

Now they are connected.

Alternatively, B rejects:

```js
{
  fromUserId: A,
  toUserId: B,
  status: "rejected"
}
```

Later A can send another request:

```js
{
  fromUserId: A,
  toUserId: B,
  status: "interested"
}
```

So history might look like:

```text
A → B   rejected    ← old request
A → B   interested  ← new request
```

This is allowed because only the active states participate in the partial unique index.

---

# 25. Why keep rejected/ignored documents instead of deleting them?

Because they provide history.

Instead of:

```text
delete old request
```

you keep:

```text
createdAt
updatedAt
fromUserId
toUserId
status
```

This can later be useful for:

```text
request history
analytics
debugging
moderation
audit information
```

---

# 26. What does `timestamps: true` do?

Your schema has:

```js
{
  timestamps: true,
}
```

Mongoose automatically adds:

```js
createdAt
updatedAt
```

Example:

```js
{
  fromUserId: A,
  toUserId: B,
  status: "interested",

  createdAt: "...",
  updatedAt: "..."
}
```

When status changes:

```text
interested → accepted
```

`updatedAt` is automatically updated.

---

# 27. Complete mental model

Remember your schema like this:

```text
ConnectionRequest
│
├── fromUserId
│      Who sent it?
│
├── toUserId
│      Who received it?
│
├── status
│      What is happening to the request?
│
│      interested
│      accepted
│      rejected
│      ignored
│
├── createdAt
│
└── updatedAt
```

And then:

```text
MODEL RULES
│
├── status must be one of 4 values
│
├── fromUserId != toUserId
│
└── active (interested/accepted)
    duplicate ordered pair not allowed
```

---

# 28. The index in one sentence

Your index means:

> For `interested` and `accepted` documents, the same `fromUserId → toUserId` pair cannot appear more than once.

```js
connectionRequestSchema.index(
  { fromUserId: 1, toUserId: 1 },
  {
    unique: true,
    partialFilterExpression: {
      status: { $in: ["interested", "accepted"] },
    },
  },
);
```

---

# 29. Very important distinction

Do not memorize this as:

> "Index prevents duplicate users."

That is incorrect.

The index specifically prevents:

```text
same fromUserId
+
same toUserId
+
active status
```

from appearing in multiple documents.

It does NOT prevent:

```text
A → B
```

and:

```text
B → A
```

because those are different ordered pairs.

---

# 30. Query examples you should know

## Find all requests sent by A

```js
ConnectionRequest.find({
  fromUserId: A,
});
```

## Find requests received by B

```js
ConnectionRequest.find({
  toUserId: B,
});
```

## Find pending requests received by B

```js
ConnectionRequest.find({
  toUserId: B,
  status: "interested",
});
```

## Find active relationship between A and B

```js
ConnectionRequest.findOne({
  $or: [
    {
      fromUserId: A,
      toUserId: B,
    },
    {
      fromUserId: B,
      toUserId: A,
    },
  ],
  status: {
    $in: ["interested", "accepted"],
  },
});
```

## Find accepted connections

```js
ConnectionRequest.find({
  $or: [
    { fromUserId: A },
    { toUserId: A },
  ],
  status: "accepted",
});
```

---

# 31. What happens when duplicate index is violated?

MongoDB generally returns:

```text
E11000 duplicate key error
```

Code:

```text
11000
```

Your controller can catch that and convert it into something like:

```text
409 Conflict
```

Meaning:

> An active connection request already exists.

This is why your controller can have application-level checking while still handling the database's `11000` safety net.

---

# 32. Schema validation vs business logic vs database integrity

This is one of the most important lessons from this schema.

### Schema validation

Checks:

```text
Is status valid?
Are IDs present?
Are required fields present?
```

Example:

```js
enum
required
```

### Model middleware

Checks model-level invariants:

```text
A cannot send request to A
```

Example:

```js
pre("save")
```

### Controller

Checks business rules:

```text
Can this user send this request now?
Can they send it in this direction?
Is there already an active relationship?
Does the request belong to the receiver?
Is this request currently "interested"?
```

### Database index

Protects data integrity:

```text
Do not allow duplicate active
(fromUserId, toUserId)
```

Think:

```text
Controller
   ↓
Business rules

Schema
   ↓
Data shape + basic validation

Middleware
   ↓
Model invariant checks

Database index
   ↓
Final integrity guarantee
```

---

# 33. What you actually need to remember for DevTinder

Do not try to memorize the entire large comment block.

Understand these 7 points:

```text
1. ConnectionRequest stores a request between two users.

2. fromUserId = sender
   toUserId   = receiver

3. status tells the current state.

4. enum restricts status to:
   interested
   ignored
   accepted
   rejected

5. Index improves lookup and can enforce uniqueness.

6. Partial unique index means:
   only interested + accepted participate
   in the unique constraint.

7. Controller checks both directions,
   while the DB index protects against duplicate
   active requests in the same direction.
```

---

# 34. The easiest way to visualize the whole thing

```text
                 CONNECTION REQUEST
                         │
          ┌──────────────┴──────────────┐
          │                             │
      fromUserId                    toUserId
        sender                       receiver
          │                             │
          └──────────────┬──────────────┘
                         ↓
                       status
                         │
       ┌─────────────────┼─────────────────┐
       ↓                 ↓                 ↓
  interested         accepted       rejected/ignored
     ACTIVE             ACTIVE          INACTIVE
       │                  │                 │
       └────────────┬─────┘                 │
                    ↓                       ↓
          Partial Unique Index       old history remains
                    │
                    ↓
        same A → B active pair
             cannot repeat
```

# 35. One final conceptual example

Suppose:

```text
A = Pranav
B = Rahul
```

### First request

```text
Pranav → Rahul
interested
```

Allowed.

### Pranav sends again

```text
Pranav → Rahul
interested
```

Rejected because an active `(A,B)` already exists.

### Rahul accepts

```text
Pranav → Rahul
accepted
```

Still active.

### Pranav tries again

```text
Pranav → Rahul
interested
```

Rejected because the existing accepted relationship is still active.

### Rahul had rejected instead

```text
Pranav → Rahul
rejected
```

Later:

```text
Pranav → Rahul
interested
```

Allowed.

### Reverse direction

```text
Rahul → Pranav
interested
```

The compound index by itself treats this as a different pair.

Therefore your controller must check:

```text
Pranav → Rahul
OR
Rahul → Pranav
```

for active statuses.

That is the core reason your schema contains both **status logic** and a **partial unique index**.