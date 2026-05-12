
---



# Cookies in Express.js — Complete Beginner Notes

---

## 1. What Is a Cookie?

A **cookie** is a small piece of text data that a server sends to a browser, and the browser stores it and automatically sends it back with every future request to that same domain.

Think of it like a **stamp on your hand** at an amusement park. When you first enter (first request), the staff stamps your hand. Every time you go on a ride (future requests), you just show your hand — you don't need to buy a ticket again. The stamp is the cookie.

```
First visit:
Browser → GET /         → Server
Browser ← Set-Cookie: username=pranav ← Server
Browser saves: username=pranav

Every future request:
Browser → GET /dashboard  (Cookie: username=pranav) → Server
Server reads the cookie and knows who you are
```

Cookies are stored in the browser, not on the server. This is the fundamental difference between cookies and sessions (sessions store data on the server and only send an ID to the browser).

---

## 2. Why Do We Need Cookies?

HTTP is **stateless** — every request the browser sends to the server is completely independent. The server has no built-in memory of previous requests. Cookies are the browser's mechanism for carrying state across requests.

Without cookies:

```
Request 1: You log in → server says "OK you're in"
Request 2: You visit /profile → server says "Who are you? Log in again."
```

With cookies:

```
Request 1: You log in → server says "OK" + sets cookie: loggedIn=true
Request 2: Browser auto-sends cookie → server reads it → "Oh it's you, welcome!"
```

---

## 3. Cookie Use Cases

Cookies are used for many different purposes in web development:

**Authentication / Session Management** — The most common use. After login, the server stores a session ID in a cookie. Every request sends this ID so the server knows who you are. This is what `express-session` uses under the hood.

**User Preferences** — Storing things like theme preference (`theme=dark`), language (`lang=en`), or timezone so the UI feels consistent across visits.

**Tracking and Analytics** — Services like Google Analytics use cookies to identify returning visitors and track behavior across pages.

**Shopping Cart** — E-commerce sites use cookies to remember what's in your cart even before you log in.

**Remember Me** — The "remember me for 30 days" checkbox on login forms works by setting a long-lived cookie.

**Flash Messages** — Short one-time messages (like "Login successful") can be briefly stored in cookies and cleared after display.

```
Use Case               Cookie Example
──────────────────────────────────────────────────
Session ID             sid=abc123xyz
Dark mode preference   theme=dark
Language               lang=hi
Cart item count        cartCount=3
Remember me token      rememberToken=7f8a9b...
```

---

## 4. Setting Up Cookie Support in Express

Express has basic cookie reading built in through the `cookie-parser` middleware. Without it, you can send cookies but cannot easily read them.

```bash
npm install cookie-parser
```

```js
const express = require("express");
const cookieParser = require("cookie-parser");

const app = express();

// For normal cookies only — no argument needed
app.use(cookieParser());

// For signed cookies — pass a secret string
// This secret is used to create and verify the HMAC signature
app.use(cookieParser("my-cookie-secret-key"));

app.listen(3000);
```

Once `cookieParser` is registered, you get two new objects on every request:

- `req.cookies` — contains all **normal (unsigned)** cookies
- `req.signedCookies` — contains all **signed** cookies (only populated when a secret is set)

---

## 5. Setting Cookies — `res.cookie()`

You set a cookie on the response using `res.cookie()`. The browser receives it in the `Set-Cookie` response header and saves it automatically.

```js
// Basic syntax:
res.cookie(name, value, options)

// name    → the cookie's key (string)
// value   → what to store (string, or object — objects get JSON stringified)
// options → optional settings that control cookie behavior
```

```js
app.get("/set", (req, res) => {
  res.cookie("username", "pranav");
  res.send("Cookie has been set!");
});
```

When the browser receives this response, it stores `username=pranav` and sends it with every future request to this domain.

---

## 6. Reading Cookies — `req.cookies`

After `cookie-parser` is set up, all cookies sent by the browser are available in `req.cookies` as a plain JavaScript object.

```js
app.get("/read", (req, res) => {
  console.log(req.cookies);
  // { username: "pranav", theme: "dark" }

  const username = req.cookies.username;

  if (!username) {
    return res.send("No cookie found.");
  }

  res.send(`Hello, ${username}!`);
});
```

