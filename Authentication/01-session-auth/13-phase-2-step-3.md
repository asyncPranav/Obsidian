

---

Excellent. ✅ **Phase 2 — Registration + password hashing is complete.**

Now we're moving to the most important part of authentication:

# Step 3 — Login

Registration answers:

> **"Can I create an account securely?"**

Login answers:

> **"Can I prove that I am the owner of this account?"**

And this is where you'll learn why we **don't decrypt the bcrypt hash**.

---

## 1. What happens during login?

Suppose MongoDB contains:

```text
User
├── email: test@example.com
└── password: $2b$10$....
```

The user sends:

```json
{
  "email": "test@example.com",
  "password": "Test@123"
}
```

We need to determine:

```text
Does "Test@123"
match
the stored bcrypt hash?
```

The flow is:

```text
POST /api/auth/login
        ↓
Validate input
        ↓
Find user by email
        ↓
Get stored password hash
        ↓
bcrypt.compare()
        ↓
Password correct?
      /     \
    NO       YES
    ↓         ↓
  401     Create session
            ↓
         Cookie
```

Notice something important:

### We do NOT do this:

```text
stored hash
    ↓
decrypt
    ↓
original password
```

Instead:

```text
submitted password
        +
stored hash
        ↓
bcrypt.compare()
        ↓
true / false
```

---

# 2. Add login validation

We already have:

```text
validators/createUser.validator.js
```

But login shouldn't require:

```text
name
```

So create:

```text
src/validators/login.validator.js
```

```js
import { body } from "express-validator";

const loginValidator = [
  body("email")
    .trim()
    .notEmpty()
    .withMessage("Email is required")
    .normalizeEmail()
    .isEmail()
    .withMessage("Invalid email format"),

  body("password")
    .notEmpty()
    .withMessage("Password is required"),
];

export default loginValidator;
```

Notice we're **not using `isStrongPassword()` for login**.

Why?

Because login isn't the place to decide whether the password is strong.

The user already has a password.

We're simply asking:

> "Did they provide a password?"

The registration validator decides whether a **new password** meets our password requirements.

---

# 3. Add `login` to `auth.controller.js`

Your current controller has:

```js
register()
```

Now add:

```js
const login = async (req, res, next) => {
  try {
    const { email, password } = req.body;

    // 1. Find user by email
    const user = await User.findOne({ email });

    if (!user) {
      return next(new ApiError(401, "Invalid email or password"));
    }

    // 2. Compare submitted password with stored hash
    const isPasswordCorrect = await bcrypt.compare(
      password,
      user.password,
    );

    if (!isPasswordCorrect) {
      return next(new ApiError(401, "Invalid email or password"));
    }

    // Temporary response — session comes next
    res.status(200).json({
      status: "success",
      message: "Login successful",
      data: {
        user: {
          id: user._id,
          name: user.name,
          email: user.email,
          role: user.role,
        },
      },
    });
  } catch (error) {
    next(error);
  }
};
```

And change your export:

```js
export { register, login };
```

---

# 4. Understand the most important part

This:

```js
const isPasswordCorrect = await bcrypt.compare(
  password,
  user.password,
);
```

is the heart of login.

Imagine:

```text
req.body.password
        ↓
    "Test@123"
```

And MongoDB:

```text
user.password
        ↓
"$2b$10$......"
```

Then:

```text
bcrypt.compare(
    "Test@123",
    "$2b$10$......"
)
```

returns:

```js
true
```

if correct.

Or:

```js
false
```

if incorrect.

---

# 5. Why don't we compare strings?

You might wonder why we don't do:

```js
if (password === user.password)
```

Because:

```text
password:
Test@123

database:
$2b$10$.....
```

They're obviously different.

The stored value isn't supposed to be the original password.

`bcrypt.compare()` understands the bcrypt hash format and performs the appropriate verification.

---

# 6. Why use the same error for email and password?

Notice:

```js
if (!user) {
  return next(new ApiError(401, "Invalid email or password"));
}
```

and:

```js
if (!isPasswordCorrect) {
  return next(new ApiError(401, "Invalid email or password"));
}
```

We deliberately don't say:

```text
Email doesn't exist
```

or:

```text
Password is wrong
```

because that can reveal whether an account exists.

Instead:

```text
Invalid email or password
```

is a generic authentication failure.

---

# 7. Add the route

Update your `auth.route.js`.

Currently you have:

```js
import { register } from "../controllers/auth.controller.js";
```

Change it to:

```js
import { register, login } from "../controllers/auth.controller.js";
```

Import the validator:

```js
import loginValidator from "../validators/login.validator.js";
```

Then add:

```js
router.post(
  "/login",
  loginValidator,
  validate,
  login,
);
```

So your authentication routes are now:

```text
POST /api/auth/register
POST /api/auth/login
```

---

# 8. Test in Postman

Use an account that you just registered.

```http
POST http://localhost:5000/api/auth/login
```

Body:

```json
{
  "email": "test@example.com",
  "password": "Test@123"
}
```

Expected:

```json
{
  "status": "success",
  "message": "Login successful",
  "data": {
    "user": {
      "id": "...",
      "name": "Test User",
      "email": "test@example.com",
      "role": "user"
    }
  }
}
```

---

## Now test the failure cases

### Wrong password

```json
{
  "email": "test@example.com",
  "password": "WrongPassword@123"
}
```

Expected:

```text
401
Invalid email or password
```

### Nonexistent email

```json
{
  "email": "doesnotexist@example.com",
  "password": "Test@123"
}
```

Expected:

```text
401
Invalid email or password
```

### Missing password

```json
{
  "email": "test@example.com"
}
```

Expected:

```text
400
Password is required
```

---

# ⚠️ One important thing

After this step, **login will say "Login successful", but the user is NOT actually logged in yet.**

That's intentional.

Right now:

```text
REGISTER
   ↓
hashed password
   ↓
MongoDB
   ↓
✅ DONE


LOGIN
   ↓
find user
   ↓
compare password
   ↓
✅ credentials verified
   ↓
❌ no session yet
```

We're stopping here temporarily because I want you to clearly see the distinction:

> **Verifying credentials is not the same thing as maintaining authentication.**

After a successful login, the server currently forgets the request as soon as the request finishes.

Next, we're going to solve exactly that:

```text
Login successful
       ↓
Create a SESSION
       ↓
Give client a COOKIE
       ↓
Client sends cookie
       ↓
Server recognizes user
```

**Implement and test login first.** Don't add sessions yet.