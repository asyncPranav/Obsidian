
---

# DevTinder Phase-03 — Step-02

# Connection Request Validators

## 1. What are we building in Step-02?

In Step-01, we created the `ConnectionRequest` model.

It defines what a connection-request document can look like:

```js
{
  fromUserId,
  toUserId,
  status
}
```

But there are two different API endpoints:

```text
POST /request/send/:status/:toUserId

POST /request/review/:status/:requestId
```

They accept different values.

### Send endpoint

The sender can choose:

```text
interested
ignored
```

### Review endpoint

The receiver can choose:

```text
accepted
rejected
```

Therefore we need validators that make sure the URL parameters contain the correct values.

---

# 2. What is request validation?

Request validation means:

> Check incoming data before it reaches the controller/business logic.

Example:

```text
Client
  ↓
POST /request/send/interested/64f...
  ↓
Validator
  ↓
Controller
  ↓
Database
```

If the input is invalid:

```text
Client
  ↓
Validator
  ↓
❌ 400 Bad Request
```

The controller does not need to deal with obviously invalid input.

---

# 3. Why do we need validation?

Suppose someone sends:

```text
POST /request/send/hello/abc
```

Problems:

```text
status = hello
toUserId = abc
```

`hello` is not a valid send status.

`abc` is not a valid MongoDB ObjectId.

Without validation, this bad input reaches the controller.

With validation:

```text
status
  ↓
"isIn(['interested', 'ignored'])"
  ↓
❌ invalid
```

and:

```text
toUserId
  ↓
"isValidObjectId"
  ↓
❌ invalid
```

The request is rejected early.

---

# 4. First import

```js
import { param } from "express-validator";
```

`param()` comes from `express-validator`.

It allows us to validate values that are present in:

```js
req.params
```

For example:

```text
POST /request/send/interested/64abc123...
```

For this route:

```js
"/request/send/:status/:toUserId"
```

Express creates:

```js
req.params = {
  status: "interested",
  toUserId: "64abc123..."
}
```

Therefore:

```js
param("status")
```

means:

> Validate `req.params.status`.

And:

```js
param("toUserId")
```

means:

> Validate `req.params.toUserId`.

---

# 5. Route parameters vs body vs query

This is very important.

Suppose we have:

```text
POST /request/send/interested/64abc...
```

Then:

```js
req.params.status
req.params.toUserId
```

contain the values.

That is why we use:

```js
param()
```

---

## Query parameters

Example:

```text
GET /users?page=2&limit=10
```

Values are:

```js
req.query.page
req.query.limit
```

For these, use:

```js
query("page")
query("limit")
```

---

## Request body

Example:

```http
POST /users
Content-Type: application/json
```

```json
{
  "firstName": "Pranav",
  "age": 21
}
```

Values are:

```js
req.body.firstName
req.body.age
```

For these, use:

```js
body("firstName")
body("age")
```

---

## Main idea

```text
URL /:parameter
       ↓
     param()

?query=value
       ↓
     query()

JSON request body
       ↓
      body()
```

---

# 6. `mongoose` import

```js
import mongoose from "mongoose";
```

We use Mongoose because we want to check whether something looks like a valid MongoDB ObjectId.

---

# 7. The helper function

Your code has:

```js
const isValidObjectId = (value) =>
  mongoose.Types.ObjectId.isValid(value);
```

This is a reusable helper.

Conceptually:

```text
value
  ↓
ObjectId.isValid(value)
  ↓
true / false
```

Example:

```js
isValidObjectId("64f123...")
```

returns a boolean indicating whether Mongoose considers the value a valid ObjectId input.

---

# 8. Why are we validating ObjectIds?

Your route contains:

```text
:toUserId
```

and:

```text
:requestId
```

Both represent MongoDB document IDs.

For example:

```text
/request/send/interested/650ab12...
```

The last part should represent a user ID.

Similarly:

```text
/request/review/accepted/671cd45...
```

should contain a request document ID.

You do not want:

```text
/request/send/interested/hello
```

to reach the controller and eventually produce an unexpected database/cast error.

So we validate the format first.

