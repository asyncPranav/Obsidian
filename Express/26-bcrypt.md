

----

# Complete bcrypt Notes (Very Detailed)

# 1. What is bcrypt?

bcrypt is a library used to:

```text
hash passwords securely
```

before storing them in database.

---

# Simple Definition

> bcrypt converts normal passwords into secure unreadable hashes.

---

# Main Purpose

```text
Protect user passwords
```

---

# 2. Main Problem bcrypt Solves

# Question

> “Why can’t we store passwords directly in database?”

Suppose user password is:

```text
dark123
```

If database stores:

```text
dark123
```

this is VERY dangerous.

If hacker steals database:

- all passwords exposed
    

---

# Real-world Risk

Many users reuse same password:

- Gmail
    
- Instagram
    
- banking
    

So leaked passwords become huge security issue.

---

# bcrypt solves this problem.

---

# 3. What is Hashing?

VERY IMPORTANT.

---

# Definition

> Hashing converts data into fixed-length unreadable string.

Example:

```text
dark123
```

becomes:

```text
$2b$10$N9qo8uLOickgx2ZMRZoMye...
```

---

# Important Property

Hashing is:

```text
one-way
```

Meaning:

- cannot reverse original password easily
    

---

# 4. Encryption vs Hashing

VERY IMPORTANT DIFFERENCE.

|Encryption|Hashing|
|---|---|
|reversible|one-way|
|can decrypt|cannot decrypt|
|used for secure communication|used for passwords|

---

# Example

Encryption:

```text
decrypt possible
```

Hashing:

```text
decrypt not intended
```

---

# 5. Why bcrypt Specifically?

# Question

> “Why not simple hashing?”

Simple hashing algorithms:

- fast
    
- easier to brute force
    

bcrypt is:

- intentionally slow
    
- more secure
    

---

# bcrypt protects against:

- brute-force attacks
    
- rainbow table attacks
    

---

# 6. Installing bcrypt

```bash
npm install bcrypt
```

---

# 7. Importing bcrypt

```js
const bcrypt = require("bcrypt");
```

---

# 8. Basic Password Hashing

```js
const bcrypt = require("bcrypt");

const password = "dark123";

bcrypt.hash(password, 10, (err, hash) => {

  console.log(hash);

});
```

---

# Output Example

```text
$2b$10$8f8uDsjhshsjd...
```

---

# 9. Understanding hash() Syntax

```js
bcrypt.hash(password, saltRounds, callback)
```

---

# Parameters

|Parameter|Meaning|
|---|---|
|password|original password|
|saltRounds|security complexity|
|callback|returns hash|

---

# 10. What are Salt Rounds?

MOST IMPORTANT bcrypt concept.

---

# Question

> “What does 10 mean in bcrypt.hash(password, 10)?”

It means:

```text
salt rounds / cost factor
```

---

# Meaning

Higher rounds:

- more secure
    
- slower hashing
    

---

# Example

|Salt Rounds|Speed|
|---|---|
|5|fast|
|10|recommended|
|15|slower|
|20|very slow|

---

# Why intentionally slow?

Because attackers also become slow.

---

# 11. What is Salt?

VERY IMPORTANT.

---

# Definition

> Salt = random data added to password before hashing.

---

# Why Needed?

Without salt:

Same password → same hash

BAD.

Example:

```text
dark123 → abc
dark123 → abc
```

Hackers can predict hashes.

---

# With Salt

```text
dark123 → xyz111
dark123 → pqr999
```

Now:

- same password
    
- different hashes
    

Much safer.

---

# 12. bcrypt Automatically Handles Salt

You don’t usually manually create salt.

bcrypt does:

- generate salt
    
- combine with password
    
- hash securely
    

internally.

---

# 13. Internally What bcrypt Does

VERY IMPORTANT FLOW.

---

# Flow

```text
User password
      ↓
Generate random salt
      ↓
Combine password + salt
      ↓
Run hashing algorithm multiple times
      ↓
Generate final hash
```

---

# 14. Full Signup Flow Using bcrypt

MOST IMPORTANT REAL-WORLD FLOW.

---

# Signup Flow

```text
User enters password
        ↓
Server receives password
        ↓
bcrypt hashes password
        ↓
Hash stored in database
```

---

# Example

```js
const password = "dark123";

bcrypt.hash(password, 10, async (err, hash) => {

  console.log(hash);

  // save hash in DB

});
```

---

# Database Stores

```text
$2b$10$KJHSDKJHSDKJ...
```

NOT:

```text
dark123
```

---

# 15. Login Flow Using bcrypt.compare()

VERY IMPORTANT.

---

# Main Question

> “How login works if original password is not stored?”

Answer:

- compare entered password with stored hash
    

---

# Syntax

```js
bcrypt.compare(password, hash, callback)
```

---

# Example

```js
bcrypt.compare(
  "dark123",
  storedHash,
  (err, result) => {

    console.log(result);

  }
);
```

---

# Result

```text
true
```

if password matches.

---

# 16. Complete Login Flow

VERY IMPORTANT.

---

# Flow

