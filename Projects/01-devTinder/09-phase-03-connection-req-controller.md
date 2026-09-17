
---

# DevTinder Phase-03 — Step-03

# Connection Request Controllers

## 1. What is the purpose of Step-03?

We now have:

```text
STEP-01
ConnectionRequest Model
        ↓
Defines the data and database rules


STEP-02
Validators
        ↓
Checks whether request input is valid


STEP-03
Controllers
        ↓
Checks whether the operation is allowed
and performs the database operation
```

So the controller is the layer that contains the **business logic**.

For connection requests, we have two controller functions:

```text
sendRequest()
reviewRequest()
```

They handle:

```text
POST /request/send/:status/:toUserId

POST /request/review/:status/:requestId
```

---

# 2. The overall architecture

The complete request flow is:

```text
                 CLIENT
                    │
                    ↓
                  ROUTE
                    │
                    ↓
          Authentication Middleware
                    │
                    ↓
               Validators
                    │
                    ↓
              validate()
                    │
                    ↓
              CONTROLLER
                    │
            Business Logic
                    │
                    ↓
                MODEL
                    │
                    ↓
                MONGODB
```

Each layer has a different responsibility.

```text
Validator
"What did the client send?"

Controller
"Is this action allowed?"

Model / Database
"Can this data be stored consistently?"
```

---

# 3. Imports

Your controller starts with:

```js
import userModel from "../models/user.model.js";
import connectionRequestModel from "../models/connectionRequest.model.js";

import ApiError from "../utils/ApiError.util.js";
```

## `userModel`

Used to find the target user.

```js
const toUser = await userModel.findById(toUserId);
```

We need this because a connection request should only be created for a real user.

---

## `connectionRequestModel`

Used to:

```text
find existing requests
create requests
find requests
update requests
save requests
```

---

## `ApiError`

Used when a business rule is violated.

Example:

```js
throw new ApiError(404, "User not found");
```

Your centralized error middleware will eventually turn this into the API response.

---

# 4. `ACTIVE_STATUSES`

```js
const ACTIVE_STATUSES = ["interested", "accepted"];
```

This is simply a reusable array.

It represents:

> Which connection-request states count as an active relationship?

Your project's rule is:

```text
interested → ACTIVE
accepted   → ACTIVE

rejected   → NOT ACTIVE
ignored    → NOT ACTIVE
```

So instead of repeatedly writing:

```js
status: {
  $in: ["interested", "accepted"]
}
```

you define:

```js
const ACTIVE_STATUSES = ["interested", "accepted"];
```

and use:

```js
status: {
  $in: ACTIVE_STATUSES
}
```

This improves readability and reduces duplication.

---

# 5. Why is `interested` active?

Because the request is still pending.

Example:

```text
A ─────────→ B
   interested
```

There is currently an active request between them.

So A should not create another active request to B.

---

# 6. Why is `accepted` active?

Because the users are now connected.

```text
A ↔ B
   accepted
```

The relationship still exists.

Therefore another connection request should not be created.

---

# 7. Why are `rejected` and `ignored` not active?

Because those states represent a finished request.

Example:

```text
A → B
rejected
```

Later:

```text
A → B
interested
```

This new request is allowed.

The old rejected request remains as history.

---

# 8. `sendRequest()` — what does it do?

```js
const sendRequest = async (req, res, next) => {
```

This controller handles:

```text
POST /request/send/:status/:toUserId
```

Its job is:

```text
1. Identify the sender
2. Read status and target user
3. Prevent self-request
4. Check target user exists
5. Check active relationship does not exist
6. Create the request
7. Return success
```

---

# 9. First line — identify the sender

```js
const fromUserId = req.user._id;
```

This comes from your authentication middleware.

Remember the flow:

```text
Access Token
    ↓
authenticate middleware
    ↓
User identified
    ↓
req.user
```

So:

```js
req.user._id
```

is the logged-in user's MongoDB ID.

The important security principle here is:

> **The sender is taken from the authenticated user, not from the URL.**

The client does NOT send:

```text
fromUserId
```

This is good design.

You don't want someone saying:

```json
{
  "fromUserId": "someOtherUser"
}
```

and pretending to be that user.

---

# 10. Read route parameters

```js
const { status, toUserId } = req.params;
```

For:

```text
POST /request/send/interested/650abc...
```

Express gives:

```js
req.params = {
  status: "interested",
  toUserId: "650abc..."
};
```

Destructuring:

```js
const { status, toUserId } = req.params;
```

is simply shorthand for:

```js
const status = req.params.status;
const toUserId = req.params.toUserId;
```

---

# 11. Step 1 — prevent self-request

```js
if (fromUserId.toString() === toUserId) {
  throw new ApiError(
    400,
    "You cannot send a connection request to yourself",
  );
}
```

The rule is:

```text
FROM ≠ TO
```

Invalid:

```text
A → A
```

Valid:

```text
A → B
```

---

# 12. Why use `.toString()`?

`fromUserId` is usually a Mongoose `ObjectId`.

`toUserId` comes from a URL, so it is a string.

Conceptually:

```text
fromUserId = ObjectId("650...")
toUserId   = "650..."
```

Comparing them directly can be problematic because their JavaScript types differ.

So:

```js
fromUserId.toString()
```

produces:

```text
"650..."
```

and now:

```js
fromUserId.toString() === toUserId
```

compares two strings.

---

# 13. Why do this check in both controller and model?

You already have a model-level guard:

```js
pre("save")
```

which prevents:

```text
A → A
```

So why check again here?

Because the controller can reject the request:

```text
BEFORE
```

doing any database lookup.

Benefits:

```text
Controller
   ↓
400 immediately
   ↓
No unnecessary DB query
```

The model check remains a second safety layer.

This is a good example of:

```text
Controller
+
Model
```

both protecting an invariant, but at different levels.

---

# 14. Step 2 — confirm target user exists

```js
const toUser = await userModel.findById(toUserId);
```

This asks MongoDB:

> Does a User document with this `_id` exist?

Possible result:

```text
User exists
   ↓
document returned
```

or:

```text
User does not exist
   ↓
null
```

Then:

```js
if (!toUser) {
  throw new ApiError(404, "User not found");
}
```

---

# 15. Why do this if the ObjectId was already validated?

This is a very important distinction.

Step-02 checked:

```text
"Is this a valid ObjectId?"
```

Step-03 checks:

```text
"Does a user with this ObjectId actually exist?"
```

These are different questions.

Example:

```text
650abc1234567890abcdef12
```

could be a valid ObjectId.

But there may be no user with that ID.

So:

```text
Validator
   ↓
Valid ObjectId ✅

Controller
   ↓
User exists? ❌
```

Result:

```text
404 User not found
```

---

# 16. `findById()`

This:

```js
userModel.findById(toUserId)
```

is essentially asking for:

```js
{
  _id: toUserId
}
```

It is equivalent in concept to:

```js
userModel.findOne({
  _id: toUserId,
});
```

---

# 17. Step 3 — check existing active relationship

This is the most important query in `sendRequest()`.

```js
const existingActiveRequest =
  await connectionRequestModel.findOne({
    status: { $in: ACTIVE_STATUSES },

    $or: [
      { fromUserId, toUserId },
      { fromUserId: toUserId, toUserId: fromUserId },
    ],
  });
```

Let's break it down piece by piece.

---

# 18. `findOne()`

```js
connectionRequestModel.findOne(...)
```

means:

> Find one document matching these conditions.

We don't need every matching request.

We only need to know:

```text
Does an active relationship already exist?
```

So one matching document is enough.

---

# 19. `$in`

You have:

```js
status: {
  $in: ACTIVE_STATUSES,
}
```

and:

```js
ACTIVE_STATUSES = ["interested", "accepted"];
```

So this becomes conceptually:

```js
status: {
  $in: ["interested", "accepted"],
}
```

Meaning:

> Status must be either `interested` or `accepted`.

So these count:

```text
interested ✅
accepted   ✅
```

These do not:

```text
rejected ❌
ignored  ❌
```

---

# 20. `$or`

Now the interesting part:

```js
$or: [
  { fromUserId, toUserId },
  { fromUserId: toUserId, toUserId: fromUserId },
]
```

`$or` means:

> At least one of these conditions must be true.

---

# 21. First condition

```js
{ fromUserId, toUserId }
```

Because of JavaScript object shorthand, this means:

```js
{
  fromUserId: fromUserId,
  toUserId: toUserId,
}
```

Suppose:

```text
logged-in user = A
target user    = B
```

Then this checks:

```text
A → B
```

---