---

# 9. What `custom()` means

You have:

```js
param("toUserId")
  .custom(isValidObjectId)
  .withMessage("toUserId must be a valid user id")
```

`custom()` allows us to provide our own validation function.

Normal built-in validators include things such as:

```js
.isEmail()
.isInt()
.isLength()
.isIn()
.notEmpty()
```

But sometimes your requirement is custom.

Here:

```js
.custom(isValidObjectId)
```

means:

> Run my own function against this parameter.

So internally the idea is:

```text
toUserId
   ↓
isValidObjectId(toUserId)
   ↓
true → validation passes
false → validation fails
```

---

# 10. Why not use `.isMongoId()`?

`express-validator` also provides validators for Mongo-style IDs.

Your current implementation instead uses:

```js
.custom(isValidObjectId)
```

because you are explicitly using Mongoose's ObjectId validation logic.

This also gives you a reusable helper that can be used for both:

```text
toUserId
requestId
```

The important lesson is not "always use custom".

The lesson is:

> Use a custom validator when your validation rule is better expressed by your own function.

---

# 11. The message after a validator

Example:

```js
.withMessage("toUserId must be a valid user id")
```

This defines the error message when the previous validator fails.

For example, if:

```text
toUserId = abc
```

the validation result can contain:

```text
toUserId must be a valid user id
```

This makes API errors understandable.

---

# 12. Understanding `sendRequestValidator`

Your code:

```js
const sendRequestValidator = [
  param("status")
    .isIn(["interested", "ignored"])
    .withMessage('status must be "interested" or "ignored"'),

  param("toUserId")
    .custom(isValidObjectId)
    .withMessage("toUserId must be a valid user id"),
];
```

This is an **array of middleware functions**.

It validates the parameters of:

```text
POST /request/send/:status/:toUserId
```

---

# 13. First validator — `status`

```js
param("status")
```

means:

```js
req.params.status
```

Then:

```js
.isIn(["interested", "ignored"])
```

means:

> The value must be one of these values.

Valid:

```text
interested
ignored
```

Invalid:

```text
accepted
rejected
hello
pending
```

---

# 14. Why only `interested` and `ignored`?

Because this is the **send** operation.

The sender is allowed to choose:

```text
interested
```

meaning:

> I want to connect.

or:

```text
ignored
```

meaning:

> I don't want this connection.

The sender is NOT allowed to send:

```text
accepted
rejected
```

because those are review decisions made by the recipient.

Therefore:

```text
SEND endpoint
     │
     ├── interested ✅
     └── ignored    ✅

     ├── accepted   ❌
     └── rejected   ❌
```

---

# 15. Why does the model allow all four statuses then?

This is a very important distinction.

The model has:

```js
enum: {
  values: [
    "interested",
    "ignored",
    "accepted",
    "rejected"
  ]
}
```

That means:

> A ConnectionRequest document may ultimately contain any of these four states.

But the endpoint validators define:

> Which states this particular endpoint is allowed to request.

So:

```text
MODEL
  ↓
All valid states
  ↓
interested
ignored
accepted
rejected
```

while:

```text
SEND ENDPOINT
  ↓
Only allowed input
  ↓
interested
ignored
```

and:

```text
REVIEW ENDPOINT
  ↓
Only allowed input
  ↓
accepted
rejected
```

This is why you should NOT simply use one validator for both endpoints.

---

# 16. Second validator — `toUserId`

```js
param("toUserId")
```

means:

```js
req.params.toUserId
```

Then:

```js
.custom(isValidObjectId)
```

checks whether it is a valid ObjectId input.

Then:

```js
.withMessage("toUserId must be a valid user id")
```

specifies the error message if it fails.

---

# 17. Understanding the complete send validator

Think of:

```js
const sendRequestValidator = [...]
```

as:

```text
POST /request/send/:status/:toUserId
                │          │
                │          │
                ↓          ↓
              status     toUserId
                │          │
                ↓          ↓
           allowed?     valid ID?
                │          │
                └────┬─────┘
                     ↓
              validation result
```

Both parameters need to be valid before the controller should run.

---

