
---
# 🔐 Node.js `crypto` Module – Complete Detailed Notes

---

## 1️⃣ What is the `crypto` module?

The **`crypto` module** is a **built-in core module** in Node.js that provides **cryptographic functionalities**.

It helps in:

- Password hashing
    
- Data encryption & decryption
    
- Generating random values
    
- Creating hashes (SHA, MD5, etc.)
    
- Secure authentication systems
    

---

## 2️⃣ Why do we need cryptography?

Without crypto:

- Passwords can be stolen
    
- Data can be modified
    
- Communication is unsafe
    

📌 Crypto ensures:

- **Confidentiality** (no one can read data)
    
- **Integrity** (data not changed)
    
- **Authentication** (verify identity)
    

---

## 3️⃣ Importing crypto module

```js
const crypto = require("crypto");
```

No installation needed.

---

## 4️⃣ Important Crypto Concepts (VERY IMPORTANT)

Before methods, understand these terms 👇

---

### 🔹 Hashing

- One-way process
    
- Cannot be reversed
    
- Same input → same output
    

📌 Used for passwords

---

### 🔹 Encryption

- Two-way process
    
- Can encrypt & decrypt
    
- Needs a key
    

📌 Used for secure data storage

---

### 🔹 Salt

- Random value added to password
    
- Prevents rainbow table attacks
    

---

### 🔹 Cipher

- Algorithm used for encryption/decryption
    

---

### 🔹 Digest

- Final output of hash
    

---

# 🔥 CRYPTO MODULE METHODS (IN DEPTH)

---

## 🔹 1. `crypto.createHash()`

### 🧠 What it does

Creates a **hash object** using a hashing algorithm.

---

### Syntax

```js
crypto.createHash(algorithm)
```

Common algorithms:

- `sha256`
    
- `sha512`
    
- `md5` ❌ (not secure)
    

---

### Example

```js
const crypto = require("crypto");

const hash = crypto.createHash("sha256");
hash.update("password123");
const result = hash.digest("hex");

console.log(result);
```

### Output (example)

```sh
ef92b778bafe771e89245b89ecbc08a44a4e166c06659911881f383d4473e94f
```

---

### ✅ Use case

- Password hashing
    
- File integrity checks
    

---

## 🔹 2. `crypto.createHmac()`

### 🧠 What it does

Creates a **keyed hash** (Hash + Secret key).

---

### Syntax

```js
crypto.createHmac(algorithm, secretKey)
```

---

### Example

```js
const hmac = crypto.createHmac("sha256", "secret-key");
hmac.update("hello");
console.log(hmac.digest("hex"));
```

---

### ✅ Use case

- API authentication
    
- Secure tokens
    

---

## 🔹 3. `crypto.randomBytes()`

### 🧠 What it does

Generates **cryptographically secure random data**.

---

### Example

```js
crypto.randomBytes(16, (err, buffer) => {
  console.log(buffer.toString("hex"));
});
```

### Output

```sh
9f86d081884c7d659a2feaa0c55ad015
```

---

### Sync version

```js
const buf = crypto.randomBytes(8);
console.log(buf.toString("hex"));
```

---

### ✅ Use case

- Session IDs
    
- Tokens
    
- Salts
    

---

## 🔹 4. `crypto.pbkdf2()` ⭐ VERY IMPORTANT

### 🧠 What it does

Hashes passwords using **Password-Based Key Derivation Function**.

This is **slow on purpose** → more secure.

---

### Syntax

```js
crypto.pbkdf2(password, salt, iterations, keylen, digest, callback)
```

---

### Example (Async)

```js
crypto.pbkdf2("mypassword", "salt123", 100000, 64, "sha512",
  (err, derivedKey) => {
    console.log(derivedKey.toString("hex"));
});
```

---

### Sync version

```js
const key = crypto.pbkdf2Sync(
  "mypassword",
  "salt123",
  100000,
  64,
  "sha512"
);

console.log(key.toString("hex"));
```

