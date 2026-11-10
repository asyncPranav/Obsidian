
---
`DevTinder — Phase 04: User Feed, Received Requests & Connections`

---

# DevTinder — Phase 04

## 1. What are we building?

In Phase-03, we implemented the connection-request system.

Users can:

- Send connection requests.
    
- Accept connection requests.
    
- Reject connection requests.
    
- Ignore other users.
    

But we still need a way to discover users and see existing relationships.

That's the purpose of Phase-04.

We implement three endpoints:

|Endpoint|Purpose|
|---|---|
|`GET /api/user/feed`|Discover other users|
|`GET /api/user/requests/received`|See incoming pending requests|
|`GET /api/user/connections`|See accepted connections|

### Overall architecture

```
                    DEV TINDER
                        |
             +----------+----------+
             |          |          |
             v          v          v
           FEED       RECEIVED   CONNECTIONS
             |          |          |
             v          v          v
         Discover     Pending    Accepted
          Users       Requests   Relationships
             |          |          |
             +----------+----------+
                        |
                        v
                   MONGODB
```

## 2. Connection with Phase-03

Before understanding Phase-04, remember the connection-request statuses.

|Status|Meaning|Active?|
|---|---|---|
|`interested`|Request sent, waiting for review|Yes|
|`accepted`|Request accepted|Yes|
|`rejected`|Request rejected|No|
|`ignored`|Sender passed on the user|No|

Our product rule:

- Users with an active `interested` request should not appear in each other's feed.
    
- Accepted connections should not appear in the feed either.
    
- Rejected and ignored users may appear again.
    
- The logged-in user should never appear in their own feed.
    

We define:

```
const ACTIVE_STATUSES = ["interested", "accepted"];
```

This array represents the statuses that should hide another user from the feed.

**Important:** `ACTIVE_STATUSES` is an application-level constant. It is not a MongoDB feature.

---

# PART A — USER FEED

## 3. What is a user feed?

Imagine DevTinder has these users:

```
A = Pranav (logged-in user)
B = Rahul
C = Aman
D = Priya
E = Karan
F = Neha
```

Suppose these relationships exist:

```
A → B    interested
C → A    accepted
A → D    rejected
A → E    ignored
```

Which users should appear in A's feed?

Let's evaluate:

|User|Relationship with A|Show in feed?|
|---|---|---|
|A|Logged-in user|No|
|B|A sent interested request|No|
|C|C and A are connected|No|
|D|A's request was rejected|Yes|
|E|A ignored E|Yes|
|F|No relationship|Yes|

Therefore:

```
All Users
    |
    v
Remove myself
    |
    v
Remove users with active relationships
    |
    v
Remaining users
    |
    v
PAGINATED FEED
```

This is the core logic of `getFeed()`.

## 4. Understanding the feed validator

File:

`validators/user.validator.js`

Code:

```
import { query } from "express-validator";

const feedValidator = [
  query("page")
    .optional()
    .isInt({ min: 1 })
    .withMessage("page must be a positive integer"),

  query("limit")
    .optional()
    .isInt({ min: 1 })
    .withMessage("limit must be a positive integer"),
];

export { feedValidator };
```

### 4.1 Why do we need this validator?

Our feed supports pagination through URL query parameters.

Example:

```
GET /api/user/feed?page=1&limit=10
```

Here:

- `page` tells us which page to retrieve.
    
- `limit` tells us how many users to retrieve per page.
    

But clients can send incorrect values:

```
?page=0
?limit=-5
?page=hello
?limit=abc
```

We need to reject invalid inputs.

### 4.2 What is `query()`?

In Express, incoming URL data can exist in different places:

|Location|Express property|Validator|
|---|---|---|
|Route parameter|`req.params`|`param()`|
|Query parameter|`req.query`|`query()`|
|Request body|`req.body`|`body()`|

For example:

```
GET /user/feed?page=2&limit=10
```

Express makes:

```
req.query = {
  page: "2",
  limit: "10"
};
```

Notice that query values normally arrive as strings.

Therefore:

```
query("page")
```

means:

> Validate the page value from `req.query.page`.

### 4.3 What does `.optional()` do?

```
query("page").optional()
```

It means the parameter is not mandatory.

Both requests are allowed:

```
GET /user/feed
GET /user/feed?page=2
```

When the parameter is absent, the controller applies its default value.

### 4.4 What does `.isInt({ min: 1 })` do?

It validates that the supplied value is an integer of at least 1.

|Input|Result|
|---|---|
|`"1"`|Valid|
|`"10"`|Valid|
|`"100"`|Valid|
|`"0"`|Invalid|
|`"-1"`|Invalid|
|`"abc"`|Invalid|
|`"1.5"`|Invalid|

### 4.5 Why is there no maximum value in the validator?

Notice:

```
.isInt({ min: 1 })
```

There is no `max` option.

This is intentional.

The controller handles the maximum `limit` by clamping it.

For example:

```
?limit=500
```

Instead of returning an error, the server reduces it to:

```
limit = 50
```

This prevents clients from requesting excessively large pages.

**Important distinction:**

```
Validator:
Is limit a positive integer?

Controller:
What is the maximum limit the server will actually allow?
```

---

# 5. Understanding pagination

Pagination means dividing a large collection of records into smaller groups called pages.

Imagine there are 100 users eligible for your feed.

Returning all 100 at once is unnecessary.

Instead:

```
Page 1 → Users 1–10
Page 2 → Users 11–20
Page 3 → Users 21–30
...
Page 10 → Users 91–100
```

This improves response size and avoids sending unnecessary data.

### Without pagination

```
Client requests feed
       |
       v
Server retrieves 1000 users
       |
       v
Returns all 1000 users
```

Problems:

- Larger API responses.
    
- More memory usage.
    
- More unnecessary data transfer.
    
- Slower client-side rendering.
    

### With pagination

```
Client requests page 1
       |
       v
Server retrieves 10 users
       |
       v
Returns 10 users
```

The client can request the next page when needed.

---

# 6. Pagination constants

Your controller defines:

```
const DEFAULT_LIMIT = 10;
const MAX_LIMIT = 50;
```

Meaning:

|Constant|Purpose|
|---|---|
|`DEFAULT_LIMIT`|Default users per page|
|`MAX_LIMIT`|Maximum users per page|

Why do we need both?

Suppose a client sends:

```
GET /user/feed
```

The client did not specify a limit.

We use:

```
DEFAULT_LIMIT = 10
```

Now suppose someone requests:

```
GET /user/feed?limit=500
```

We apply:

```
MAX_LIMIT = 50
```

The client cannot force the server to return 500 users in one page.

This is a server-side resource-protection measure.

---

# 7. Understanding `getFeed()` step by step