---

## 7. Deleting Cookies — `res.clearCookie()`

You cannot truly "delete" a cookie from the server — the cookie lives in the browser. What you can do is tell the browser to expire the cookie immediately by setting its expiry to a past date. `res.clearCookie()` does this for you.

```js
app.get("/clear", (req, res) => {
  res.clearCookie("username");
  res.send("Cookie cleared!");
});
```

> ⚠️ If you set a cookie with specific options (like `path` or `domain`), you must pass those same options to `clearCookie()` — otherwise the browser may not match the cookie to clear.

---

## 8. Cookie Options — Controlling Cookie Behavior

The third argument to `res.cookie()` is an options object. These options are critical for both security and behavior. Let's go through every important one.

### `maxAge` — How Long the Cookie Lives (in milliseconds)

```js
// Cookie expires in 1 day
res.cookie("username", "pranav", {
  maxAge: 1000 * 60 * 60 * 24 // 24 hours in milliseconds
});

// Cookie expires in 7 days
res.cookie("theme", "dark", {
  maxAge: 1000 * 60 * 60 * 24 * 7
});
```

Without `maxAge` (or `expires`), the cookie is a **session cookie** — it is automatically deleted when the browser is closed.

### `expires` — Absolute Expiry Date

```js
// Alternative to maxAge — set an absolute Date object
res.cookie("promoSeen", "true", {
  expires: new Date("2025-12-31")
});
```

`maxAge` is generally preferred because it's relative ("expire in X milliseconds from now") which is easier to reason about than an absolute date.

### `httpOnly` — Block JavaScript Access

```js
res.cookie("sessionId", "abc123", {
  httpOnly: true // JavaScript in the browser CANNOT read this cookie
});
```

This is a critical security option. With `httpOnly: true`, the cookie is invisible to `document.cookie` in browser JavaScript. This prevents **XSS (Cross-Site Scripting)** attacks — even if a malicious script is injected into your page, it cannot steal this cookie.

**Always set `httpOnly: true` on authentication/session cookies.**

### `secure` — HTTPS Only

```js
res.cookie("sessionId", "abc123", {
  secure: true // cookie is ONLY sent over HTTPS connections
});
```

With `secure: true`, the browser will never send this cookie over an unencrypted HTTP connection, protecting it from being intercepted. Always enable this in production where you have HTTPS.

In development (where you use HTTP), setting `secure: true` will break things because the browser refuses to set or send the cookie. Use:

```js
secure: process.env.NODE_ENV === "production"
```

### `sameSite` — Cross-Site Request Protection

```js
res.cookie("sessionId", "abc123", {
  sameSite: "strict" // or "lax" or "none"
});
```

`sameSite` controls when the browser sends the cookie with cross-site requests. This is your primary defense against **CSRF (Cross-Site Request Forgery)** attacks.

