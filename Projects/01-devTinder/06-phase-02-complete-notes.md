
---


# DevTinder Backend — Phase 02: Profile Management & Password Change

## 1. Phase Objective

Phase 02 adds authenticated user profile operations:

```text
Authentication
     ↓
Authenticate user
     ↓
Profile Management
 ┌───────────────┐
 │ View Profile  │
 │ Edit Profile  │
 │ Change Pass   │
 └───────────────┘
```

### APIs implemented

|Method|Endpoint|Purpose|
|---|---|---|
|GET|`/api/profile/view`|View authenticated user's profile|
|PATCH|`/api/profile/edit`|Partially update allowed profile fields|
|PATCH|`/api/profile/change-password`|Change password and invalidate sessions|

---

# 2. Validation Layer

File:

```text
src/validators/profile.validator.js
```

Uses:

```js
import { body } from "express-validator";
```

The validator is responsible for **request-data validation**, not authentication or database/business logic.

---

# 3. `profileValidator`

Used by:

```http
PATCH /api/profile/edit
```

All fields are optional because `PATCH` supports **partial updates**.

### `age`

```js
body("age")
  .optional()
  .isInt({ min: 13, max: 100 })
```

Accepts only an integer between **13 and 100**.

---

### `about`

```js
body("about")
  .optional()
  .trim()
  .isLength({ max: 500 })
```

- Optional
    
- Removes leading/trailing whitespace
    
- Maximum 500 characters
    

---

### `skills`

```js
body("skills")
  .optional()
  .isArray({ max: 10 })
```

Requires an array containing at most **10 items**.

Then:

```js
body("skills.*")
```

validates **each element of the array**.

```js
.trim()
.notEmpty()
```

Therefore every skill must be a non-empty string after trimming.

Example:

```json
{
  "skills": ["Node.js", "MongoDB", "Express"]
}
```

---

### `photoUrl`

```js
body("photoUrl")
  .optional()
  .trim()
  .isURL()
```

Ensures the supplied value is a valid URL.

---

# 4. `changePasswordValidator`

Used by:

```http
PATCH /api/profile/change-password
```

Three request fields are required:

```text
currentPassword
newPassword
confirmPassword
```

### Current password

```js
body("currentPassword")
  .notEmpty()
```

Ensures the user provides their current password.

This **does not verify the password against the database**. Actual verification happens in the controller using bcrypt.

---

### New password

```js
.isStrongPassword({
  minLength: 8,
  minLowercase: 1,
  minUppercase: 1,
  minNumbers: 1,
  minSymbols: 1,
})
```

Requires:

```text
Minimum length     → 8
Lowercase          → ≥ 1
Uppercase          → ≥ 1
Number             → ≥ 1
Symbol             → ≥ 1
```

It also uses `.custom()` to ensure:

```text
newPassword ≠ currentPassword
```

---

# 5. `.custom()` and `meta`

Example:

```js
.custom((value, { req }) => {
```

`express-validator` automatically provides the custom validator with:

```text
1st argument → value
2nd argument → meta object
```

The `meta` object contains validation information such as:

```text
req
location
path
```

Using:

```js
{ req }
```

destructures the `req` property from the metadata object.

Therefore:

```js
req.body.currentPassword
```

allows the validator to access another field from the same request.

### In this validator

```text
value
  ↓
newPassword

req.body.currentPassword
  ↓
currentPassword
```

They are compared to ensure the new password is different.

---

# 6. Confirm Password Validation

```js
body("confirmPassword")
  .notEmpty()
  .custom((value, { req }) => {
```

The custom rule checks:

```js
value !== req.body.newPassword
```

Therefore:

```text
confirmPassword === newPassword
```

must be true.

---

# 7. Validation Flow

The routes use:

```text
Request
   ↓
authenticate
   ↓
profileValidator
   ↓
validate
   ↓
controller
   ↓
database
   ↓
response
```

### Important distinction

```text
authenticate
→ Who is the user?

validator
→ Is the request data valid?

controller
→ What should the application do?

Mongoose
→ How is the database changed?
```

---

# 8. Profile Routes

File:

```text
src/routes/profile.routes.js
```

Router:

```js
const profileRouter = Router();
```

### View profile

```js
profileRouter.get("/view", authenticate, getProfile);
```

Flow:

```text
GET /api/profile/view
        ↓
authenticate
        ↓
getProfile
```

No request-body validation is needed.

---

### Edit profile

```js
profileRouter.patch(
  "/edit",
  authenticate,
  profileValidator,
  validate,
  updateProfile
);
```

Flow:

```text
PATCH /api/profile/edit
        ↓
authenticate
        ↓
profileValidator
        ↓
validate
        ↓
updateProfile
```

---

### Change password

```js
profileRouter.patch(
  "/change-password",
  authenticate,
  changePasswordValidator,
  validate,
  changePassword
);
```

Flow:

```text
PATCH /api/profile/change-password
        ↓
authenticate
        ↓
changePasswordValidator
        ↓
validate
        ↓
changePassword
```