# 18. Understanding `reviewRequestValidator`

Your second validator is:

```js

// 
const reviewRequestValidator = [
  param("status")
    .isIn(["accepted", "rejected"])
    .withMessage('status must be "accepted" or "rejected"'),

  param("requestId")
    .custom(isValidObjectId)
    .withMessage("requestId must be a valid request id"),
];
```

This is for:

```text
POST /request/review/:status/:requestId
```

---

# 19. Why does review accept only `accepted` and `rejected`?

Because the recipient is reviewing a request.

The recipient should be able to say:

```text
accepted
```

or:

```text
rejected
```

They should not send:

```text
interested
ignored
```

because those belong to the sender's action.

So:

```text
SEND
   interested
   ignored

REVIEW
   accepted
   rejected
```

This represents your application's state-transition rules at the endpoint-input level.

---

# 20. `requestId` validation

The review route is:

```text
/request/review/:status/:requestId
```

Therefore:

```js
param("requestId")
```

reads:

```js
req.params.requestId
```

Then:

```js
.custom(isValidObjectId)
```

checks its ObjectId validity.

So the validator makes sure the request ID has a valid MongoDB ObjectId format.

---

# 21. What this validator DOES NOT check

This is perhaps the most important concept in this file.

The validator checks:

```text
Is the value syntactically valid?
```

It does NOT check:

```text
Does this user actually exist?
```

It does NOT check:

```text
Does this connection request actually exist?
```

It does NOT check:

```text
Does this request belong to the logged-in user?
```

It does NOT check:

```text
Is the request currently "interested"?
```

It does NOT check:

```text
Is the sender trying to send to themselves?
```

It does NOT check:

```text
Is there already an active connection?
```

Those are controller/service/business-logic responsibilities.

---

# 22. Example: valid ObjectId but nonexistent user

Suppose:

```text
toUserId = 64abcdef1234567890abcdef
```

and Mongoose considers it a valid ObjectId.

Validator:

```text
✅ valid format
```

But perhaps there is no User document with that `_id`.

Therefore:

```text
Validator
  ↓
✅ passes

Controller
  ↓
Check User
  ↓
❌ User not found
```

This is why validation does not replace database checks.

---

# 23. Example: valid request ID but wrong user

Suppose:

```text
requestId = 64abcdef1234567890abcdef
```

The ObjectId is valid.

So:

```text
Validator
  ↓
✅ passes
```

But perhaps the logged-in user is User C while the request is:

```text
A → B
```

User C must not be allowed to accept/reject it.

That is authorization/business logic.

The controller must verify who the recipient is.

---

# 24. Example: valid request ID but already rejected

Suppose the request already contains:

```js
{
  status: "rejected"
}
```

Someone sends:

```text
POST /request/review/accepted/:requestId
```

The validator sees:

```text
status = accepted
requestId = valid ObjectId
```

Everything passes.

But the request is already:

```text
rejected
```

So the controller must reject the operation because only:

```text
interested → accepted
```

or:

```text
interested → rejected
```

is allowed.

Again:

```text
Validator ≠ Business Logic
```

---

# 25. Very important distinction: validation vs authorization vs business logic

You will use this distinction throughout backend development.

## Validation

Question:

> Is the input structurally and syntactically valid?

Example:

```text
status = accepted ✅
requestId = valid ObjectId ✅
```

---

## Authentication

Question:

> Who is making this request?

Your middleware handles this using the access token.

For example:

```text
Bearer accessToken
      ↓
authenticate middleware
      ↓
req.user
```

---

## Authorization

Question:

> Is this authenticated user allowed to perform this operation?

Example:

```text
Request = A → B
Logged-in user = C
```

C should not be allowed to review it.

---

## Business logic

Question:

> Is this operation allowed according to our application's rules?

Examples:

```text
Can A send to B?
Is there already an active relationship?
Is the request still interested?
Can the sender send another request?
```

---

# 26. The complete request pipeline

Your DevTinder request will approximately flow like this:

```text
Client
  │
  ↓
Route
  │
  ↓
Authentication middleware
  │
  ↓
Request validator
  │
  ↓
Controller
  │
  ↓
Database
```