Let's divide the controller into five logical operations:

```
getFeed()
    |
    +-- 1. Identify logged-in user
    |
    +-- 2. Calculate pagination
    |
    +-- 3. Find active relationships
    |
    +-- 4. Build exclusion list
    |
    +-- 5. Fetch users and pagination count
```

## 7.1 Identify the logged-in user

```
const loggedInUserId = req.user._id;
```

Remember authentication from previous phases.

```
Access Token
     |
     v
Authentication Middleware
     |
     v
Identify user
     |
     v
req.user
     |
     v
req.user._id
```

This gives us the logged-in user's ID.

Why is this important?

Because the feed must be personalized.

Every user gets a different feed depending on their existing relationships.

We do not accept the logged-in user ID from the client. We use the authenticated identity.

## 7.2 Calculate pagination

```
const page = Math.max(parseInt(req.query.page, 10) || 1, 1);

const limit = Math.min(
  Math.max(parseInt(req.query.limit, 10) || DEFAULT_LIMIT, 1),
  MAX_LIMIT,
);

const skip = (page - 1) * limit;
```

This is one of the most important pieces of the feed.

Let's understand each function.

### `parseInt()`

Converts a numeric string into an integer.

```
parseInt("10", 10); // 10
parseInt("25", 10); // 25
```

The second argument, `10`, specifies decimal (base-10) parsing.

### `||`

The logical OR operator.

```
parseInt(req.query.page, 10) || 1
```

means:

> Use the parsed page value, or use 1 if the result is falsy.

Since our validator rejects supplied zero and invalid values, the default mainly handles a missing parameter.

### `Math.max()`

Returns the larger number.

```
Math.max(5, 1); // 5
Math.max(0, 1); // 1
```

Here it ensures the page and limit do not fall below 1.

### `Math.min()`

Returns the smaller number.

```
Math.min(10, 50);  // 10
Math.min(100, 50); // 50
```

This ensures the requested limit never exceeds 50.

### Complete examples

|Client input|Page|Limit|
|---|---|---|
|No parameters|1|10|
|`?page=2`|2|10|
|`?limit=20`|1|20|
|`?page=3&limit=15`|3|15|
|`?limit=500`|1|50|

### 7.3 Understanding `skip()`

Now:

```
const skip = (page - 1) * limit;
```

`skip` tells MongoDB how many matching documents to skip before returning results.

Formula:

[  
skip = (page - 1) \times limit  
]

Example with limit = 10:

|Page|Calculation|Skip|
|---|---|---|
|1|(1 - 1) × 10|0|
|2|(2 - 1) × 10|10|
|3|(3 - 1) × 10|20|
|4|(4 - 1) × 10|30|

Visual:

```
All eligible users
------------------------------------------------
U1 U2 U3 U4 U5 U6 U7 U8 U9 U10
U11 U12 U13 U14 U15 U16 U17 U18 U19 U20
U21 U22 U23 U24 U25 U26 U27 U28 U29 U30
------------------------------------------------

Page 1: Skip 0  → Return U1–U10
Page 2: Skip 10 → Return U11–U20
Page 3: Skip 20 → Return U21–U30
```

### 7.4 Understanding `limit()`

`limit()` tells MongoDB the maximum number of documents to return.

```
.limit(limit)
```

Example:

```
userModel.find(filter)
  .skip(10)
  .limit(10);
```

Meaning:

1. Find matching users.
    
2. Skip the first 10.
    
3. Return up to the next 10.
    

**Remember:**

```
skip()  → How many documents to bypass?
limit() → How many documents to return?
```

---

# 8. Find all active requests involving the logged-in user

Code:

```
const activeRequests = await connectionRequestModel
  .find({
    status: { $in: ACTIVE_STATUSES },
    $or: [
      { fromUserId: loggedInUserId },
      { toUserId: loggedInUserId },
    ],
  })
  .select("fromUserId toUserId");
```

This is the core database query used to build the feed exclusion list.

Let's understand it.

### 8.1 Why use `connectionRequestModel`?

Because the connection-request collection contains information about relationships between users.

We need to know:

- Who sent a request?
    
- Who received it?
    
- Is it still active?
    

So we query the ConnectionRequest collection rather than performing a separate query for each user.

### 8.2 Understanding `$in`

```
status: {
  $in: ACTIVE_STATUSES
}
```

Our constant is:

```
const ACTIVE_STATUSES = ["interested", "accepted"];
```

MongoDB `$in` means:

> Match a document if its field value matches at least one value in the supplied array.

Equivalent:

```
status: {
  $in: ["interested", "accepted"]
}
```

|Status|Matches?|
|---|---|
|interested|Yes|
|accepted|Yes|
|rejected|No|
|ignored|No|

### 8.3 Understanding `$or`

```
$or: [
  { fromUserId: loggedInUserId },
  { toUserId: loggedInUserId },
]
```

MongoDB `$or` means:

> At least one of the specified conditions must be true.

Suppose the logged-in user is A.

We need to find:

```
A → B
```

and:

```
C → A
```

The first condition:

```
{ fromUserId: loggedInUserId }
```

finds requests sent by A.

The second condition:

```
{ toUserId: loggedInUserId }
```

finds requests received by A.

Therefore:

```
              Logged-in user A
                      |
               +------+------+
               |             |
               v             v
             A → B         C → A
            Outgoing       Incoming
```

Both directions matter because either kind of active request should hide the other user from the feed.

### 8.4 Combining `$in` and `$or`

The full query:

```
{
  status: { $in: ["interested", "accepted"] },

  $or: [
    { fromUserId: loggedInUserId },
    { toUserId: loggedInUserId },
  ],
}
```

means:

> Find connection requests whose status is active AND where the logged-in user is either the sender or the receiver.

Notice the logical relationship:

```
ACTIVE STATUS
     AND
(
  I AM THE SENDER
      OR
  I AM THE RECEIVER
)
```

### 8.5 Why `.select()`?

```
.select("fromUserId toUserId");
```

Normally, MongoDB would return the complete matching documents.

But we only need:

```
fromUserId
toUserId
```

We don't need:

```
status
createdAt
updatedAt
```

because the query has already filtered by status.

`.select()` is Mongoose field projection.

It limits the fields retrieved from the database.

MongoDB generally includes `_id` unless explicitly excluded.

This reduces unnecessary data retrieval.

---

# 9. Building the exclusion list using `Set`

Code:

```
const excludedIds = new Set([loggedInUserId.toString()]);

activeRequests.forEach((request) => {
  excludedIds.add(request.fromUserId.toString());
  excludedIds.add(request.toUserId.toString());
});
```

This is another important programming concept.

### 9.1 What is an exclusion list?

An exclusion list contains the IDs of users who must not appear in our feed.