# 22. Second condition

```js
{
  fromUserId: toUserId,
  toUserId: fromUserId
}
```

This reverses them.

It checks:

```text
B → A
```

So the complete query checks:

```text
A → B
OR
B → A
```

---

# 23. Why both directions?

Because a connection relationship is effectively between two users.

You don't want:

```text
A → B interested
```

and then:

```text
B → A interested
```

at the same time.

Your application rule is:

```text
Only one active relationship between A and B.
```

regardless of direction.

Therefore the controller checks both.

---

# 24. Complete meaning of the query

This:

```js
await connectionRequestModel.findOne({
  status: { $in: ACTIVE_STATUSES },
  $or: [
    { fromUserId, toUserId },
    { fromUserId: toUserId, toUserId: fromUserId },
  ],
});
```

means:

> Find whether there is any active connection request where these two users are connected in either direction.

Visual:

```text
             A and B
                │
        ┌───────┴───────┐
        ↓               ↓
      A → B           B → A
        │               │
        └───────┬───────┘
                ↓
        status = interested
                OR
        status = accepted
```

If one exists:

```text
existingActiveRequest !== null
```

---

# 25. `if (existingActiveRequest)`

```js
if (existingActiveRequest) {
  throw new ApiError(
    409,
    "A connection request already exists between you and this user",
  );
}
```

If MongoDB found a matching active request:

```text
STOP
```

and return:

```text
409 Conflict
```

---

# 26. Why HTTP 409?

`409 Conflict` is appropriate when:

> The request conflicts with the current state of the resource.

Example:

```text
You already have an active relationship.
```

The input may be structurally valid, but the operation conflicts with the current state.

Remember:

```text
400 → input/request is invalid
404 → resource doesn't exist
403 → authenticated user isn't allowed
409 → operation conflicts with current state
```

---

# 27. Important: validator vs 409

Suppose:

```text
POST /request/send/interested/invalid
```

Invalid ObjectId.

That is:

```text
400
```

But:

```text
POST /request/send/interested/validUserId
```

where an active relationship already exists is:

```text
409
```

So:

```text
VALIDATION ERROR
     ↓
400

BUSINESS STATE CONFLICT
     ↓
409
```

---

# 28. Step 4 — create the request

```js
const connectionRequest =
  await connectionRequestModel.create({
    fromUserId,
    toUserId,
    status,
  });
```

This creates a new MongoDB document.

Example:

```js
{
  fromUserId: A,
  toUserId: B,
  status: "interested"
}
```

Mongoose then validates/saves it.

Because you are using:

```js
connectionRequestModel.create(...)
```

Mongoose performs the document creation and save process, including the model's relevant validation/save middleware.

Therefore your earlier:

```js
pre("save")
```

guard also matters here.

---

# 29. What does the database finally contain?

Example:

```js
{
  _id: "...",

  fromUserId: "A",
  toUserId: "B",

  status: "interested",

  createdAt: "...",
  updatedAt: "..."
}
```

The `createdAt` and `updatedAt` come from:

```js
timestamps: true
```

in Step-01.

---

# 30. Step 5 — success response

```js
return res.status(201).json({
  status: "success",
  message: `Connection request ${status === "interested" ? "sent" : "recorded"} successfully`,
  data: { connectionRequest },
});
```

---

# 31. Why 201?

HTTP `201 Created` means:

> A new resource was successfully created.

That is exactly what happened.

For a new `ConnectionRequest`:

```text
POST
 ↓
new document created
 ↓
201 Created
```

---

# 32. Why `"sent"` vs `"recorded"`?

Your status can be:

```text
interested
ignored
```

For:

```text
interested
```

the message becomes:

```text
Connection request sent successfully
```

For:

```text
ignored
```

the message becomes:

```text
Connection request recorded successfully
```

The ternary:

```js
status === "interested"
  ? "sent"
  : "recorded"
```

means:

```text
if status === interested
    use "sent"
else
    use "recorded"
```

---

# 33. Understanding the `catch`

```js
} catch (error) {
```

Everything inside the `try` can fail.

Examples:

```text
database failure
ApiError thrown
duplicate index
MongoDB error
unexpected error
```

The `catch` receives the error.

---

# 34. The `11000` error

You have:

```js
if (error.code === 11000) {
```

MongoDB commonly uses error code:

```text
11000
```

for duplicate-key violations.