For example:

```text
POST /request/review/accepted/64abc...
          │
          ↓
       Route
          │
          ↓
   authenticateUser
          │
          ↓
 reviewRequestValidator
          │
          ↓
       Controller
          │
          ↓
   Business checks
          │
          ↓
     MongoDB update
```

Each layer has a different job.

---

# 27. What happens when validation fails?

Your project already has a `validate` middleware from Phase-01.

Conceptually:

```js
const validate = (req, res, next) => {
  const errors = validationResult(req);

  if (!errors.isEmpty()) {
    return next(
      new ApiError(
        400,
        "Validation failed",
        errors.array(),
      ),
    );
  }

  next();
};
```

Therefore the route can be:

```js
router.post(
  "/send/:status/:toUserId",
  authenticateUser,
  sendRequestValidator,
  validate,
  sendRequest,
);
```

The idea is:

```text
Request
   ↓
Authenticate
   ↓
Run validator rules
   ↓
Collect validation errors
   ↓
validate middleware
   ↓
error?
 ┌───────┴───────┐
 ↓               ↓
YES              NO
 ↓               ↓
ApiError         Controller
```

---

# 28. Important: validator array itself does not automatically send the response

This is another common beginner misunderstanding.

This:

```js
const sendRequestValidator = [
  param("status")...,
  param("toUserId")...,
];
```

only defines validation rules.

The validation result still needs to be checked.

That is the purpose of your:

```text
validate
```

middleware.

So:

```text
Validator rules
      ↓
Store errors in request
      ↓
validate middleware
      ↓
validationResult(req)
      ↓
error or next()
```

---

# 29. Why separate `sendRequestValidator` and `reviewRequestValidator`?

This is excellent design because these endpoints represent different actions.

### Send

```text
POST /request/send/:status/:toUserId
```

Allowed statuses:

```text
interested
ignored
```

### Review

```text
POST /request/review/:status/:requestId
```

Allowed statuses:

```text
accepted
rejected
```

If you merged them:

```js
.isIn([
  "interested",
  "ignored",
  "accepted",
  "rejected"
])
```

then this would incorrectly pass:

```text
POST /request/send/accepted/:toUserId
```

and:

```text
POST /request/review/interested/:requestId
```

The validator would say:

```text
✅ valid
```

even though the endpoint's action is invalid.

Separate validators prevent that.

---

# 30. Think of endpoint validation as a gate

Imagine the model allows four states:

```text
              MODEL
                │
       ┌────────┼────────┐
       ↓        ↓        ↓
 interested  accepted  rejected
       │                 │
       └────── ignored ──┘
```

But the endpoints have different gates.

```text
SEND GATE
   │
   ├── interested ✅
   └── ignored    ✅
```

and:

```text
REVIEW GATE
   │
   ├── accepted ✅
   └── rejected ✅
```

The model defines the vocabulary.

The endpoint validator defines which vocabulary is allowed for that operation.

---

# 31. Understanding `.isIn()`

This:

```js
.isIn(["interested", "ignored"])
```

means:

> The value must exist in this array.

Conceptually:

```js
const allowed = ["interested", "ignored"];

allowed.includes(value);
```

Examples:

```text
"interested" → true
"ignored"    → true
"accepted"   → false
"hello"      → false
```

---

# 32. Understanding `.withMessage()`

Example:

```js
.isIn(["accepted", "rejected"])
.withMessage('status must be "accepted" or "rejected"')
```

Think of it as:

```text
Rule:
status must be accepted/rejected

     ↓
If rule fails

     ↓

Use this error message
```

This makes your validation errors much cleaner than generic messages.

---

# 33. Understanding `.custom()` in this code

```js
.custom(isValidObjectId)
```

is approximately conceptually equivalent to:

```js
.custom((value) => {
  return mongoose.Types.ObjectId.isValid(value);
})
```

So:

```js
param("toUserId")
```

selects the parameter, and:

```js
.custom(isValidObjectId)
```

defines how to validate it.

Together:

```text
req.params.toUserId
        ↓
isValidObjectId(value)
        ↓
true / false
```

---

# 34. One technical detail about ObjectId validation

Your current helper is:

```js
mongoose.Types.ObjectId.isValid(value)
```

It asks Mongoose whether the value is considered a valid/castable ObjectId input.

That is slightly broader than the rule:

> "must be exactly 24 hexadecimal characters."

If your project specifically wants **strict 24-character Mongo ObjectId strings**, Mongoose also provides stricter ObjectId helpers in modern versions.

For the current DevTinder learning step, the important concept is:

```text
ObjectId.isValid()
    ↓
basic ObjectId validity check
```

You do not need to change your code just to understand Step-02.

---

# 35. Full example: send request

Request:

```text
POST /request/send/interested/650f1234567890abcdef1234
```

Express parses:

```js
req.params = {
  status: "interested",
  toUserId: "650f1234567890abcdef1234"
};
```

Validator 1:

```text
status
 ↓
isIn(["interested", "ignored"])
 ↓
✅
```

Validator 2:

```text
toUserId
 ↓
isValidObjectId()
 ↓
✅
```

Then:

```text
validate middleware
 ↓
no errors
 ↓
controller
```

---

# 36. Full example: invalid send status

Request:

```text
POST /request/send/accepted/650f1234567890abcdef1234
```

Validator:

```text
status = accepted
        ↓
isIn(["interested", "ignored"])
        ↓
❌
```

Validation error:

```text
status must be "interested" or "ignored"
```

Controller should not run.

---

# 37. Full example: invalid request ID

Request:

```text
POST /request/review/accepted/hello
```

Validator:

```text
status = accepted
        ↓
✅

requestId = hello
        ↓
isValidObjectId()
        ↓
❌
```

Result:

```text
400 Bad Request
```

with your validation error.

---

# 38. Full example: validation passes but controller rejects

Request:

```text
POST /request/review/accepted/650f1234567890abcdef1234
```

Everything has a valid format.

Therefore:

```text
Validator
   ↓
✅
```

But database contains:

```js
{
  fromUserId: A,
  toUserId: B,
  status: "rejected"
}
```

and logged-in user is B.

The controller must say:

```text
Cannot review this request.
The request is no longer interested.
```

This illustrates the key boundary:

```text
VALIDATION
"What did you send?"

BUSINESS LOGIC
"Are you allowed to do it?"
```

---

# 39. Why validation should happen before controller logic

Without validation, your controller would become full of checks like:

```js
if (!["interested", "ignored"].includes(req.params.status)) ...
if (!mongoose.Types.ObjectId.isValid(req.params.toUserId)) ...
if (!...) ...
```

This makes the controller large and difficult to maintain.

Instead:

```text
Validator
  ↓
Input validation
  ↓
Controller
  ↓
Business logic
```

Now each layer has a clear responsibility.

---

# 40. Your current code, explained line by line

```js
import { param } from "express-validator";
```

Import `param()` so URL parameters can be validated.

```js
import mongoose from "mongoose";
```

Import Mongoose so we can use ObjectId validation.

```js
const isValidObjectId = (value) =>
  mongoose.Types.ObjectId.isValid(value);
```

Reusable helper that checks whether a value is considered a valid Mongoose ObjectId.

---

```js
const sendRequestValidator = [
```

Create the validation rules for the send endpoint.

```js
param("status")
```

Validate:

```js
req.params.status
```

```js
.isIn(["interested", "ignored"])
```

Only allow:

```text
interested
ignored
```

```js
.withMessage(
  'status must be "interested" or "ignored"',
)
```

Custom error message when that rule fails.

---

```js
param("toUserId")
```

Validate:

```js
req.params.toUserId
```

```js
.custom(isValidObjectId)
```

Use our custom ObjectId check.

```js
.withMessage("toUserId must be a valid user id")
```

Error message if it fails.

---

```js
const reviewRequestValidator = [
```

Create a separate validation rule set for reviewing requests.

```js
param("status")
```

Validate:

```js
req.params.status
```

```js
.isIn(["accepted", "rejected"])
```

Only allow review decisions.