It includes:

1. The logged-in user.
    
2. Users having active relationships with the logged-in user.
    

Example:

```
Logged-in user = A

Active requests:

A → B
C → A
A → D
```

We need to exclude:

```
A, B, C, D
```

### 9.2 What is a JavaScript Set?

A `Set` is a JavaScript data structure that stores unique values.

Example:

```
const ids = new Set();

ids.add("A");
ids.add("B");
ids.add("C");
ids.add("B");

console.log(ids);
```

Conceptually:

```
A
B
C
```

B does not appear twice.

**Important property:**

A Set automatically prevents duplicate values.

### 9.3 Why initialize it with the logged-in user?

```
new Set([loggedInUserId.toString()])
```

Because we must never display ourselves in our own feed.

Even if there are no connection requests, our own profile must be excluded.

### 9.4 Why add both IDs from every request?

```
activeRequests.forEach((request) => {
  excludedIds.add(request.fromUserId.toString());
  excludedIds.add(request.toUserId.toString());
});
```

Consider:

```
A → B
```

A is the logged-in user.

We add:

```
A
B
```

Now consider:

```
C → A
```

We add:

```
C
A
```

The Set automatically handles duplicates.

Final result:

```
excludedIds = {A, B, C, D}
```

### 9.5 Why convert ObjectIds to strings?

```
request.fromUserId.toString()
```

MongoDB ObjectIds are objects.

A JavaScript Set compares objects by reference identity, not by their internal ObjectId value.

Two different ObjectId objects can represent the same database ID.

Converting them into strings gives us a consistent primitive representation for deduplication.

### 9.6 Why use `forEach()` instead of a query inside a loop?

Imagine we have 100 active requests.

A poor approach would be:

```
Find active requests
       |
       v
For each request:
       |
       v
Perform another database query
```

This creates unnecessary database calls.

Your implementation:

```
One database query
       |
       v
Receive all matching IDs
       |
       v
Process IDs in JavaScript
       |
       v
Build one Set
```

This is more efficient.

---

# 10. Understanding `$nin`

Code:

```
const filter = {
  _id: {
    $nin: Array.from(excludedIds),
  },
};
```

We have understood `$in`.

Now the opposite:

`$nin` means **not in**.

It selects documents whose field value does not match any value in the supplied array.

Example:

```
{
  _id: {
    $nin: ["A", "B", "C"]
  }
}
```

Meaning:

> Find users whose `_id` is not A, B, or C.

### Why `Array.from()`?

Our exclusion list is a Set.

```
excludedIds
```

But we want an ordinary array for the MongoDB query.

```
Array.from(excludedIds)
```

converts the Set into an array.

Example:

```
const ids = new Set(["A", "B", "C"]);

const result = Array.from(ids);

// ["A", "B", "C"]
```

### Complete flow

```
Active Connection Requests
           |
           v
       JavaScript Set
           |
           v
     Unique excluded IDs
           |
           v
      Array.from(Set)
           |
           v
          $nin
           |
           v
      MongoDB User Query
```

### Important: why does `$nin` work here?

Suppose:

```
All user IDs = A, B, C, D, E, F

Excluded IDs = A, B, C
```

Then:

```
{
  _id: {
    $nin: ["A", "B", "C"]
  }
}
```

returns:

```
D, E, F
```

This is the basic mechanism behind your feed.

---

# 11. Fetch users and count in parallel

Code:

```
const [users, total] = await Promise.all([
  userModel
    .find(filter)
    .select(PUBLIC_USER_FIELDS)
    .sort({ _id: -1 })
    .skip(skip)
    .limit(limit),

  userModel.countDocuments(filter),
]);
```

This is one of the most useful concepts in this phase.

We need two things:

1. The actual users for the requested page.
    
2. The total number of eligible users.
    

These are two separate database operations.

### 11.1 What does `countDocuments()` do?

```
userModel.countDocuments(filter)
```

Counts documents matching the filter.

It does not return user documents.

Example:

```
const total = await userModel.countDocuments(filter);
```

If 47 users satisfy the feed conditions:

```
total = 47
```

This is needed for pagination metadata.

### 11.2 Why do we need total?

Suppose we return 10 users.

The client also needs to know:

- How many pages exist?
    
- Is there another page?
    
- How many eligible users exist in total?
    

The total count helps answer these questions.

### 11.3 What is `Promise.all()`?

`Promise.all()` executes multiple promises concurrently and waits until all of them complete.

Without it:

```
const users = await userModel.find(filter);

const total = await userModel.countDocuments(filter);
```

The second operation starts only after the first finishes.

With:

```
const [users, total] = await Promise.all([
  userModel.find(filter),
  userModel.countDocuments(filter),
]);
```

both operations are started without waiting for one another.

Visual:

```
              Promise.all()
                    |
             +------+------+
             |             |
             v             v
         Find Users    Count Users
             |             |
             v             v
         Page Data     Total Count
             |             |
             +------+------+
                    |
                    v
               Both finish
                    |
                    v
               Continue
```

### 11.4 Why is this useful?

The two operations do not depend on each other's results.

Both use the same filter.

Therefore, they can run concurrently.

One technical qualification: `Promise.all()` does not guarantee a transaction or a single database snapshot. If users are added or removed between the operations, the returned user list and total may differ slightly.

For this feed, that is generally an acceptable trade-off.

---

# 12. Understanding `PUBLIC_USER_FIELDS`

Code:

```
const PUBLIC_USER_FIELDS =
  "firstName lastName photoUrl age gender about skills";
```

This is extremely important for API security.

Your User model contains fields such as:

```
firstName
lastName
email
password
age
gender
about
skills
photoUrl
role
isPremium
membership
```

Should every user see all these fields?

No.

For example:

- `password` must never be returned.
    
- `email` should not be exposed through the public feed.
    
- `role` should not be exposed unnecessarily.
    
- `isPremium` and membership information should not be exposed unless a product feature requires it.
    

Therefore, we define a public-field whitelist.

### What is a whitelist?

A whitelist specifies exactly what is allowed.

Instead of trying to remove every sensitive field, we explicitly select only the fields that are permitted.

```
USER DOCUMENT
     |
     +-- firstName
     +-- lastName
     +-- email
     +-- password
     +-- age
     +-- gender
     +-- about
     +-- skills
     +-- photoUrl
     +-- role
     +-- isPremium
     |
     v
PUBLIC_USER_FIELDS
     |
     v
Only approved fields returned
```

Usage:

```
.select(PUBLIC_USER_FIELDS)
```

The `_id` field is included by default in an inclusion projection unless explicitly excluded.

### Why is this important?

Imagine returning the complete user document:

```
res.json({
  data: { users }
});
```