`"strict"` — Cookie is only sent when the request originates from your own domain. Most secure. Can cause issues if users click links to your site from external sites (cookie won't be sent on that first navigation).

`"lax"` — Cookie is sent on top-level navigations from other sites (like clicking a link to your site) but not on embedded requests. A good balance for most apps.

`"none"` — Cookie is sent on all cross-site requests. Requires `secure: true`. Only use for specific cross-domain scenarios.

### `path` — Restrict Cookie to a URL Path

```js
// This cookie is only sent for requests to /admin and its sub-paths
res.cookie("adminToken", "xyz", {
  path: "/admin"
});
// Cookie will NOT be sent for GET /home, GET /profile, etc.
```

### `domain` — Restrict Cookie to a Domain or Subdomain

```js
// Cookie is accessible on all subdomains of example.com
res.cookie("userId", "42", {
  domain: ".example.com"
  // accessible on: app.example.com, api.example.com, etc.
});
```

### All Options Together — A Secure Production Cookie

```js
res.cookie("sessionId", "abc123xyz", {
  httpOnly:  true,                                    // no JS access
  secure:    process.env.NODE_ENV === "production",   // HTTPS only in prod
  sameSite:  "strict",                                // CSRF protection
  maxAge:    1000 * 60 * 60 * 24 * 7,                // 7 days
  path:      "/",                                     // available site-wide
});
```

---

## 9. Normal Cookies vs Signed Cookies

This is one of the most misunderstood concepts in Express cookies. Let's understand it thoroughly.

### Normal (Unsigned) Cookies

A normal cookie stores the value in plain text. Anyone who can inspect the browser's cookie storage (using DevTools or by reading `document.cookie`) can see the exact value — and worse, **change it**.

```js
// Server sets:
res.cookie("role", "user");
// Browser stores: role=user

// A malicious user opens DevTools → Application → Cookies
// Changes "user" to "admin"
// Next request sends: Cookie: role=admin
// Server reads req.cookies.role → "admin" — TRICKED!
```

Normal cookies are suitable for **non-sensitive, non-security-critical data** like UI preferences, language settings, or non-sensitive tracking.

### Signed Cookies — Tamper Detection

A signed cookie adds a **cryptographic signature** (an HMAC hash) to the value. The server uses its `secret` to create this signature. When the cookie comes back, the server re-computes the signature and compares it. If someone tampered with the value, the signature won't match and Express returns `false`.

```js
// You MUST pass a secret to cookie-parser for signed cookies to work
app.use(cookieParser("my-secret-key-change-in-production"));

// Server sets:
res.cookie("role", "user", { signed: true });
// Browser stores: role=s%3Auser.KJHSD789dsj...
//                         ^             ^
//                   "s:" prefix = signed   signature hash

// User tampers: changes to s%3Aadmin.KJHSD789dsj...
// Server reads: req.signedCookies.role → false (signature mismatch!)
// Attack defeated ✓
```

To **set** a signed cookie:

```js
res.cookie("role", "user", { signed: true });
```

To **read** a signed cookie:

```js
req.signedCookies.role // returns the value, or false if tampered
```

---

## 10. The Same-Name Conflict — A Critical Clarification

> This is a real confusion that trips up many developers. Read carefully.

### What Happens If You Use the Same Name for Both Types?

```js
res.cookie("username", "pranav");                        // normal cookie
res.cookie("username", "pranav", { signed: true });      // signed cookie
```

**The second one silently overwrites the first.** The browser stores cookies as `name=value` pairs. Both cookies have the name `"username"`, so only one can exist. The last `Set-Cookie` header wins.

### Why They Look Different in the Browser

A **normal** cookie is stored as:

```
username=asyncpranav
```

A **signed** cookie is stored as:

```
username=s%3Aasyncpranav.ABCXYZSIGNATURE
```

Even though the stored values look different, from the browser's perspective they are still two cookies with the same **name** — `username`. The browser keeps only one.

### The Fix — Always Use Different Names

```js
// ✅ Correct approach: different names for different types
res.cookie("normalUsername", "asyncpranav");
res.cookie("signedUsername", "asyncpranav", { signed: true });
```

Now you can access both independently:

```js
req.cookies.normalUsername       // "asyncpranav"
req.signedCookies.signedUsername // "asyncpranav"
```

### Summary Table

|Type|How it's stored in browser|How to access|
|---|---|---|
|Normal|`normalUsername=asyncpranav`|`req.cookies.normalUsername`|
|Signed|`signedUsername=s%3Aasyncpranav.KJHSD789dsj`|`req.signedCookies.signedUsername`|

---

## 11. Signed ≠ Encrypted — An Important Misconception

**A signed cookie is NOT encrypted.** This is one of the most common misconceptions for beginners.

Many developers assume:

> "Signed means the value is hidden or encoded."

This is wrong. Let's prove it:

```
Signed cookie stored in browser:
signedUsername=s%3Aasyncpranav.KJHSD789dsj

URL-decoded:
signedUsername=s:asyncpranav.KJHSD789dsj
               ^  ^          ^
         prefix  value      signature
```

You can clearly see `asyncpranav` in the cookie value. The signature (`KJHSD789dsj...`) is just a **hash** that proves the value hasn't been changed. It does not hide the value.

```
Purpose of signing:   Detect tampering (integrity)
Purpose of encryption: Hide the value (confidentiality)

Signed cookie:   Value is VISIBLE, but tampering is DETECTABLE
Encrypted cookie: Value is HIDDEN (not built into express — needs custom implementation)
```

**When to use signed cookies:** When you need to prevent the user from modifying the cookie value (like storing a user role or user ID), but you don't mind the user reading it.

**When you need truly hidden values:** Don't store sensitive data in cookies at all. Store it in a server-side session and only put the session ID in the cookie.

### The Tamper Detection in Action

```
1. Server sets: res.cookie("role", "user", { signed: true })
2. Browser receives: role=s%3Auser.HMAC_SIGNATURE_OF_user

3. Malicious user opens DevTools and changes:
   role=s%3Aadmin.HMAC_SIGNATURE_OF_user
   (they changed "user" to "admin" but kept the old signature)

4. Server receives this tampered cookie
5. cookie-parser computes HMAC of "admin" → completely different hash
6. Hash doesn't match the signature → tamper detected!
7. req.signedCookies.role returns false
8. Attack defeated ✓
```

---

## 12. Cookie Security Summary

|Attack|How Cookies Are Vulnerable|Defense|
|---|---|---|
|XSS (Script injection steals cookie)|JS reads `document.cookie`|`httpOnly: true`|
|MITM (Cookie intercepted over HTTP)|Cookie sent unencrypted|`secure: true`|
|CSRF (Another site makes requests with your cookie)|Browser auto-sends cookies|`sameSite: "strict"`|
|Cookie Tampering (User edits role/userId)|Plain text value is editable|`signed: true`|
|Cookie Theft (Attacker gets your session cookie)|Long-lived cookie window|Short `maxAge`, rotate tokens|

---

## 13. Cookies vs Sessions — When to Use Which

```
┌─────────────────────────────────────────────────────────────────┐
│                    USE COOKIES WHEN                             │
│                                                                 │
│  • Data is non-sensitive (theme, language, preferences)         │
│  • You need data to persist after browser is closed             │
│  • You want simple key-value storage without a session store    │
│  • Storing a "remember me" token (long-lived, signed)           │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                    USE SESSIONS WHEN                            │
│                                                                 │
│  • Data is sensitive (userId, role, cart, auth state)           │
│  • You never want the user to see or modify the stored data     │
│  • You need to invalidate a user's access immediately (logout)  │
│  • Storing large amounts of data per user                       │
└─────────────────────────────────────────────────────────────────┘
```

In practice, sessions and cookies work together: sessions use a cookie to carry the session ID, while the actual data lives on the server.

---

## 14. Real-World Example — A Complete Cookie Demo App

This example demonstrates every concept: setting normal cookies, signed cookies, reading them, deleting them, and using them for a simple "remember my theme preference" and "track visit count" feature.

```js
const express = require("express");
const cookieParser = require("cookie-parser");
require("dotenv").config();

const app = express();

// ─── Setup ────────────────────────────────────────────────────────────────────
// Pass the secret to enable signed cookie support
app.use(cookieParser(process.env.COOKIE_SECRET || "dev-secret-change-in-prod"));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

// ─── Route 1: Home — Track Visit Count with a Normal Cookie ──────────────────
// Visit count is non-sensitive, so a normal cookie is fine
app.get("/", (req, res) => {
  // Read existing visit count cookie, default to 0 if not found
  let visits = parseInt(req.cookies.visitCount) || 0;
  visits++;

  // Update the cookie with the new count
  // maxAge: 30 days — persists even after browser is closed
  res.cookie("visitCount", visits, {
    maxAge: 1000 * 60 * 60 * 24 * 30, // 30 days
    httpOnly: false, // intentionally readable by JS (non-sensitive data)
  });

  res.json({
    message: visits === 1
      ? "Welcome! This is your first visit."
      : `Welcome back! You have visited ${visits} times.`,
    visitCount: visits,
  });
});

// ─── Route 2: Set Theme Preference — Normal Cookie ────────────────────────────
// Stores UI preference. Non-sensitive — normal cookie is appropriate.
app.post("/set-theme", (req, res) => {
  const { theme } = req.body;

  const allowedThemes = ["light", "dark", "system"];
  if (!allowedThemes.includes(theme)) {
    return res.status(400).json({ error: "Invalid theme. Choose light, dark, or system." });
  }

  res.cookie("theme", theme, {
    maxAge: 1000 * 60 * 60 * 24 * 365, // 1 year — preference should persist
    httpOnly: false, // frontend JS needs to read this to apply the theme
    sameSite: "lax",
  });

  res.json({ message: `Theme set to "${theme}"`, theme });
});

// ─── Route 3: Get Current Theme ───────────────────────────────────────────────
app.get("/get-theme", (req, res) => {
  const theme = req.cookies.theme || "system"; // default to "system" if not set
  res.json({ theme });
});

// ─── Route 4: Set a Signed Cookie (User Role) ─────────────────────────────────
// Role is security-relevant — we use a signed cookie to prevent tampering
// Important: use a DIFFERENT name than any normal cookie to avoid conflicts
app.post("/set-role", (req, res) => {
  const { role } = req.body;

  const allowedRoles = ["user", "editor", "admin"];
  if (!allowedRoles.includes(role)) {
    return res.status(400).json({ error: "Invalid role." });
  }

  // Setting a SIGNED cookie — note the different name "signedRole"
  // Not using "role" to avoid any conflict with a hypothetical plain "role" cookie
  res.cookie("signedRole", role, {
    signed: true,           // ← this is what makes it signed
    httpOnly: true,         // JS cannot read it
    sameSite: "strict",     // CSRF protection
    maxAge: 1000 * 60 * 60, // 1 hour
  });

  res.json({
    message: `Role "${role}" saved as signed cookie`,
    note: "The cookie value is visible in browser but tamper-proof",
  });
});

// ─── Route 5: Read the Signed Cookie ─────────────────────────────────────────
app.get("/get-role", (req, res) => {
  // Signed cookies are in req.signedCookies, NOT req.cookies
  const role = req.signedCookies.signedRole;

  // If the cookie was tampered with, role will be false (not a string)
  if (role === false) {
    return res.status(403).json({
      error: "Cookie tampering detected! Signature verification failed.",
    });
  }

  if (!role) {
    return res.status(404).json({ message: "No role cookie found. Set one first." });
  }

  res.json({
    message: "Signed cookie read successfully",
    role,
    isVerified: true, // if we got here, signature passed
  });
});

// ─── Route 6: Demonstrate Both Cookie Types Side By Side ──────────────────────
// This route sets both — uses DIFFERENT NAMES to avoid the overwrite conflict
app.get("/set-both", (req, res) => {

  // Normal cookie — plain text, readable, but no tamper detection
  res.cookie("normalUsername", "asyncpranav", {
    maxAge: 1000 * 60 * 60,
    httpOnly: false, // visible to JS
  });

  // Signed cookie — has HMAC signature, tamper-proof
  res.cookie("signedUsername", "asyncpranav", {
    signed: true,
    maxAge: 1000 * 60 * 60,
    httpOnly: true,
  });

  res.json({
    message: "Both cookies set! Open DevTools → Application → Cookies to see the difference.",
    whatToLookFor: {
      normalUsername: "asyncpranav                           ← plain text",
      signedUsername: "s%3Aasyncpranav.KJHSD789...          ← has signature",
    },
    howToAccess: {
      normal: "req.cookies.normalUsername",
      signed: "req.signedCookies.signedUsername",
    },
  });
});

// ─── Route 7: Read Both Cookies ───────────────────────────────────────────────
app.get("/read-both", (req, res) => {
  const normalUsername = req.cookies.normalUsername;
  const signedUsername = req.signedCookies.signedUsername;

  res.json({
    normalCookie: {
      value: normalUsername || "Not set",
      source: "req.cookies",
    },
    signedCookie: {
      value: signedUsername === false ? "TAMPERED! (false returned)" : (signedUsername || "Not set"),
      source: "req.signedCookies",
      wasVerified: signedUsername !== false && !!signedUsername,
    },
  });
});

// ─── Route 8: Clear a Specific Cookie ────────────────────────────────────────
app.delete("/clear/:name", (req, res) => {
  const cookieName = req.params.name;

  res.clearCookie(cookieName);

  res.json({ message: `Cookie "${cookieName}" has been cleared.` });
});

// ─── Route 9: Clear ALL Cookies ──────────────────────────────────────────────
app.delete("/clear-all", (req, res) => {
  // Read all cookie names and clear each one
  const allCookieNames = Object.keys(req.cookies);

  allCookieNames.forEach(name => res.clearCookie(name));

  res.json({
    message: "All cookies cleared.",
    clearedCookies: allCookieNames,
  });
});

// ─── Route 10: Cookie Inspector — See Everything ─────────────────────────────
// Development-only route for learning purposes — remove in production!
app.get("/inspect", (req, res) => {
  res.json({
    normalCookies: req.cookies,           // all unsigned cookies as key-value pairs
    signedCookies: req.signedCookies,     // all signed cookies (or false if tampered)
    cookieHeader: req.headers.cookie,     // the raw Cookie header as a string
  });
});

// ─── Start ────────────────────────────────────────────────────────────────────
app.listen(3000, () => {
  console.log("Cookie demo server running on http://localhost:3000");
  console.log("");
  console.log("Routes to test:");
  console.log("  GET    /                → visit counter (normal cookie)");
  console.log("  POST   /set-theme       → set theme preference");
  console.log("  GET    /get-theme       → read theme preference");
  console.log("  POST   /set-role        → set signed role cookie");
  console.log("  GET    /get-role        → read & verify signed cookie");
  console.log("  GET    /set-both        → set both types, different names");
  console.log("  GET    /read-both       → read both types");
  console.log("  DELETE /clear/:name     → clear a specific cookie");
  console.log("  DELETE /clear-all       → clear all cookies");
  console.log("  GET    /inspect         → see all current cookies");
});
```

---

## 15. Testing This App — A Guided Exploration

Follow these steps in order in Postman (or your browser) to see every concept in action:

**Step 1:** `GET /` — Visit a few times and watch `visitCount` increment.

**Step 2:** `POST /set-theme` with body `{ "theme": "dark" }` — Sets a preference cookie.

**Step 3:** `GET /inspect` — See all your cookies in one place. Note the plain text values.

**Step 4:** `GET /set-both` — Sets both `normalUsername` and `signedUsername`.

**Step 5:** Open browser DevTools → Application → Cookies. Observe:

- `normalUsername` → `asyncpranav` (plain)
- `signedUsername` → `s%3Aasyncpranav.KJHSD789...` (with signature)

**Step 6:** Manually edit `signedUsername` in DevTools — change `asyncpranav` to `admin`.

**Step 7:** `GET /read-both` — See that `signedCookies.signedUsername` returns `false`. Tamper detected!

**Step 8:** `POST /set-role` with body `{ "role": "user" }`.

**Step 9:** `GET /get-role` — See the verified role.

**Step 10:** `DELETE /clear-all` — Clean up everything.

---

## 16. Quick Reference Cheat Sheet

```js
// ── Setup ────────────────────────────────────────────────────
const cookieParser = require("cookie-parser");
app.use(cookieParser("your-secret")); // secret enables signed cookies

// ── Set Cookies ──────────────────────────────────────────────
res.cookie("name", "value");                          // basic normal cookie
res.cookie("name", "value", { signed: true });        // signed cookie
res.cookie("name", "value", {                         // production-ready cookie
  httpOnly: true,
  secure: process.env.NODE_ENV === "production",
  sameSite: "strict",
  maxAge: 1000 * 60 * 60 * 24,
});

// ── Read Cookies ─────────────────────────────────────────────
req.cookies.name                    // read a normal cookie
req.signedCookies.name              // read a signed cookie (false if tampered)
req.headers.cookie                  // raw Cookie header string

// ── Delete Cookies ───────────────────────────────────────────
res.clearCookie("name");            // clear a specific cookie
res.clearCookie("name", { path: "/admin" }); // must match original options

// ── Cookie Options ───────────────────────────────────────────
// maxAge     → lifetime in ms (relative)
// expires    → absolute expiry Date
// httpOnly   → hide from browser JS (XSS protection)
// secure     → HTTPS only
// sameSite   → "strict" | "lax" | "none" (CSRF protection)
// signed     → add HMAC signature (tamper detection)
// path       → restrict to URL path
// domain     → restrict to domain/subdomain
```

---

> ✅ **You now have a complete understanding of cookies in Express — what they are, why they exist, how to set/read/clear them, the difference between normal and signed cookies, why same-name cookies conflict and how to avoid it, and the critical clarification that signed ≠ encrypted. The natural next topic to connect with this is JWT authentication, where tokens are often stored in `httpOnly` cookies rather than `localStorage` for better security.**