In your project this can happen when the unique index rejects an active duplicate for the same ordered pair:

```text
A → B interested
```

already exists, and another:

```text
A → B interested
```

tries to be inserted.

The database says:

```text
duplicate key
```

---

# 35. Why catch `11000` separately?

Without this:

```text
MongoDB duplicate error
       ↓
generic server error
       ↓
possibly 500
```

But this condition is actually a known business conflict.

So you convert it to:

```text
409 Conflict
```

using:

```js
return next(
  new ApiError(
    409,
    "A connection request already exists between you and this user",
  ),
);
```

Now both situations produce the same API behavior:

```text
Situation 1
Controller detects existing request
       ↓
409

Situation 2
DB unique index detects duplicate
       ↓
409
```

This gives the client a consistent result.

---

# 36. Very important concurrency concept: race condition

Your code is trying to protect against this:

```text
Request A                    Request B
    │                            │
    ↓                            ↓
Check database              Check database
    │                            │
    ↓                            ↓
No active request           No active request
    │                            │
    └────────────┬───────────────┘
                 ↓
              Both try
               to save
```

If both requests are for:

```text
A → B
```

the partial unique index can reject one of them.

So:

```text
Application check
        +
Database unique index
```

is stronger than only using the application check.

---

# 37. IMPORTANT accuracy note about the race-condition comment

Your code comments say the unique index is the safety net for the overall rule:

> "no ACTIVE relationship in either direction."

That is **not completely true**.

The index is:

```text
(fromUserId, toUserId)
```

and therefore it sees:

```text
A → B
```

and:

```text
B → A
```

as different keys.

So consider a concurrent case:

```text
Request 1                 Request 2
A → B interested          B → A interested
       │                         │
       ↓                         ↓
  check passes             check passes
       │                         │
       ↓                         ↓
  insert A → B             insert B → A
```

The partial unique index does NOT by itself prevent this because:

```text
(A,B) ≠ (B,A)
```

Therefore:

> The application-level `$or` check enforces the two-direction business rule during normal sequential execution, while the unique index provides a DB-level guarantee for duplicate active documents in the same direction.

This distinction is important to understand correctly.

---

# 38. If we truly need DB-level protection for both directions

This is an advanced design issue.

If the requirement is:

```text
A ↔ B can never have two active relationships
even under simultaneous opposite-direction requests
```

then the database needs some **direction-independent uniqueness key** or a transaction/locking strategy.

One common design is to store a canonical pair:

```text
userLow
userHigh
```

where:

```text
userLow  = smaller ObjectId
userHigh = larger ObjectId
```

Then:

```text
A → B
B → A
```

both map to:

```text
(userLow = A, userHigh = B)
```

and a unique index can enforce one active pair.

You do NOT need to redesign Step-03 right now; just understand the concurrency limitation.

---

# 39. `next(error)`

At the end:

```js
next(error);
```

means:

> Pass this error to Express's error-handling middleware.

Your centralized error handler from Phase-01 can then format the response.

Flow:

```text
Controller
   ↓
error occurs
   ↓
catch(error)
   ↓
next(error)
   ↓
errorHandler middleware
   ↓
HTTP response
```

This keeps controllers from repeating:

```js
res.status(...).json(...)
```

for every unexpected error.

---

# 40. Now understand `reviewRequest()`

The second controller:

```js
const reviewRequest = async (req, res, next) => {
```

handles:

```text
POST /request/review/:status/:requestId
```

Its job is:

```text
1. Identify logged-in user
2. Read status and request ID
3. Find the connection request
4. Check logged-in user is the recipient
5. Check request is still interested
6. Change status
7. Save
8. Return response
```

---

# 41. Get the logged-in user

```js
const loggedInUserId = req.user._id;
```

Again:

```text
Authentication middleware
       ↓
req.user
       ↓
req.user._id
```

This is the currently authenticated user.

---

# 42. Read route parameters

```js
const { status, requestId } = req.params;
```

For:

```text
POST /request/review/accepted/650abc...
```

we get:

```js
status = "accepted";
requestId = "650abc...";
```

The validator already guarantees:

```text
status = accepted/rejected
requestId = valid ObjectId format
```

Now the controller performs the actual business checks.

---

# 43. Step 1 — load the request

```js
const connectionRequest =
  await connectionRequestModel.findById(requestId);
```