If `users` contains unrestricted database documents, sensitive information could accidentally reach the client.

A field whitelist minimizes that risk.

**Remember:** Whenever you add a new field to the User model, decide explicitly whether that field should be publicly exposed.

Also, `age` and `gender` are treated as public by this particular product design. If that privacy policy changes, remove them from the whitelist.

---

# 13. Understanding `.sort({ _id: -1 })`

Code:

```
.sort({ _id: -1 })
```

MongoDB needs a consistent order for paginated results.

Suppose we have:

```
A, B, C, D, E, F...
```

If there is no sort, MongoDB does not guarantee a particular order.

One request might return:

```
Page 1: A, B, C
```

Another request might return:

```
Page 2: C, D, E
```

This can create overlapping pages.

Therefore, we specify an ordering.

### What does `-1` mean?

In MongoDB:

```
.sort({ _id: 1 })
```

means ascending.

```
.sort({ _id: -1 })
```

means descending.

So:

```
.sort({ _id: -1 })
```

sorts by `_id` in descending order.

Because `_id` is unique, this gives a deterministic ordering for a fixed dataset.

### Important distinction

A stable sort helps prevent arbitrary page ordering, but it does not completely eliminate duplication or skipping when the dataset changes between page requests.

For example, a new user inserted before page 2 is fetched can shift the offset.

Later, cursor-based pagination can solve much of this problem more effectively.

Also, `_id` order is not a strict guarantee of real-world chronological creation order in every application. It is being used here primarily for consistent ordering.

---

# 14. Understanding the complete user query

```
userModel
  .find(filter)
  .select(PUBLIC_USER_FIELDS)
  .sort({ _id: -1 })
  .skip(skip)
  .limit(limit);
```

Let's understand the chain.

|Method|Purpose|
|---|---|
|`find(filter)`|Find users who are not excluded|
|`select()`|Return only public fields|
|`sort()`|Apply consistent ordering|
|`skip()`|Skip previous pages|
|`limit()`|Return only the requested number|

Visual:

```
             USER COLLECTION
                    |
                    v
              find(filter)
                    |
                    v
             Eligible Users
                    |
                    v
                select()
                    |
                    v
             Public Fields Only
                    |
                    v
                 sort()
                    |
                    v
              Ordered Results
                    |
                    v
                 skip()
                    |
                    v
             Skip Previous Pages
                    |
                    v
                limit()
                    |
                    v
               Current Page
```

---

# 15. Understanding the pagination response

Code:

```
return res.status(200).json({
  status: "success",
  message: "Feed retrieved successfully",
  data: {
    users,
    pagination: {
      page,
      limit,
      total,
      totalPages: Math.ceil(total / limit),
      hasMore: skip + users.length < total,
    },
  },
});
```

The response contains two main things:

```
data
 |
 +-- users
 |
 +-- pagination
```

### 15.1 `users`

The user documents for the current page.

### 15.2 `page`

The current requested page.

### 15.3 `limit`

The actual server-approved page size.

Remember, it may be lower than the client-requested value.

### 15.4 `total`

The total number of users eligible for the feed, before applying pagination.

Example:

```
total = 25
```

### 15.5 `totalPages`

Code:

```
Math.ceil(total / limit)
```

`Math.ceil()` rounds a number up to the next integer.

Example:

```
total = 25
limit = 10

25 / 10 = 2.5

Math.ceil(2.5) = 3
```

Therefore:

```
totalPages = 3
```

### 15.6 `hasMore`

Code:

```
skip + users.length < total
```

This tells the client whether more eligible users remain after the current page.

Example:

```
total = 25
page = 2
limit = 10

skip = (2 - 1) × 10 = 10
users.length = 10

10 + 10 < 25

20 < 25

hasMore = true
```

There are more users.

For page 3:

```
skip = 20
users.length = 5

20 + 5 < 25

25 < 25

hasMore = false
```

No more users remain.

### Complete example

```
{
  "status": "success",
  "message": "Feed retrieved successfully",
  "data": {
    "users": [],
    "pagination": {
      "page": 2,
      "limit": 10,
      "total": 25,
      "totalPages": 3,
      "hasMore": true
    }
  }
}
```

The empty `users` array above is illustrative; the actual response contains the returned user documents.

---

# 16. Complete feed flow diagram

```
                GET /api/user/feed
                         |
                         v
                  AUTHENTICATION
                         |
                         v
                    VALIDATION
                         |
                         v
                      getFeed()
                         |
                         v
                 Identify logged-in user
                         |
                         v
                 Calculate pagination
                   page, limit, skip
                         |
                         v
             Find active requests involving me
                         |
                         v
             Extract both users from requests
                         |
                         v
                Build exclusion Set
                         |
                         v
                 Convert Set to array
                         |
                         v
                Build $nin user filter
                         |
                         v
              +----------+----------+
              |                     |
              v                     v
          Fetch users           Count users
              |                     |
              v                     v
       Filter + projection       Total count
       + sort + skip + limit        |
              |                     |
              +----------+----------+
                         |
                         v
                 Build pagination
                    metadata
                         |
                         v
                    HTTP 200
```

---

# PART B — RECEIVED CONNECTION REQUESTS

# 17. What are received requests?

In Phase-03, we created a system where:

```
A → B
interested
```

means A wants to connect with B.

B should have a way to see pending incoming requests.

That is what this endpoint provides:

```
GET /api/user/requests/received
```

Its purpose:

> Return all interested requests where the logged-in user is the recipient.

## 18. Understanding `getReceivedRequests()`

Code:

```
const getReceivedRequests = async (req, res, next) => {
  try {
    const requests = await connectionRequestModel
      .find({
        toUserId: req.user._id,
        status: "interested",
      })
      .populate("fromUserId", PUBLIC_USER_FIELDS)
      .sort({ createdAt: -1 });

    return res.status(200).json({
      status: "success",
      message: "Received requests retrieved successfully",
      data: { requests },
    });
  } catch (error) {
    next(error);
  }
};
```

Let's break it down.

### 18.1 Find incoming requests

```
.find({
  toUserId: req.user._id,
  status: "interested",
})
```

Suppose logged-in user is B.

The query searches for:

```
A → B    interested
C → B    interested
D → B    interested
```

It does NOT return:

```
B → A    interested
```

because that is an outgoing request, not an incoming one.

It also does not return:

```
A → B    rejected
A → B    accepted
A → B    ignored
```

because we only want pending requests.

### 18.2 Why not use `$or` here?

In the feed, we needed both directions.

Here, we specifically want incoming requests.

Therefore only:

```
toUserId: req.user._id
```

is required.

This is a good example of choosing a query based on the actual business requirement instead of adding unnecessary conditions.

---

# 19. What is `populate()`?