---

# 9. Controller Layer

File:

```text
src/controllers/profile.controller.js
```

The controller handles **business logic** after authentication and validation have succeeded.

---

# 10. `sanitizeUser()`

```js
const sanitizeUser = (user) => ({
  id: user._id,
  firstName: user.firstName,
  lastName: user.lastName,
  email: user.email,
  age: user.age,
  gender: user.gender,
  about: user.about,
  skills: user.skills,
  photoUrl: user.photoUrl,
  isPremium: user.isPremium,
  membershipType: user.membershipType,
  role: user.role,
});
```

### Purpose

Creates a **safe response representation** of the user.

Instead of sending the complete Mongoose document:

```js
res.json(user);
```

we explicitly select fields that are allowed to reach the client.

This provides an additional protection against accidentally exposing sensitive/internal fields.

### Important

`sanitizeUser()` is a **response transformation**, not a replacement for:

```js
select: false
```

on sensitive database fields such as passwords.

Both protections serve different purposes.

---

# 11. `getProfile`

```js
data: sanitizeUser(req.user)
```

`req.user` is populated by the authentication middleware.

The controller does not need to query the database again if the middleware has already attached the required user information.

Response structure:

```json
{
  "status": "success",
  "message": "Profile retrieved successfully",
  "data": {}
}
```

---

# 12. `updateProfile`

Endpoint:

```http
PATCH /api/profile/edit
```

## Step 1 — Get authenticated user

```js
const user = req.user;
```

The authentication middleware is expected to attach the authenticated user to `req.user`.

A defensive check is still present:

```js
if (!user) {
  throw new ApiError(401, "Unauthorized");
}
```

---

# 13. Allow-list / Field Whitelisting

The most important security concept in this controller is:

```js
const ALLOWED_FIELDS = [
  "age",
  "about",
  "skills",
  "photoUrl"
];
```

Only these fields may be modified through this endpoint.

### Why?

Never blindly do:

```js
userModel.findByIdAndUpdate(req.user._id, req.body);
```

A malicious client could attempt:

```json
{
  "age": 21,
  "role": "admin",
  "isPremium": true
}
```

The explicit allow-list prevents unauthorized fields from reaching the update operation.

---

# 14. Building the Update Object

```js
const updates = {};

for (const field of ALLOWED_FIELDS) {
  if (req.body[field] !== undefined) {
    updates[field] = req.body[field];
  }
}
```

The controller constructs a **new update object** containing only allowed fields actually supplied by the client.

Example:

```json
{
  "about": "Backend developer",
  "age": 21
}
```

becomes:

```js
{
  about: "Backend developer",
  age: 21
}
```

---

# 15. Why Check `!== undefined`?

This allows the API to distinguish between:

```text
field not supplied
        ↓
do not update it
```

and a supplied value.

This is important for `PATCH`, where the client may update only selected fields.

---

# 16. Empty Update Protection

```js
if (Object.keys(updates).length === 0) {
  throw new ApiError(
    400,
    "No valid fields provided for update"
  );
}
```

If the request contains no allowed fields, there is nothing meaningful to update.

Therefore the API returns:

```text
400 Bad Request
```

---

# 17. Mongoose Update

```js
const updatedUser = await userModel.findByIdAndUpdate(
  user._id,
  updates,
  {
    new: true,
    runValidators: true,
  }
);
```

### `new: true`

Returns the **updated document** instead of the old document.

### `runValidators: true`

Runs Mongoose schema validators during the update.

### Important distinction

There are two validation layers:

```text
express-validator
      ↓
Request/API validation

Mongoose validators
      ↓
Database/model-level validation
```

They complement each other.

---

# 18. Why Check `updatedUser` Again?

Even though `req.user` exists, the database operation can still return `null`.

Possible situation:

```text
Request begins
    ↓
authenticate → user exists
    ↓
User gets deleted
    ↓
findByIdAndUpdate()
    ↓
No document found
```

Therefore:

```js
if (!updatedUser) {
  throw new ApiError(404, "User not found");
}
```

is a defensive database existence check.

---

# 19. Profile Update Response

```json
{
  "status": "success",
  "message": "Profile updated successfully",
  "data": {}
}
```

The returned user is passed through:

```js
sanitizeUser(updatedUser)
```

before being sent to the client.

---

# 20. `changePassword`

Endpoint:

```http
PATCH /api/profile/change-password
```

The controller receives:

```js
const { currentPassword, newPassword } = req.body;
```

---

# 21. Why `req.user` Cannot Directly Be Used for Password Comparison

The authenticated user object does not contain the password because the password field is configured with:

```js
select: false
```

Therefore:

```js
req.user.password
```

is unavailable.

The controller explicitly fetches the password:

```js
const user = await userModel
  .findById(req.user._id)
  .select("+password");
```

### `.select("+password")`

Explicitly includes a field that normally has `select: false`.

---

# 22. Fetch User

```js
if (!user) {
  throw new ApiError(404, "User not found");
}
```

The authenticated user's ID is used to retrieve the database record.