---

### Output

```sh
a5d7c9c2b1f4...
```

---

### ✅ Use case

- Password storage
    
- Login systems
    

📌 **Never store raw passwords**

---

## 🔹 5. `crypto.createCipheriv()` (Encryption)

### 🧠 What it does

Encrypts data using a **key & IV**.

---

### Example

`const algorithm = "aes-256-cbc"; const key = crypto.randomBytes(32); const iv = crypto.randomBytes(16);  const cipher = crypto.createCipheriv(algorithm, key, iv); let encrypted = cipher.update("Secret Message", "utf8", "hex"); encrypted += cipher.final("hex");  console.log(encrypted);`

---

### ✅ Use case

- Secure data storage
    
- Sensitive information
    

---

## 🔹 6. `crypto.createDecipheriv()` (Decryption)

### 🧠 What it does

Decrypts encrypted data.

---

### Example

`const decipher = crypto.createDecipheriv(algorithm, key, iv); let decrypted = decipher.update(encrypted, "hex", "utf8"); decrypted += decipher.final("utf8");  console.log(decrypted);`

### Output

`Secret Message`

---

## 🔹 7. `crypto.getHashes()`

### 🧠 What it does

Returns supported hash algorithms.

---

### Example

`console.log(crypto.getHashes());`

---

### ✅ Use case

- Compatibility checks
    

---

## 🔹 8. `crypto.timingSafeEqual()`

### 🧠 What it does

Compares two values securely (prevents timing attacks).

---

### Example

`const a = Buffer.from("abc"); const b = Buffer.from("abc");  console.log(crypto.timingSafeEqual(a, b));`

### Output

`true`

---

### ✅ Use case

- Secure password comparison
    

---

## 🔹 9. `crypto.scrypt()` (Modern Password Hashing)

### 🧠 What it does

Memory-hard password hashing algorithm.

---

### Example

`crypto.scrypt("password", "salt", 64, (err, key) => {   console.log(key.toString("hex")); });`

---

### ✅ Use case

- Modern authentication systems
    

---

## 🔹 10. `crypto.createSign()` & `crypto.createVerify()`

### 🧠 What it does

Used for **digital signatures**.

---

### Example (Simple)

`const sign = crypto.createSign("SHA256"); sign.update("important data"); const signature = sign.sign(privateKey, "hex");`

---

### ✅ Use case

- JWT
    
- Secure APIs
    
- Blockchain
    

---

# 🧪 REAL-WORLD MINI PROJECT: Password Hashing

`const crypto = require("crypto");  function hashPassword(password) {   const salt = crypto.randomBytes(16).toString("hex");   const hash = crypto.pbkdf2Sync(password, salt, 100000, 64, "sha512")                     .toString("hex");   return { salt, hash }; }  function verifyPassword(password, salt, hash) {   const verifyHash = crypto.pbkdf2Sync(password, salt, 100000, 64, "sha512")                           .toString("hex");   return verifyHash === hash; }  const user = hashPassword("mypassword"); console.log(user); console.log(verifyPassword("mypassword", user.salt, user.hash));`

---

# 🧠 INTERVIEW GOLD POINTS

- `crypto` provides **security primitives**
    
- Hashing ≠ Encryption
    
- `pbkdf2` & `scrypt` preferred for passwords
    
- Sync crypto blocks event loop ❌
    
- Use async crypto in production ✅
    

---

# 🔁 QUICK REVISION TABLE

|Method|Purpose|
|---|---|
|createHash|Hash data|
|createHmac|Keyed hash|
|randomBytes|Secure random|
|pbkdf2|Password hashing|
|scrypt|Modern password hash|
|createCipheriv|Encrypt|
|createDecipheriv|Decrypt|
|timingSafeEqual|Secure compare|
|getHashes|Supported hashes|

---

# 🏁 FINAL MEMORY LINE

> **crypto = security backbone of Node.js**