This is one of the most important Mongoose concepts in Phase-04.

Remember your ConnectionRequest model:

```
fromUserId: {
  type: mongoose.Schema.Types.ObjectId,
  ref: "User",
},

toUserId: {
  type: mongoose.Schema.Types.ObjectId,
  ref: "User",
},
```

The `ref: "User"` tells Mongoose which model these ObjectIds refer to.

By default, a connection request stores references, not complete user documents.

Example stored document:

```
{
  fromUserId: ObjectId("USER_A_ID"),
  toUserId: ObjectId("USER_B_ID"),
  status: "interested"
}
```

But when displaying incoming requests, the frontend needs to know who sent the request.

It needs:

```
firstName
lastName
photoUrl
about
skills
```

We could manually query the sender separately.

But Mongoose provides `populate()`.

### 19.1 Basic populate

```
.populate("fromUserId")
```

means:

> Replace the `fromUserId` reference with the corresponding User document.

Conceptually:

Before populate:

```
{
  fromUserId: ObjectId("USER_A_ID"),
  toUserId: ObjectId("USER_B_ID"),
  status: "interested"
}
```

After populate:

```
{
  fromUserId: {
    _id: "USER_A_ID",
    firstName: "Pranav",
    lastName: "Chandel"
  },

  toUserId: ObjectId("USER_B_ID"),

  status: "interested"
}
```

The stored connection-request document is not permanently replaced by the populated data. Populate resolves the reference when retrieving the result.

### 19.2 Populate with field selection

Your code:

```
.populate("fromUserId", PUBLIC_USER_FIELDS)
```

means:

1. Find the user referenced by `fromUserId`.
    
2. Retrieve that user.
    
3. Return only approved public fields.
    
4. Attach the populated result to `fromUserId`.
    

Visual:

```
ConnectionRequest Collection
          |
          v
      fromUserId
          |
          v
       ObjectId
          |
          v
     User Collection
          |
          v
    Find referenced user
          |
          v
    Select public fields
          |
          v
   Populate fromUserId
```

Why is this useful?

Because the frontend needs to display information about the sender, not just an ObjectId.

### 19.3 Why only populate `fromUserId`?

Because:

```
fromUserId = sender
toUserId   = logged-in user
```

The frontend needs the sender's profile information.

It already knows which user is logged in.

So populating the receiver is unnecessary.

---

# 20. Understanding `.sort({ createdAt: -1 })`

```
.sort({ createdAt: -1 })
```

This sorts incoming requests by creation date.

Descending order means newest first.

Example:

```
Request A → received at 10:00
Request B → received at 11:00
Request C → received at 12:00
```

Result:

```
C
B
A
```

This ensures newer requests appear before older requests.

### Important

Unlike the feed, this query does not use pagination.

It returns all matching pending requests.

For the current project scope, this is acceptable. When the list grows, add pagination to avoid returning an unbounded number of records.

---

# 21. Received requests — visual flow

```
             GET /api/user/requests/received
                            |
                            v
                     Authentication
                            |
                            v
                   getReceivedRequests()
                            |
                            v
                    Logged-in user ID
                            |
                            v
                  Query ConnectionRequest
                            |
                            v
              toUserId = logged-in user
                            AND
                  status = interested
                            |
                            v
                   Find matching requests
                            |
                            v
                  Populate fromUserId
                            |
                            v
                  Select public fields
                            |
                            v
                 Sort newest requests first
                            |
                            v
                         HTTP 200
```

---

# PART C — ACCEPTED CONNECTIONS

# 22. What are connections?

In Phase-03, a recipient can accept a connection request.

Example:

```
A → B
interested
```

After B accepts:

```
A → B
accepted
```

Now A and B are connections.

But remember:

The sender and receiver can be either side of an accepted request.

For example:

```
A → B accepted
```

or:

```
C → A accepted
```

When A views their connections, both B and C must appear.

Therefore we need a query that checks both directions.

Endpoint:

```
GET /api/user/connections
```

---

# 23. Understanding `getConnections()`

Code:

```
const getConnections = async (req, res, next) => {
  try {
    const loggedInUserId = req.user._id;

    const accepted = await connectionRequestModel
      .find({
        status: "accepted",
        $or: [
          { fromUserId: loggedInUserId },
          { toUserId: loggedInUserId },
        ],
      })
      .populate("fromUserId", PUBLIC_USER_FIELDS)
      .populate("toUserId", PUBLIC_USER_FIELDS)
      .sort({ updatedAt: -1 });
```

Let's understand this query.

## 23.1 Why status = accepted?

```
status: "accepted"
```

We only want actual connections.

We do not want:

```
interested
rejected
ignored
```

## 23.2 Why `$or`?

```
$or: [
  { fromUserId: loggedInUserId },
  { toUserId: loggedInUserId },
]
```

Because the logged-in user can be either:

- The sender.
    
- The receiver.
    

Example, logged-in user A:

```
A → B accepted
C → A accepted
```

Both should be returned.

Visual:

```
                USER A
                   |
          +--------+--------+
          |                 |
          v                 v
        A → B             C → A
          |                 |
          v                 v
          B                 C

       Both are A's connections
```

## 23.3 Why populate BOTH fields?

```
.populate("fromUserId", PUBLIC_USER_FIELDS)
.populate("toUserId", PUBLIC_USER_FIELDS)
```

Because we don't know which side contains the logged-in user.

We need the documents for both sides to identify the other person.

Example:

Before populate:

```
{
  fromUserId: ObjectId("A"),
  toUserId: ObjectId("B"),
  status: "accepted"
}
```

After populate:

```
{
  fromUserId: {
    _id: "A",
    firstName: "Pranav"
  },

  toUserId: {
    _id: "B",
    firstName: "Rahul"
  },

  status: "accepted"
}
```

Now we can determine which person is not the logged-in user.

## 23.4 Why sort by `updatedAt`?

```
.sort({ updatedAt: -1 })
```

When a request changes from:

```
interested → accepted
```

Mongoose updates `updatedAt` because timestamps are enabled.

Thus, the current implementation uses `updatedAt` as the approximate connection-acceptance time.

Newest accepted relationships appear first.

One future improvement: introduce a dedicated `acceptedAt` field if the application needs a permanent, semantically accurate connection timestamp. `updatedAt` can change again if the document is updated later.

---

# 24. Understanding the `map()` operation

This is the most important piece of `getConnections()`.

Code:

```
const connections = accepted.map((request) => ({
  requestId: request._id,
  connectedAt: request.updatedAt,
  user: request.fromUserId._id.equals(loggedInUserId)
    ? request.toUserId
    : request.fromUserId,
}));
```

Let's understand why this transformation is necessary.

### 24.1 What is `.map()`?