---

# 23. Verify Current Password

```js
const isCurrentPasswordValid = await bcrypt.compare(
  currentPassword,
  user.password
);
```

`bcrypt.compare()` compares:

```text
Plain-text password
        +
Stored bcrypt hash
        ↓
true / false
```

The original password is **not decrypted**.

If incorrect:

```js
throw new ApiError(
  401,
  "Current password is incorrect"
);
```

---

# 24. Hash New Password

After the current password is verified:

```js
const hashedPassword = await bcrypt.hash(newPassword, 10);
```

The new password is converted into a bcrypt hash.

The plaintext password should never be stored in the database.

---

# 25. Save New Password

```js
user.password = hashedPassword;
await user.save();
```

The newly generated hash replaces the old password hash.

---

# 26. Session Revocation

After changing the password:

```js
await sessionModel.updateMany(
  { user: user._id, revoked: false },
  { $set: { revoked: true } }
);
```

This revokes **all active sessions** belonging to the user.

### Why?

Suppose the user is logged in on:

```text
Laptop
Phone
Tablet
```

and changes their password.

All existing sessions are revoked:

```text
Password changed
       ↓
Revoke all sessions
       ↓
Existing refresh tokens stop working
       ↓
Login required again
```

This is an important security measure after a credential change.

---

# 27. Clear Current Refresh-Token Cookie

```js
clearRefreshTokenCookie(res);
```

This removes the refresh-token cookie from the current browser/device.

There are therefore two related actions:

```text
Database
→ revoke all sessions

Current client
→ clear refresh-token cookie
```

---

# 28. Password Change Response

```json
{
  "status": "success",
  "message": "Password changed successfully. Please log in again."
}
```

The API does not return the password or password hash.

---

# 29. Centralized Error Handling

All controller operations use:

```js
try {
  // logic
} catch (error) {
  next(error);
}
```

Instead of manually formatting errors in every controller, the error is passed to the application's centralized error middleware.

Custom errors use:

```js
throw new ApiError(statusCode, message);
```

This keeps error handling consistent across the application.

---

# 30. Complete Phase-02 Architecture

```text
                    CLIENT
                      │
                      ▼
              PATCH /api/profile/edit
                      │
                      ▼
              authenticate.middleware
                      │
                 req.user
                      │
                      ▼
              profileValidator
                      │
                      ▼
               validate.middleware
                      │
                      ▼
             profile.controller
                      │
             ┌────────┴─────────┐
             ▼                  ▼
        Allow-list          Business Logic
             │                  │
             └────────┬─────────┘
                      ▼
                   Mongoose
                      │
                      ▼
                sanitizeUser()
                      │
                      ▼
                  Response
```

Password flow:

```text
PATCH /change-password
          ↓
     authenticate
          ↓
 changePasswordValidator
          ↓
       validate
          ↓
  Fetch user + password
          ↓
 bcrypt.compare()
          ↓
 Current password valid?
      │          │
     No          Yes
      ↓           ↓
    401       bcrypt.hash()
                  ↓
             Save password
                  ↓
          Revoke all sessions
                  ↓
        Clear refresh cookie
                  ↓
               200 OK
```

---

# 31. Key Industry Practices Used in Phase 02

### 1. Partial updates

`PATCH` is used because users can update selected profile fields instead of replacing the entire profile.

### 2. Input validation

`express-validator` validates incoming API data before controller logic.

### 3. Field allow-listing

Only explicitly permitted fields can be updated.

### 4. Defense in depth

Validation exists at both:

```text
API layer → express-validator
Model layer → Mongoose validators
```

### 5. Sensitive-field protection

Password is excluded by default and explicitly selected only when required.

### 6. Password hashing

Passwords are stored as bcrypt hashes, never plaintext.

### 7. Password verification

`bcrypt.compare()` verifies the existing password without decrypting the stored hash.

### 8. Session invalidation

Changing a password revokes existing sessions so old refresh tokens cannot continue authenticating the user.

### 9. Response sanitization

`sanitizeUser()` controls exactly which user fields are exposed to the client.

### 10. Centralized errors

Controllers delegate errors to the centralized error middleware through `next(error)`.

---

# 32. Important Things to Remember

```text
express-validator
→ Validates incoming request data

authenticate
→ Establishes authenticated user

req.user
→ User information attached by authentication middleware

select("+password")
→ Explicitly include normally-hidden password field

bcrypt.compare()
→ Verify plain password against stored hash

bcrypt.hash()
→ Create hash for new password

ALLOWED_FIELDS
→ Prevent unauthorized field updates

runValidators: true
→ Run Mongoose validators during findByIdAndUpdate

sanitizeUser()
→ Control fields exposed in API response

sessionModel.updateMany()
→ Revoke all active sessions

clearRefreshTokenCookie()
→ Remove refresh token from current browser
```

### Phase-02 core principle

> **Validate the input → authenticate the user → allow only permitted changes → perform business logic → protect sensitive data → invalidate sessions when credentials change → return a sanitized response.**