This asks:

> Does a connection-request document with this ID exist?

If not:

```js
if (!connectionRequest) {
  throw new ApiError(
    404,
    "Connection request not found",
  );
}
```

Result:

```text
404 Not Found
```

---

# 44. Why can't the validator check this?

Because the validator only knows:

```text
"Is this a valid ObjectId?"
```

It doesn't normally perform the database business operation of:

```text
"Does this request document exist?"
```

That is controller/service responsibility.

Again:

```text
VALID ObjectId
      ≠
EXISTING document
```

---

# 45. Step 2 — ownership / authorization check

This is one of the most important pieces:

```js
if (
  connectionRequest.toUserId.toString() !==
  loggedInUserId.toString()
) {
  throw new ApiError(
    403,
    "You are not authorized to review this connection request",
  );
}
```

Remember the document:

```text
A → B
```

means:

```text
fromUserId = A
toUserId   = B
```

Who should review it?

```text
B
```

The recipient.

Not:

```text
A ❌ sender
C ❌ random user
```

---

# 46. Visualizing authorization

Suppose:

```text
A ─────────→ B
   request
```

The review endpoint must allow:

```text
B → review
```

but reject:

```text
A → review ❌
C → review ❌
```

Therefore:

```text
loggedInUserId
       │
       ↓
must equal
       │
       ↓
connectionRequest.toUserId
```

---

# 47. Why HTTP 403?

`403 Forbidden` means:

> The server understood the request, but the authenticated user is not allowed to perform that action.

Here:

```text
Request exists ✅
Request ID valid ✅
But user isn't the recipient ❌
```

Therefore:

```text
403
```

---

# 48. Authentication vs authorization

This controller demonstrates the difference perfectly.

### Authentication

```text
Who are you?
```

Your auth middleware answers this:

```text
req.user
```

### Authorization

```text
Are you allowed to review this particular request?
```

The controller answers this:

```js
connectionRequest.toUserId === req.user._id
```

So:

```text
Authentication
      ↓
Identity

Authorization
      ↓
Permission
```

---

# 49. Step 3 — only `interested` can be reviewed

```js
if (connectionRequest.status !== "interested") {
  throw new ApiError(
    400,
    `This request has already been reviewed (current status: ${connectionRequest.status})`,
  );
}
```

This implements your state transition rule.

A request can move:

```text
interested
    │
    ├── accepted
    │
    └── rejected
```

But it cannot move:

```text
accepted → rejected ❌
accepted → accepted ❌
rejected → accepted ❌
rejected → rejected ❌
ignored → accepted ❌
```

---

# 50. Why does the controller check the current status?

Because the validator only checks the new requested status.

For example:

```text
POST /request/review/accepted/:requestId
```

passes validation because:

```text
accepted ✅
valid ObjectId ✅
```

But the database could currently contain:

```js
status: "rejected"
```

Therefore the controller must inspect:

```js
connectionRequest.status
```

and enforce:

```text
current state must be "interested"
```

---

# 51. State machine

This is the cleanest way to remember the feature:

```text
                  ┌──────────────┐
                  │  interested  │
                  └──────┬───────┘
                         │
                  recipient reviews
                    ┌────┴────┐
                    ↓         ↓
               accepted    rejected
```

Sender may create:

```text
send/interested
send/ignored
```

So:

```text
SEND OPERATION
      │
      ├── interested
      └── ignored

REVIEW OPERATION
      │
      ├── accepted
      └── rejected
```

---

# 52. Step 4 — update the status

```js
connectionRequest.status = status;
```

Suppose current request is:

```js
{
  status: "interested"
}
```

and URL says:

```text
/review/accepted/:requestId
```

Then:

```js
status = "accepted";
```

and the document becomes:

```js
{
  status: "accepted"
}
```

---

# 53. Save the document

```js
await connectionRequest.save();
```

Now the change is persisted to MongoDB.

Because this is `.save()`, your Mongoose save middleware can run as well.

Also:

```text
updatedAt
```

is updated automatically because of:

```js
timestamps: true
```

---

# 54. Success response for review

```js
return res.status(200).json({
  status: "success",
  message: `Connection request ${status} successfully`,
  data: { connectionRequest },
});
```

Example:

```json
{
  "status": "success",
  "message": "Connection request accepted successfully",
  "data": {
    "connectionRequest": {
      "_id": "...",
      "fromUserId": "...",
      "toUserId": "...",
      "status": "accepted"
    }
  }
}
```

Why `200`?

Because we are updating an existing resource, not creating a new one.

```text
CREATE → 201
UPDATE → 200
```

---

# 55. Review controller error handling

```js
} catch (error) {
  next(error);
}
```

Unlike `sendRequest`, there is no special `11000` handling here because this operation changes an existing request's status and normally isn't creating a duplicate indexed document.

The error goes to:

```text
errorHandler
```

---

# 56. Complete SEND flow

Here is the full flow.

```text
        POST /request/send/:status/:toUserId
                         │
                         ↓
                  Authentication
                         │
                         ↓
                   Validation
                         │
               ┌─────────┴─────────┐
               │                   │
              FAIL                PASS
               │                   │
              400                  ↓
                            sendRequest()
                                  │
                    ┌─────────────┴─────────────┐
                    ↓                           ↓
              Self request?                Target exists?
                    │                           │
                 YES → 400                  NO → 404
                    │
                   PASS
                    │
                    ↓
           Active relation exists?
                    │
              ┌─────┴─────┐
             YES          NO
              │            │
            409            ↓
                       Create request
                            │
                            ↓
                    Database / Index
                            │
                      ┌─────┴─────┐
                     OK          11000
                      │             │
                    201             409
```

---

# 57. Complete REVIEW flow

```text
       POST /request/review/:status/:requestId
                         │
                         ↓
                  Authentication
                         │
                         ↓
                   Validation
                         │
                         ↓
                 reviewRequest()
                         │
                         ↓
               Find connection request
                         │
                    ┌────┴────┐
                   NO        YES
                   │           │
                 404           ↓
                         Is logged-in user
                         the recipient?
                              │
                       ┌──────┴──────┐
                      NO             YES
                       │              │
                     403              ↓
                           Is status "interested"?
                                  │
                            ┌─────┴─────┐
                           NO           YES
                           │              │
                         400              ↓
                               Update status
                                    │
                                    ↓
                                   save()
                                    │
                                    ↓
                                   200
```

---

# 58. Complete feature architecture

Put the three steps together:

```text
                    CONNECTION REQUEST
                           │
            ┌──────────────┴──────────────┐
            │                             │
           SEND                          REVIEW
            │                             │
            ↓                             ↓
 /request/send/:status/:toUserId   /request/review/:status/:requestId
            │                             │
            ↓                             ↓
        Validators                    Validators
            │                             │
            ↓                             ↓
        Controller                    Controller
            │                             │
      ┌─────┴─────┐                ┌─────┴─────┐
      ↓           ↓                ↓           ↓
   Business     Create          Ownership    State check
   checks       document        check
      │                             │
      └─────────────┬───────────────┘
                    ↓
              ConnectionRequest
                    Model
                    ↓
                 MongoDB
```

---

# 59. What exactly is the controller responsible for?

For `sendRequest()`:

```text
1. Identify sender
2. Prevent self-request
3. Verify target user exists
4. Check active relationship
5. Create request
6. Handle duplicate-key race for same direction
7. Send response
```

For `reviewRequest()`:

```text
1. Identify reviewer
2. Find request
3. Verify reviewer is recipient
4. Verify request is still interested
5. Change status
6. Save
7. Send response
```

---

# 60. Why don't we put all this logic in the validator?

Because these are not merely input-validation rules.

For example:

```text
"Is requestId a valid ObjectId?"
```

is validation.

But:

```text
"Does request #123 actually exist?"
```

requires the database.

And:

```text
"Does this request belong to the logged-in user?"
```

is authorization.

And:

```text
"Is the request still interested?"
```

is business logic.

So the controller is the right place for this stage of your project.

---

# 61. Error status map for this feature

Memorize this table.

|Situation|Status|
|---|--:|
|Invalid status / invalid ObjectId|`400`|
|Sending request to yourself|`400`|
|Reviewing a non-interested request|`400`|
|User does not exist|`404`|
|Connection request does not exist|`404`|
|User is not recipient|`403`|
|Active relationship already exists|`409`|
|Duplicate-key from unique index|`409`|
|Successful send/create|`201`|
|Successful review/update|`200`|

---

# 62. One subtle point about `ignored`

This controller allows:

```text
POST /request/send/ignored/:toUserId
```