JavaScript `map()` is an array method.

It transforms each element into a new value and returns a new array.

Example:

```
const numbers = [1, 2, 3];

const doubled = numbers.map((number) => number * 2);

// [2, 4, 6]
```

We use it to transform database connection-request documents into a simpler API response.

### 24.2 Why do we need transformation?

The database document contains:

```
{
  fromUserId: A,
  toUserId: B,
  status: "accepted"
}
```

But the frontend doesn't need to determine who is the other person for every relationship.

We want a consistent response:

```
{
  requestId: "...",
  connectedAt: "...",
  user: {
    firstName: "...",
    lastName: "...",
    photoUrl: "..."
  }
}
```

No matter whether the logged-in user was the sender or receiver.

### 24.3 Understanding `.equals()`

```
request.fromUserId._id.equals(loggedInUserId)
```

Because `fromUserId` has been populated, its `_id` is an ObjectId.

`.equals()` compares ObjectId values.

It asks:

> Is the sender's ID the same as the logged-in user's ID?

### 24.4 Understanding the ternary operator

Your code:

```
request.fromUserId._id.equals(loggedInUserId)
  ? request.toUserId
  : request.fromUserId
```

A ternary operator has this structure:

```
condition ? valueIfTrue : valueIfFalse;
```

So:

- If the logged-in user is the sender, return the receiver.
    
- Otherwise, return the sender.
    

Example 1:

```
A → B accepted

Logged-in user = A

fromUserId = A

Is A equal to A?
YES

Return toUserId = B
```

Example 2:

```
C → A accepted

Logged-in user = A

fromUserId = C

Is C equal to A?
NO

Return fromUserId = C
```

Both return the other person.

### Complete visualization

```
               ACCEPTED REQUEST
                       |
                A → B accepted
                       |
                       v
             Is logged-in user
             equal to fromUserId?
                       |
                 +-----+-----+
                 |           |
                YES          NO
                 |           |
                 v           v
             Return       Return
             toUserId   fromUserId
                 |           |
                 +-----+-----+
                       |
                       v
               OTHER USER ONLY
```

### 24.5 Why include `requestId`?

```
requestId: request._id
```

This is the ID of the ConnectionRequest document, not the User document.

Why is it useful?

Because in Phase-05, when building the chat system, we need a way to associate a chat with the relevant connection.

The request ID gives us that relationship reference.

### 24.6 Why include `connectedAt`?

```
connectedAt: request.updatedAt
```

This provides the frontend with a timestamp for displaying connection information.

For example:

```
Connected on September 17
```

Remember the distinction:

```
requestId → ConnectionRequest document ID

user      → Other person's User document

connectedAt → Timestamp currently based on updatedAt
```

---

# 25. Complete connections response

Conceptual example:

```
{
  "status": "success",
  "message": "Connections retrieved successfully",
  "data": {
    "connections": [
      {
        "requestId": "REQUEST_ID_1",
        "connectedAt": "2026-09-17T10:30:00.000Z",
        "user": {
          "_id": "USER_B",
          "firstName": "Rahul",
          "lastName": "Sharma",
          "photoUrl": "...",
          "about": "Developer",
          "skills": ["JavaScript", "React"]
        }
      }
    ]
  }
}
```

The IDs and values above are illustrative.

---

# PART D — DATABASE INDEXES

# 26. Why are we adding another index?

Your Phase-03 model already has:

```
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

Phase-04 introduces more queries involving:

```
toUserId
status
```

Specifically:

```
.find({
  toUserId: loggedInUserId,
  status: "interested",
})
```

and the incoming side of:

```
.find({
  status: "accepted",
  $or: [
    { fromUserId: loggedInUserId },
    { toUserId: loggedInUserId },
  ],
})
```

The existing compound index starts with `fromUserId`.

A compound index is ordered by its indexed fields, so an index beginning with `fromUserId` is not an equivalent replacement for one beginning with `toUserId`.

Therefore we add:

```
connectionRequestSchema.index({
  toUserId: 1,
  status: 1,
});
```

## 27. Understanding the new compound index

This:

```
{
  toUserId: 1,
  status: 1
}
```

is a compound index.

It indexes two fields together:

1. `toUserId`
    
2. `status`
    

It can help MongoDB locate documents using filters on those fields.

### Why this field order?

Our common query is:

```
{
  toUserId: loggedInUserId,
  status: "interested",
}
```

The index starts with `toUserId`, then `status`.

This is a suitable structure for that query.

### Why not make it unique?

Because a user can receive multiple requests.

Example:

```
A → B interested
C → B interested
D → B interested
```

These are all valid.

We want efficient lookup, not uniqueness.

So:

```
connectionRequestSchema.index({
  toUserId: 1,
  status: 1,
});
```

is a normal, non-unique compound index.

### Existing versus new index

|Index|Main purpose|
|---|---|
|`{ fromUserId: 1, toUserId: 1 }` with partial unique rule|Protect active ordered-pair uniqueness and support sender-side lookups|
|`{ toUserId: 1, status: 1 }`|Help recipient-side status queries|

### Important distinction

The first index enforces a uniqueness rule.

The new index mainly improves query performance.

**Not every index is a unique index.**

Indexes also have a cost: they consume storage and must be maintained during writes. Add them for real query patterns, and verify their benefit with MongoDB's `explain()` when the application grows.

---

# PART E — ROUTES AND APPLICATION MOUNTING

# 28. Understanding `user.routes.js`

Code:

```
import { Router } from "express";

import authenticate from "../middlewares/authenticate.middleware.js";
import validate from "../middlewares/validate.middleware.js";

import { feedValidator } from "../validators/user.validator.js";

import {
  getFeed,
  getReceivedRequests,
  getConnections,
} from "../controllers/user.controller.js";

const userRouter = Router();

userRouter.get(
  "/feed",
  authenticate,
  feedValidator,
  validate,
  getFeed
);

userRouter.get(
  "/requests/received",
  authenticate,
  getReceivedRequests
);

userRouter.get(
  "/connections",
  authenticate,
  getConnections
);

export default userRouter;
```

### 28.1 What is `Router()`?

Express provides Router to group related routes into a separate module.

Instead of keeping every endpoint in `app.js`, we organize them by feature.

Example:

```
routes/
  auth.routes.js
  user.routes.js
  request.routes.js
```

This makes the application easier to maintain.

### 28.2 Why do we need `authenticate` on all three routes?

All three endpoints return information specific to the logged-in user.

For example:

```
GET /user/feed
```

must know whose feed to generate.

Similarly:

```
GET /user/connections
```

must know which connections to return.

Therefore all routes need authentication.

### 28.3 Middleware sequence

Feed:

```
userRouter.get(
  "/feed",
  authenticate,
  feedValidator,
  validate,
  getFeed
);
```

Execution order:

```
Request
   |
   v