```js
param("requestId")
```

Validate:

```js
req.params.requestId
```

```js
.custom(isValidObjectId)
```

Check whether it is a valid ObjectId.

---

# 41. Full architecture for this feature

Your DevTinder request feature now looks like:

```text
                   CONNECTION REQUEST
                           │
                           ↓
                    ┌────────────┐
                    │   ROUTER   │
                    └─────┬──────┘
                          ↓
                  Authentication
                          │
                          ↓
                  ┌──────────────┐
                  │   Validator  │
                  └──────┬───────┘
                         │
              ┌──────────┴──────────┐
              ↓                     ↓
         Send Validator        Review Validator
              │                     │
       status: interested      status: accepted
       status: ignored         status: rejected
              │                     │
              └──────────┬──────────┘
                         ↓
                    validate()
                         │
                         ↓
                    Controller
                         │
                         ↓
                 Business Logic
                         │
                         ↓
                      Model
                         │
                         ↓
                     MongoDB
```

---

# 42. The three layers you should remember

For your DevTinder backend:

```text
1. Validator
   ↓
   Is the input valid?

2. Controller / Service
   ↓
   Is the operation allowed?

3. Model / Database
   ↓
   Is the resulting data valid and consistent?
```

Example:

```text
POST /request/send/interested/abc
```

### Validator

```text
abc is not valid ObjectId
→ reject
```

No need to continue.

---

Example:

```text
POST /request/send/interested/validObjectId
```

### Validator

```text
✅
```

### Controller

```text
Is the target user real?
Are you sending to yourself?
Does an active relationship already exist?
```

### Model/DB

```text
Can the resulting document be saved?
Does an index constraint get violated?
```

---

# 43. Relationship with Step-01

Step-01:

```text
ConnectionRequest Schema
```

defines:

```text
What a ConnectionRequest document may contain
```

Step-02:

```text
Request Validators
```

defines:

```text
What the API is willing to accept as input
```

So:

```text
                API REQUEST
                    │
                    ↓
             STEP-02 VALIDATOR
                    │
                    ↓
             STEP-01 MODEL
                    │
                    ↓
                DATABASE
```

They are related, but they solve different problems.

---

# 44. Exam/interview-level definitions

### Request validation

> Request validation is the process of checking incoming client data against predefined rules before executing business logic.

### `param()`

> `param()` from express-validator is used to validate parameters present in `req.params`.

### `.isIn()`

> `.isIn()` validates that a value belongs to a specified set of allowed values.

### `.custom()`

> `.custom()` allows developers to define and execute custom validation logic.

### ObjectId validation

> ObjectId validation checks whether a value is a valid MongoDB/Mongoose ObjectId input before database operations.

### Validator middleware

> Validator middleware validates request input and prevents invalid requests from reaching the controller.

---

# 45. Final mental model

Remember this:

```text
URL
│
├── /request/send/:status/:toUserId
│
│      status
│        ↓
│   interested / ignored
│
│      toUserId
│        ↓
│   valid ObjectId
│
└── /request/review/:status/:requestId
       status
         ↓
    accepted / rejected

       requestId
         ↓
    valid ObjectId
```

And the most important distinction:

```text
VALIDATOR
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Checks INPUT FORMAT / ALLOWED INPUT

"Is this status allowed here?"
"Is this ID a valid ObjectId?"

            ↓

CONTROLLER
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Checks BUSINESS RULES

"Does the user exist?"
"Does the request exist?"
"Is this the recipient?"
"Is the request still interested?"
"Is there already an active connection?"

            ↓

MODEL / DATABASE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Protects DATA INTEGRITY

"Are required fields present?"
"Is status one of the model's valid states?"
"Does a unique index get violated?"
```

## What you should understand before moving to Step-03

You should now be comfortable with these concepts:

```text
param()
req.params
isIn()
custom()
withMessage()
validator arrays
ObjectId validation
validation middleware
input validation
business-logic validation
authorization vs validation
model validation vs route validation
```

The single sentence to remember is:

> **A validator answers "Is this input acceptable?"; the controller answers "Is this action allowed?"**