and creates a document:

```js
{
  fromUserId: A,
  toUserId: B,
  status: "ignored"
}
```

Because:

```text
ignored
```

is not in:

```js
ACTIVE_STATUSES
```

it does not block a future active request.

Example:

```text
A → B ignored

later

A → B interested
```

Both documents can exist.

That matches the design from Step-01.

---

# 63. Another subtle point: `findOne()` doesn't mean only one request exists

This:

```js
findOne(...)
```

only means:

> Give me one matching document.

It does NOT mean:

> The database contains only one matching document.

The partial unique index is what controls duplicate active documents for the same ordered pair.

So:

```text
findOne()
```

is a query operation.

```text
unique index
```

is a database integrity rule.

---

# 64. Why the controller query checks both directions but the index doesn't

Controller:

```text
A → B
OR
B → A
```

because business rule is relationship-oriented.

Index:

```text
A → B
```

because compound index is based on ordered fields:

```text
fromUserId
toUserId
```

Therefore:

```text
Controller
"Does ANY active relationship exist between us?"

Index
"Does this exact active ordered pair already exist?"
```

This distinction is extremely important.

---

# 65. An advanced issue in `reviewRequest()`

There is another concurrency concern worth understanding.

Current code does:

```text
find request
    ↓
check status === interested
    ↓
change status
    ↓
save
```

Imagine two review requests arrive almost simultaneously:

```text
Request 1: accept
Request 2: reject
```

Both could potentially read:

```text
status = interested
```

before either update is saved.

Then:

```text
Request 1 → accepted
Request 2 → rejected
```

The final result could depend on which save happens last.

For strict concurrency-safe state transitions, an atomic update is generally safer:

```js
findOneAndUpdate(
  {
    _id: requestId,
    toUserId: loggedInUserId,
    status: "interested",
  },
  {
    $set: { status },
  },
  {
    new: true,
  },
)
```

That makes the transition condition part of the database operation.

You do not need to rewrite your current code immediately for learning Step-03, but you should understand this principle:

> **A check followed by a separate write can have a race window. Atomic database operations can close that window.**

---

# 66. Why `try/catch` is used around async controller code

Your controller contains:

```js
await userModel.findById(...)
await connectionRequestModel.findOne(...)
await connectionRequestModel.create(...)
```

Any of these asynchronous operations can reject.

So:

```js
try {
   ...
} catch (error) {
   next(error);
}
```

captures the error.

Flow:

```text
await database operation
          │
      error?
       /   \
     NO     YES
     │       │
 continue   catch
              │
              ↓
          next(error)
```

---

# 67. Why use `throw new ApiError(...)`?

Example:

```js
throw new ApiError(404, "User not found");
```

The controller is saying:

> Stop executing this operation and pass this known application error to the error handler.

It prevents code below it from continuing.

For example:

```js
if (!toUser) {
  throw new ApiError(404, "User not found");
}

connectionRequestModel.create(...)
```

The create operation will never execute when the user doesn't exist.

---

# 68. The core business rules implemented here

Your controller currently enforces these rules:

```text
RULE 1
A user cannot send a request to themselves.

RULE 2
The target user must exist.

RULE 3
There cannot already be an active relationship
between these users in either direction
during the normal application check.

RULE 4
Only the recipient can review a request.

RULE 5
Only an "interested" request can be reviewed.

RULE 6
Review changes "interested" to:
accepted OR rejected.

RULE 7
Rejected/ignored history remains and does not
block a later request.
```

---

# 69. Business-rule flow for SEND

```text
                SEND REQUEST
                     │
                     ↓
             Who is the sender?
                     │
                req.user._id
                     │
                     ↓
               Who is target?
                     │
                req.params
                     │
                     ↓
               sender == target?
                  /        \
                YES         NO
                 │           │
                400          ↓
                        target exists?
                          /       \
                        NO         YES
                        │           │
                       404          ↓
                           active relation?
                             /       \
                           YES        NO
                            │          │
                           409         ↓
                                CREATE REQUEST
                                     │
                                     ↓
                                    201
```

---

# 70. Business-rule flow for REVIEW