authenticate
   |
   v
feedValidator
   |
   v
validate
   |
   v
getFeed
```

For the other two routes, no query validator is currently necessary because they do not accept pagination parameters or dynamic URL parameters.

### 28.4 Mounting in `app.js`

Code:

```
import userRouter from "./routes/user.routes.js";

app.use("/api/user", userRouter);
```

This mounts all routes from `userRouter` under:

```
/api/user
```

Therefore:

|Router path|Final endpoint|
|---|---|
|`/feed`|`/api/user/feed`|
|`/requests/received`|`/api/user/requests/received`|
|`/connections`|`/api/user/connections`|

Visual:

```
app.js
  |
  +-- app.use("/api/user", userRouter)
                             |
                             +-- /feed
                             |
                             +-- /requests/received
                             |
                             +-- /connections
```

---

# PART F — COMPLETE PHASE-04 ARCHITECTURE

# 29. How all three endpoints work together

```
                     AUTHENTICATED USER
                             |
                 +-----------+-----------+
                 |           |           |
                 v           v           v
               FEED       RECEIVED    CONNECTIONS
                 |           |           |
                 v           v           v
          User Collection  Connection  Connection
                              Request     Request
                              Collection  Collection
                 |           |           |
                 v           v           v
          Exclude self   Find incoming  Find accepted
          and active     interested     relationships
          relationships requests       in either direction
                 |           |           |
                 v           v           v
            Pagination    Populate      Populate both
                 |        sender         users
                 |           |           |
                 v           v           v
           Public users  Request list  Other user + requestId
                 |           |           |
                 +-----------+-----------+
                             |
                             v
                        API RESPONSE
```

## 30. Responsibility of each file

|File|Responsibility|
|---|---|
|`validators/user.validator.js`|Validate feed query parameters|
|`controllers/user.controller.js`|Implement feed, received-request and connection logic|
|`routes/user.routes.js`|Define endpoints and middleware sequence|
|`user.model.js`|Access User collection|
|`connectionRequest.model.js`|Access connection-request collection|
|`app.js`|Mount user routes|

---

# PART G — TESTING AND EDGE CASES

# 31. Test the feed

Seed at least 10 users.

Test the following conditions:

|Test|Expected result|
|---|---|
|Logged-in user requests feed|Self is excluded|
|A has sent interested to B|B is excluded for A|
|B has sent interested to A|B is excluded for A|
|A and B are accepted connections|Both are excluded from each other's feed|
|A's request to B was rejected|B may appear again in A's feed|
|A ignored B|B may appear again in A's feed|
|No eligible users|Empty array|

### Why test both directions?

Because the feed exclusion query has:

```
$or: [
  { fromUserId: loggedInUserId },
  { toUserId: loggedInUserId },
]
```

Testing only outgoing requests would leave the incoming-request case untested.

# 32. Test pagination

Suppose there are 25 eligible users.

Use:

```
GET /api/user/feed?page=1&limit=10
```

Expected:

```
10 users
page = 1
total = 25
totalPages = 3
hasMore = true
```

Page 2:

```
GET /api/user/feed?page=2&limit=10
```

Expected:

```
10 users
hasMore = true
```

Page 3:

```
GET /api/user/feed?page=3&limit=10
```

Expected:

```
5 users
hasMore = false
```

Page 4:

```
GET /api/user/feed?page=4&limit=10
```

Expected:

```
users = []
hasMore = false
```

### Test the limit cap

```
GET /api/user/feed?limit=500
```

Expected:

```
limit = 50
```

### Test invalid inputs

```
GET /api/user/feed?page=0
GET /api/user/feed?limit=0
GET /api/user/feed?page=abc
GET /api/user/feed?limit=-10
```

Expected:

```
400 Bad Request
```

# 33. Test received requests

Suppose:

```
A → B interested
C → B interested
D → A interested
```

Logged-in user:

```
B
```

Expected:

```
A → B
C → B
```

The request:

```
D → A
```

must not appear because B is not the recipient.

Also verify that sender details are populated:

```
firstName
lastName
photoUrl
about
skills
```

# 34. Test connections

Suppose:

```
A → B accepted
C → A accepted
D → E accepted
```

Logged-in user:

```
A
```

Expected:

```
B
C
```

Not:

```
A
D
E
```

Also verify that each connection includes:

```
requestId
connectedAt
user
```

# 35. Test public-field protection

Check all three API responses.

They must not expose:

```
password
email
role
isPremium
membership
```

The expected user information should contain only approved public fields.

---

# PART H — IMPORTANT TECHNICAL CONCEPTS AND LIMITATIONS

# 36. `find()` vs `findOne()` vs `findById()`

|Method|Purpose|
|---|---|
|`find()`|Returns all matching documents|
|`findOne()`|Returns one matching document|
|`findById(id)`|Finds one document by `_id`|

Examples:

```
User.find({ age: 22 });
```

Returns matching users.

```
User.findOne({ email: "test@example.com" });
```

Returns one matching user or `null`.

```
User.findById(userId);
```

Finds a user by `_id`.

# 37. `select()` vs `populate()`

These concepts are different.

### `select()`

Controls which fields are returned from a document.

```
User.find().select("firstName lastName");
```

### `populate()`

Resolves an ObjectId reference into its referenced document.

```
ConnectionRequest.find()
  .populate("fromUserId", "firstName lastName");