```text
User enters password
        ↓
Server gets stored hash from DB
        ↓
bcrypt.compare()
        ↓
bcrypt hashes entered password again internally
        ↓
Compares hashes
        ↓
true or false returned
```

---

# 17. Important Understanding

# Question

> “Does bcrypt decrypt hash during login?”

NO.

bcrypt NEVER decrypts.

Instead:

- hashes entered password again
    
- compares results
    

---

# 18. Async vs Sync Methods

bcrypt provides:

- async methods
    
- sync methods
    

---

# Async Version

Preferred.

```js
bcrypt.hash(password, 10, callback)
```

---

# Sync Version

```js
const hash = bcrypt.hashSync(password, 10);
```

---

# Why async preferred?

Because hashing is CPU intensive.

Sync can block server.

---

# 19. Using Async/Await

Modern approach.

---

# Hashing

```js
const hash = await bcrypt.hash(password, 10);
```

---

# Compare

```js
const isMatch = await bcrypt.compare(password, hash);
```

---

# 20. Full Signup Example

```js
app.post("/signup", async (req, res) => {

  const { password } = req.body;

  const hashedPassword =
    await bcrypt.hash(password, 10);

  console.log(hashedPassword);

  res.send("User created");

});
```

---

# 21. Full Login Example

```js
app.post("/login", async (req, res) => {

  const { password } = req.body;

  const storedHash =
    "$2b$10$abcxyz";

  const isMatch =
    await bcrypt.compare(password, storedHash);

  if(isMatch) {
    res.send("Login success");
  }
  else {
    res.send("Wrong password");
  }

});
```

---

# 22. Why bcrypt is Better Than Plain Hashing

---

# A. Salt

Prevents predictable hashes.

---

# B. Slow Hashing

Makes brute force difficult.

---

# C. Adaptive

Can increase salt rounds as computers improve.

---

# 23. What is Brute Force Attack?

Attackers try:

- many passwords repeatedly
    

Example:

```text
123456
password
admin123
dark123
```

bcrypt slows attackers significantly.

---

# 24. What is Rainbow Table Attack?

Huge precomputed table of hashes.

Without salt:

- attackers lookup hash quickly
    

Salt breaks rainbow tables.

---

# 25. Common Beginner Mistakes

---

# Mistake 1

Storing plain passwords.

VERY BAD.

---

# Mistake 2

Using low salt rounds like:

```js
1
```

---

# Mistake 3

Trying to decrypt bcrypt hashes.

Impossible/intended not to.

---

# Mistake 4

Using sync methods excessively.

Can block server.

---

# 26. bcrypt Middleware Flow in Express

```text
Signup Request
      ↓
Validation Middleware
      ↓
Controller
      ↓
bcrypt.hash()
      ↓
Store hash in DB
      ↓
Response
```

---

# Login Flow

```text
Login Request
      ↓
Validation Middleware
      ↓
Controller
      ↓
Find user from DB
      ↓
bcrypt.compare()
      ↓
Login success/failure
```

---

# 27. bcrypt in MVC Structure

---

# Controller Example

```js
exports.signup = async (req, res) => {

  const hashedPassword =
    await bcrypt.hash(req.body.password, 10);

};
```

---

# 28. Real-World Usage

bcrypt used in:

- authentication systems
    
- social media apps
    
- banking apps
    
- ecommerce apps
    
- admin panels
    

Basically:

```text
every secure login system
```

---

# 29. Question-Driven Learning For bcrypt

---

# Beginner Questions

- Why passwords should not be stored directly?
    
- What is hashing?
    
- Why bcrypt needed?
    

---

# Intermediate Questions

- What are salt rounds?
    
- Why salt important?
    
- Why compare() works?
    

---

# Advanced Questions

- Why bcrypt intentionally slow?
    
- Difference between bcrypt and argon2?
    
- How rainbow table attacks work?
    

---

# 30. Final Mental Model

MOST IMPORTANT SECTION.

Think:

```text
Password
    ↓
bcrypt adds random salt
    ↓
bcrypt hashes multiple times
    ↓
Unreadable secure hash generated
    ↓
Hash stored in DB
```

---

# Login Mental Flow

```text
Entered password
      ↓
bcrypt hashes again internally
      ↓
Compares with stored hash
      ↓
true or false
```

---

# 31. MOST IMPORTANT THINGS TO REMEMBER

---

# 1.

Never store plain passwords.

---

# 2.

bcrypt is for:

```text
password hashing
```

---

# 3.

Hashing is:

```text
one-way
```

---

# 4.

Salt adds randomness.

---

# 5.

bcrypt.compare() checks password match.

---

# 6.

bcrypt NEVER decrypts passwords.

---

# 7.

Higher salt rounds:

- slower
    
- more secure
    

---

# Final Learning Advice

To deeply understand bcrypt:

Experiment with:

- hashing same password multiple times
    
- changing salt rounds
    
- comparing passwords
    
- inspecting generated hashes
    

Observe:

- same password gives different hashes
    
- compare() still works
    

That experimentation makes bcrypt concepts permanently understandable.