```text
                 REVIEW REQUEST
                       │
                       ↓
                 Find request
                       │
                 ┌─────┴─────┐
                 ↓           ↓
               not found    found
                 │           │
                404          ↓
                     Is current user
                     the recipient?
                         /      \
                       NO        YES
                       │          │
                      403         ↓
                       current status?
                            /          \
                       not interested   interested
                           │               │
                          400              ↓
                                   set new status
                                          │
                                          ↓
                                        save
                                          │
                                          ↓
                                         200
```

---

# 71. The most important code concepts to learn from this step

## `req.user`

Comes from authentication middleware.

```js
req.user._id
```

means:

```text
currently authenticated user's ID
```

---

## `req.params`

Contains dynamic URL values.

```js
const { status, toUserId } = req.params;
```

---

## `findById()`

Find a document by `_id`.

```js
User.findById(id)
```

---

## `findOne()`

Find one document matching conditions.

```js
Model.findOne({
  ...
})
```

---

## `$in`

Match one value from a list.

```js
status: {
  $in: ["interested", "accepted"]
}
```

---

## `$or`

At least one condition must match.

```js
$or: [
  condition1,
  condition2,
]
```

---

## `create()`

Create and save a new MongoDB document.

```js
Model.create({...})
```

---

## `.save()`

Persist changes made to an existing Mongoose document.

```js
document.status = "accepted";
await document.save();
```

---

## `throw new ApiError()`

Stop normal execution and pass a known application error to the error pipeline.

---

## `next(error)`

Pass an error to centralized Express error handling.

---

## HTTP `201`

New resource created.

---

## HTTP `200`

Existing resource successfully updated/retrieved.

---

## HTTP `400`

Invalid request/input/business-state input.

---

## HTTP `403`

Authenticated user is not permitted.

---

## HTTP `404`

Requested resource doesn't exist.

---

## HTTP `409`

Operation conflicts with existing/current resource state.

---

## Mongo error `11000`

Duplicate-key/index violation.

---

# 72. How Step-01, Step-02 and Step-03 fit together

This is the big picture you should have in your head:

```text
                    CLIENT
                       │
                       ↓
                     ROUTE
                       │
                       ↓
             AUTHENTICATION
                       │
                 "Who are you?"
                       │
                       ↓
                  VALIDATOR
                       │
               "Is input valid?"
                       │
                       ↓
                 CONTROLLER
                       │
            "Is action allowed?"
                       │
                       ↓
                    MODEL
                       │
              "Is data valid?"
                       │
                       ↓
                  DATABASE
                       │
              "Maintain integrity"
```

And specifically:

```text
STEP 01
Model
├── fields
├── enum
├── timestamps
├── pre-save self-request guard
└── partial unique index

STEP 02
Validators
├── send status
├── review status
├── toUserId format
└── requestId format

STEP 03
Controllers
├── user existence
├── authorization
├── state transitions
├── active relationship check
├── create/update
└── API responses/errors
```

---

# 73. Final mental model for DevTinder

Think of a connection request as:

```text
               CONNECTION REQUEST
                       │
          ┌────────────┴────────────┐
          ↓                         ↓
        SEND                      REVIEW
          │                         │
          ↓                         ↓
     interested                accepted
     ignored                   rejected
          │                         │
          └────────────┬────────────┘
                       ↓
                  DATABASE
```

And think of the code layers as:

```text
┌─────────────────────────────────────┐
│ VALIDATOR                           │
│ "Is the input valid?"               │
├─────────────────────────────────────┤
│ CONTROLLER                          │
│ "Is this operation allowed?"        │
├─────────────────────────────────────┤
│ MODEL                               │
│ "Is the document structurally OK?" │
├─────────────────────────────────────┤
│ DATABASE / INDEX                    │
│ "Protect data integrity"            │
└─────────────────────────────────────┘
```

# 74. The 10 things you should be able to explain yourself

Before moving on, make sure you can explain these without looking at the code:

```text
1. Why does sendRequest use req.user._id?

2. Why isn't fromUserId taken from req.params?

3. Why do we check whether toUser exists?

4. What does $in mean?

5. What does $or mean?

6. Why does the active-request query check both A → B and B → A?

7. Why do we return 409 when an active relationship exists?

8. Why can only toUserId review the request?

9. Why must the current status be "interested" before review?

10. Why do we still need the unique database index if the
    controller already checks for an existing request?
```

The strongest conceptual answer to all of this is:

> **Step-02 protects the controller from invalid input. Step-03 protects the application's business rules. Step-01 and the database protect the stored data.**