```

### Combined example

```
.populate("fromUserId", PUBLIC_USER_FIELDS)
```

means:

> Resolve the referenced sender and return only approved fields from their User document.

# 38. `map()` vs `forEach()`

Another useful distinction:

|Method|Purpose|
|---|---|
|`forEach()`|Performs an operation for every array element|
|`map()`|Transforms every element and returns a new array|

In your code:

```
activeRequests.forEach(...)
```

builds the exclusion Set.

But:

```
accepted.map(...)
```

transforms accepted request documents into connection response objects.

# 39. `Promise.all()` vs sequential `await`

Sequential:

```
const users = await User.find(filter);
const total = await User.countDocuments(filter);
```

The second operation starts after the first completes.

Concurrent:

```
const [users, total] = await Promise.all([
  User.find(filter),
  User.countDocuments(filter),
]);
```

Both start without waiting for the other.

Use `Promise.all()` when multiple independent asynchronous operations are required.

# 40. Important pagination limitations

Your current implementation uses offset pagination:

```
.skip(skip)
.limit(limit)
```

This is perfectly reasonable for the current learning stage.

However, there are two limitations worth understanding.

**1. Large offsets can become expensive.**

For example:

```
page = 100000
limit = 50
```

requires skipping a very large number of matching records.

**2. Data changes can shift pages.**

Suppose the client loads page 1, and new users are inserted before it loads page 2. The offset can shift, producing repeated or skipped results.

Later, you can move to cursor-based pagination using an ordered field such as `_id`.

You do not need to redesign the feed now.

# 41. Important limitation of the exclusion query

Your feed first retrieves all active requests involving the logged-in user:

```
connectionRequestModel.find(...)
```

Then it creates an exclusion list in application memory.

For a normal project's current scale, this is understandable and straightforward.

For users with extremely large numbers of relationships, this list could become large.

At that stage, an aggregation-based approach or a redesigned relationship data structure may be worth evaluating.

# 42. Potential robustness improvement for populated connections

The current mapping assumes both populated users exist:

```
request.fromUserId._id.equals(loggedInUserId)
```

If a referenced User document is deleted, Mongoose population can return `null`.

The current code would then fail when accessing `._id`.

This is not necessarily a problem under your current user-deletion policy, but it is an edge case to address if account deletion is introduced.

# 43. Important indexes to remember

After Phase-04, your ConnectionRequest schema has two relevant indexes:

```
// Phase-03
connectionRequestSchema.index(
  { fromUserId: 1, toUserId: 1 },
  {
    unique: true,
    partialFilterExpression: {
      status: { $in: ["interested", "accepted"] },
    },
  },
);

// Phase-04
connectionRequestSchema.index({
  toUserId: 1,
  status: 1,
});
```

Remember:

- The first index enforces uniqueness for active documents with the same ordered pair.
    
- The second index helps recipient-side filtering.
    
- Neither index eliminates the need for correct controller logic.
    
- Indexes should be evaluated against real query patterns as the application grows.
    

---

# PART I — FINAL REVISION

# 44. Phase-04 in one diagram

```
                     PHASE 04
                         |
           +-------------+-------------+
           |             |             |
           v             v             v
         FEED         RECEIVED      CONNECTIONS
           |             |             |
           v             v             v
        getFeed()   getReceived() getConnections()
           |             |             |
           v             v             v
    Find active     Find incoming  Find accepted
    relationships   interested     relationships
           |          requests       in either
           |             |           direction
           v             v             v
    Build exclusion Populate sender Populate both
    Set             with public     with public
           |        fields          fields
           v             |             |
       $nin filter        |             v
           |             |       map() → other user
           v             |             |
      Pagination          |             |
           |             |             |
           +-------------+-------------+
                         |
                         v
                    API RESPONSES
```

# 45. Concepts you should understand after Phase-04

|Concept|Why it matters|
|---|---|
|`query()`|Validates URL query parameters|
|`.optional()`|Allows omitted query parameters|
|`.isInt()`|Validates integer inputs|
|Pagination|Divides large results into pages|
|`parseInt()`|Converts query strings into integers|
|`Math.max()` / `Math.min()`|Enforces pagination boundaries|
|`skip()`|Skips previous records|
|`limit()`|Restricts returned records|
|`$in`|Matches any value from an array|
|`$or`|Matches at least one condition|
|`$nin`|Excludes values from an array|
|`Set`|Stores unique identifiers|
|`Array.from()`|Converts a Set into an array|
|`select()`|Restricts returned fields|
|`Promise.all()`|Runs independent promises concurrently|
|`countDocuments()`|Counts matching documents|
|`sort()`|Establishes result ordering|
|`populate()`|Resolves MongoDB references|
|`map()`|Transforms arrays|
|`.equals()`|Compares ObjectId values|
|Compound index|Helps queries using multiple fields|

# 46. What changed from Phase-03 to Phase-04?

|Phase-03|Phase-04|
|---|---|
|Create connection requests|Discover users|
|Review requests|Display incoming requests|
|Accept/reject requests|Display accepted connections|
|Enforce relationship rules|Filter based on relationship rules|
|Validate and create/update documents|Retrieve, filter and transform documents|

# 47. Questions you should be able to answer

Before moving forward, make sure you can explain these:

1. Why do we exclude the logged-in user from the feed?
    
2. Why do we check active requests in both directions?
    
3. Why are rejected and ignored users allowed to reappear?
    
4. What is the difference between `$in` and `$nin`?
    
5. What does `$or` do in MongoDB?
    
6. Why do we use a JavaScript `Set`?
    
7. Why do we convert ObjectIds into strings before adding them to a Set?
    
8. What is pagination, and why do we need it?
    
9. What is the difference between `skip()` and `limit()`?
    
10. Why do we use `Promise.all()`?
    
11. Why do we need `countDocuments()`?
    
12. Why is a stable sort important for pagination?
    
13. What is the purpose of `PUBLIC_USER_FIELDS`?
    
14. What is the difference between `select()` and `populate()`?
    
15. Why do we populate only `fromUserId` for received requests?
    
16. Why do we populate both user references for connections?
    
17. Why do we use `$or` when finding accepted connections?
    
18. Why do we use `map()` in `getConnections()`?
    
19. Why do we include `requestId` in the connections response?
    
20. Why do we need another compound index on `toUserId` and `status`?
    

---

# 48. The final mental model

Think of Phase-04 as three different ways of viewing the same user and relationship data.

**Feed:**

> Show me people I can potentially connect with.

**Received Requests:**

> Show me people who have requested to connect with me and are waiting for my decision.

**Connections:**

> Show me people whose connection requests have been accepted.

The underlying collections remain the same:

```
                  USER COLLECTION
                         |
                         |
                         v
              CONNECTION REQUEST COLLECTION
                         |
            +------------+------------+
            |            |            |
            v            v            v
           FEED       RECEIVED    CONNECTIONS
            |            |            |
            v            v            v
         Discover     Respond      Interact
         potential    to pending   with accepted
         connections  requests     connections
```

**The most important architectural lesson from Phase-04:**

> We store relationships in the ConnectionRequest collection, retrieve the relevant relationships using MongoDB queries, resolve user references with `populate()`, filter out unwanted users, and transform the results into frontend-friendly API responses.

That is the foundation of a relationship-driven application like DevTinder.

---

**Phase-04 completion checklist**

- Understand the feed exclusion logic.
    
- Understand `$in`, `$or`, and `$nin`.
    
- Understand how the exclusion Set is constructed.
    
- Understand pagination and its calculations.
    
- Understand field projection and public-field protection.
    
- Understand `Promise.all()` and `countDocuments()`.
    
- Understand `populate()` and ObjectId references.
    
- Understand how received requests are fetched.
    
- Understand how connections are found in both directions.
    
- Understand the `map()` transformation.
    
- Add the recipient-side compound index.
    
- Test all three endpoints, including their edge cases.
    

**End of Phase-04 learning notes.**