
---

# CSRF Tokens in Express.js — Complete Beginner Notes

---

## 1. What Is CSRF? — The Attack Explained

Before understanding CSRF protection, you need to understand the attack itself. Most tutorials skip this and jump straight to the solution — that's a mistake, because if you don't understand the attack, the protection will feel like magic you just copy-paste.

**CSRF stands for Cross-Site Request Forgery.**

It is an attack where a **malicious website tricks your browser into making a request to a different website** — one where you are already logged in — **without your knowledge or consent**.

### The Real-World Analogy

Imagine you are logged into your bank at `mybank.com`. You don't log out and you open a new tab and visit a random website `evil-hacker.com`. That website has a hidden form that automatically submits to `mybank.com/transfer` with the parameters `to=hacker&amount=50000`.

Your browser has your bank's session cookie. When the hidden form submits, your browser automatically sends that cookie with the request. The bank sees a valid session cookie and thinks **you** initiated the transfer. Money gone.

You clicked nothing. You saw nothing. The attack happened silently.

---

## 2. Why Does CSRF Work? — The Root Cause

CSRF works because of two browser behaviors that work together against you:

**Behavior 1: Browsers automatically attach cookies to every request** made to a domain, regardless of which website initiated that request.

**Behavior 2: The server cannot tell the difference** between a request your own page made and a request a malicious page forced your browser to make — because both carry the same valid cookies.

```
Legitimate request:
  Your page (myapp.com) → POST /transfer → myapp.com
  Browser attaches: Cookie: sessionId=abc123
  Server sees:      valid session → processes request ✓

CSRF attack request:
  Evil page (evil.com) → POST /transfer → myapp.com
  Browser attaches: Cookie: sessionId=abc123  ← same cookie!
  Server sees:      valid session → processes request ✓ (FOOLED!)
```

The server cannot distinguish between these two requests just by looking at cookies.

---

## 3. A Step-by-Step CSRF Attack Walkthrough

Let's trace exactly how this attack works:

```
Step 1: Victim logs into myapp.com
        Server creates session, sends Set-Cookie: sid=abc123

Step 2: Browser stores cookie: sid=abc123 for domain myapp.com

Step 3: Victim does NOT log out and visits evil.com in same browser

Step 4: evil.com loads this hidden HTML:
        <form id="attack" action="https://myapp.com/transfer" method="POST">
          <input type="hidden" name="to"     value="hacker_account" />
          <input type="hidden" name="amount" value="50000" />
        </form>
        <script>document.getElementById("attack").submit();</script>

Step 5: Browser submits the form to myapp.com/transfer
        Browser AUTOMATICALLY attaches: Cookie: sid=abc123
        (This is just how browsers work — they always send cookies for the domain)

Step 6: myapp.com/transfer receives:
        - Valid session cookie sid=abc123  ← looks legitimate!
        - POST body: { to: "hacker_account", amount: "50000" }

Step 7: Server processes the transfer — it had no way to know this came from evil.com

Step 8: Attack succeeded. $50,000 transferred.
```

---

## 4. What Types of Requests Are Vulnerable?

Not all HTTP requests are equally vulnerable to CSRF:

**Vulnerable — state-changing requests:**

- `POST` — form submissions (login, transfers, purchases, account changes)
- `PUT` / `PATCH` — updating resources
- `DELETE` — deleting resources

**Not vulnerable — read-only requests:**

- `GET` — reading data (should never cause side effects — this is a REST principle)

> ⚠️ **This is why your GET routes should NEVER change state.** If you have `GET /deleteAccount` or `GET /transfer?amount=100`, you are creating unnecessary CSRF vulnerability even beyond the normal attack surface.

---

## 5. What Is a CSRF Token?

A **CSRF token** is a secret, random, unpredictable value that the server generates and embeds in every form or API request. When the form is submitted, the server checks that the submitted token matches the one it generated. Since only your own page can read this token (evil.com cannot), the attack is blocked.

The key insight is: **a CSRF token is something evil.com cannot know.**

```
Server generates a random token: "x9K2pL7mQ4nR8vT1"
Sends it to YOUR page embedded in the HTML form

Your page submits the form → includes the token
Server checks: "x9K2pL7mQ4nR8vT1" matches → ✓ legitimate request

Evil page submits the form → does NOT have the token
Server checks: token is missing/wrong → ✗ request rejected
```

Evil.com cannot steal the token because of the **Same-Origin Policy** — browser security rules prevent one website from reading the HTML or cookies of a different website.

---

## 6. How CSRF Tokens Work — The Full Flow

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         LEGITIMATE FLOW                                 │
│                                                                         │
│  1. Browser → GET /transfer-form → Server                               │
│                                                                         │
│  2. Server generates CSRF token: "abc123secret"                         │
│     Stores it in session: req.session.csrfToken = "abc123secret"        │
│     Sends HTML form with token embedded:                                │
│     <input type="hidden" name="_csrf" value="abc123secret" />           │
│                                                                         │
│  3. Browser renders the form (token is in the HTML)                     │
│                                                                         │
│  4. User fills in form and clicks Submit                                │
│     Browser → POST /transfer                                            │
│       Body: { to: "friend", amount: 100, _csrf: "abc123secret" }        │
│       Cookie: sid=xyz789                                                │
│                                                                         │
│  5. Server receives request                                             │
│     Reads token from body: "abc123secret"                               │
│     Reads token from session: "abc123secret"                            │
│     They match → ✓ Process the transfer                                 │
└─────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────┐
│                           CSRF ATTACK FLOW                              │
│                                                                         │
│  1. Attacker creates evil.com with a hidden form targeting /transfer    │
│                                                                         │
│  2. Victim visits evil.com while logged into myapp.com                  │
│                                                                         │
│  3. Evil form submits:                                                  │
│     Browser → POST /transfer                                            │
│       Body: { to: "hacker", amount: 50000 }  ← NO _csrf token!          │
│       Cookie: sid=xyz789  ← browser attaches automatically              │
│                                                                         │
│  4. Server receives request                                             │
│     Reads token from body: undefined (missing!)                         │
│     Reads token from session: "abc123secret"                            │
│     They DON'T match → ✗ Request rejected with 403 Forbidden            │
│                                                                         │
│  5. Attack defeated ✓                                                   │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 7. Where Can You Send the CSRF Token?

The CSRF token can be transmitted in three ways, depending on your app type:

### 7.1 Hidden Form Field (Traditional Server-Rendered Apps)

The most classic approach. The token is embedded as a hidden input inside every HTML form.

```html
<form method="POST" action="/transfer">
  <!-- CSRF token hidden field — submitted with the form automatically -->
  <input type="hidden" name="_csrf" value="<%= csrfToken %>" />
  <input type="text" name="amount" />
  <button type="submit">Transfer</button>
</form>
```

### 7.2 Request Header (SPA / AJAX / Fetch)

For Single Page Applications using fetch or axios, you can't embed a hidden field. Instead, expose the token in a meta tag or an API endpoint and send it as a custom request header.

```html
<!-- In your HTML <head> -->
<meta name="csrf-token" content="<%= csrfToken %>" />
```

```js
// In your frontend JavaScript
const token = document.querySelector('meta[name="csrf-token"]').getAttribute('content');

fetch('/api/transfer', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-CSRF-Token': token,  // ← send as a custom header
  },
  body: JSON.stringify({ amount: 100 }),
});
```

### 7.3 Cookie-to-Header Pattern (Double Submit Cookie)

A variation where the token is placed in both a cookie and a request header. The server verifies both match. This is how frameworks like Angular handle CSRF automatically. We will focus on the hidden form field and request header approaches as they are more common with Express.

---

## 8. The `csrf` and `csurf` Packages — What Happened

For many years, the standard way to add CSRF protection in Express was the `csurf` package. You will see it in virtually every old tutorial and Stack Overflow answer.

**However, `csurf` was deprecated in 2023.** The maintainer archived the package because it had unresolved security issues and the ecosystem had moved on.

If you run `npm install csurf` today, you will get a deprecation warning. You should not use it for new projects.

### Modern Alternatives

The two main approaches for new Express projects are:

**1. The `csrf-csrf` package** — implements the Double Submit Cookie pattern. Simple setup, works well with both traditional and SPA apps.

**2. Manual implementation** — generate the token yourself using Node's built-in `crypto` module, store it in the session, and validate it. This is simple enough to do without any library and gives you complete control.

We will cover both, starting with the manual approach (which teaches you the fundamentals) and then the `csrf-csrf` library approach.

---

## 9. Manual CSRF Token Implementation

This is the most educational approach. By doing it manually, you understand exactly what every CSRF library does internally.

```bash
npm install express express-session connect-mongo ejs dotenv
```

```js
// utils/csrf.js — CSRF utility functions

const crypto = require("crypto");

// Generate a cryptographically random CSRF token
// crypto.randomBytes gives us real randomness — not Math.random()!
function generateCsrfToken() {
  return crypto.randomBytes(32).toString("hex");
  // Returns 64 characters of random hex: "a3f8c2d1..."
}

// Middleware: ensure a CSRF token exists in the session
// If no token exists yet, create one and store it
function ensureCsrfToken(req, res, next) {
  if (!req.session.csrfToken) {
    req.session.csrfToken = generateCsrfToken();
  }
  // Make the token available to all EJS templates automatically
  // res.locals is shared with every template rendered during this request
  res.locals.csrfToken = req.session.csrfToken;
  next();
}

// Middleware: validate the CSRF token on state-changing requests
// Compares the token submitted in the request with the one in the session
function validateCsrfToken(req, res, next) {
  // Only validate POST, PUT, PATCH, DELETE — not GET, HEAD, OPTIONS
  const safeMethods = ["GET", "HEAD", "OPTIONS"];
  if (safeMethods.includes(req.method)) {
    return next(); // safe method, no CSRF validation needed
  }

  // Token can come from:
  // - req.body._csrf   → hidden form field
  // - req.headers["x-csrf-token"] → AJAX/fetch request header
  const submittedToken =
    req.body?._csrf ||
    req.headers["x-csrf-token"] ||
    req.headers["x-xsrf-token"];

  const sessionToken = req.session?.csrfToken;

  // Both tokens must exist and be equal
  if (!submittedToken || !sessionToken || submittedToken !== sessionToken) {
    return res.status(403).json({
      success: false,
      error: "CSRF token validation failed. Request blocked.",
    });
  }

  next(); // token is valid, proceed
}

module.exports = { generateCsrfToken, ensureCsrfToken, validateCsrfToken };
```

```js
// server-manual.js — Manual CSRF implementation demo

const express = require("express");
const session = require("express-session");
require("dotenv").config();

const { ensureCsrfToken, validateCsrfToken } = require("./utils/csrf");

const app = express();

app.set("view engine", "ejs");
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

app.use(session({
  secret: process.env.SESSION_SECRET || "dev-secret",
  resave: false,
  saveUninitialized: false,
  cookie: {
    httpOnly: true,
    sameSite: "strict",
    maxAge: 1000 * 60 * 60, // 1 hour
  },
}));

// Apply CSRF token generation to ALL routes
// This ensures every response has a valid token available
app.use(ensureCsrfToken);

// Apply CSRF validation to ALL routes
// (the middleware itself skips GET/HEAD/OPTIONS automatically)
app.use(validateCsrfToken);

// ─── Routes ──────────────────────────────────────────────────────────────────

// GET /form — render the form with embedded CSRF token
app.get("/form", (req, res) => {
  // res.locals.csrfToken is already set by ensureCsrfToken middleware
  // EJS template can access it as <%= csrfToken %>
  res.render("form");
});

// POST /submit — handle the form submission
// validateCsrfToken runs before this and blocks invalid requests
app.post("/submit", (req, res) => {
  const { username, message } = req.body;
  res.json({
    success: true,
    message: "Form submitted successfully! CSRF validation passed.",
    received: { username, message },
  });
});

// GET /token — expose token for AJAX requests
app.get("/csrf-token", (req, res) => {
  res.json({ csrfToken: req.session.csrfToken });
});

app.listen(3000, () => console.log("Manual CSRF demo on http://localhost:3000"));
```

```html
<!-- views/form.ejs -->
<!DOCTYPE html>
<html>
<head>
  <title>CSRF Demo</title>
</head>
<body>

  <h1>CSRF Protected Form</h1>

  <form method="POST" action="/submit">
    <!--
      The hidden _csrf field carries the token.
      csrfToken is available because ensureCsrfToken sets res.locals.csrfToken
    -->
    <input type="hidden" name="_csrf" value="<%= csrfToken %>" />

    <div>
      <label>Username:</label>
      <input type="text" name="username" />
    </div>
    <div>
      <label>Message:</label>
      <input type="text" name="message" />
    </div>
    <button type="submit">Submit</button>
  </form>

  <hr />

  <h2>AJAX Fetch Example</h2>
  <button onclick="sendAjax()">Submit via Fetch</button>

  <script>
    async function sendAjax() {
      // For AJAX: read the token from the hidden field (already on the page)
      const token = document.querySelector('input[name="_csrf"]').value;

      const response = await fetch('/submit', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'X-CSRF-Token': token,  // send as header, NOT in body for JSON
        },
        body: JSON.stringify({ username: 'pranav', message: 'hello from fetch' }),
      });

      const data = await response.json();
      console.log(data);
      alert(JSON.stringify(data));
    }
  </script>

</body>
</html>
```

---

## 10. Using the `csrf-csrf` Library (Modern Recommended Approach)

For production projects, use the well-maintained `csrf-csrf` package. It implements the **Double Submit Cookie** pattern — the token is stored in a signed cookie AND must be present in the request body or header.

```bash
npm install csrf-csrf cookie-parser
```

```js
// server-csrfcsrf.js — using the csrf-csrf library

const express = require("express");
const cookieParser = require("cookie-parser");
const { doubleCsrf } = require("csrf-csrf");
require("dotenv").config();

const app = express();

app.set("view engine", "ejs");
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

// cookie-parser is required by csrf-csrf — must come before doubleCsrf setup
app.use(cookieParser(process.env.COOKIE_SECRET || "cookie-secret-change-in-prod"));

// ─── CSRF Configuration ───────────────────────────────────────────────────────
const {
  generateToken,      // call this to get a token for embedding in forms/responses
  doubleCsrfProtection, // this is the middleware that validates requests
} = doubleCsrf({
  // The secret used to sign the CSRF cookie — must be long and random
  getSecret: () => process.env.CSRF_SECRET || "csrf-secret-change-in-prod",

  // The name of the cookie that stores the CSRF token
  cookieName: "x-csrf-token",

  // Cookie options
  cookieOptions: {
    httpOnly: true,
    sameSite: "strict",
    secure: process.env.NODE_ENV === "production",
    maxAge: 1000 * 60 * 60, // 1 hour
  },

  // The size of the generated token in bytes
  size: 64,

  // Where to look for the CSRF token in incoming requests
  // It checks these in order and uses the first one found
  getTokenFromRequest: (req) =>
    req.body?._csrf ||               // hidden form field
    req.headers["x-csrf-token"] ||   // custom AJAX header
    req.headers["x-xsrf-token"],     // Angular's default header name
});

// ─── Routes ───────────────────────────────────────────────────────────────────

// Public routes — NO CSRF protection (GET routes, public APIs)
app.get("/", (req, res) => {
  res.send("<h1>Home</h1><a href='/form'>Go to form</a>");
});

// GET /form — generate and embed the token in the form
app.get("/form", (req, res) => {
  // generateToken creates the token AND sets the CSRF cookie in the response
  const csrfToken = generateToken(req, res);

  res.render("form", { csrfToken });
});

// GET /csrf-token — endpoint for SPAs to fetch the token via AJAX
app.get("/csrf-token", (req, res) => {
  const csrfToken = generateToken(req, res);
  res.json({ csrfToken });
});

// Protected routes — APPLY doubleCsrfProtection middleware
// Any POST/PUT/PATCH/DELETE route that should be CSRF-protected goes here

app.post("/submit",
  doubleCsrfProtection,  // ← CSRF validation runs first
  (req, res) => {
    res.json({
      success: true,
      message: "CSRF validation passed!",
      received: req.body,
    });
  }
);

app.post("/transfer",
  doubleCsrfProtection,
  (req, res) => {
    const { to, amount } = req.body;
    res.json({ success: true, message: `Transferred ${amount} to ${to}` });
  }
);

// ─── CSRF Error Handler ───────────────────────────────────────────────────────
// Must come after routes — handles the error thrown by doubleCsrfProtection
app.use((err, req, res, next) => {
  if (err.code === "EBADCSRFTOKEN" || err.message === "invalid csrf token") {
    return res.status(403).json({
      success: false,
      error: "CSRF validation failed. This request has been blocked.",
    });
  }
  next(err);
});

app.listen(3000, () => console.log("csrf-csrf demo on http://localhost:3000"));
```

---

## 11. When Do You NOT Need CSRF Protection?

Understanding when CSRF protection is NOT needed is just as important as knowing when to apply it.

### APIs that use JWT in Authorization header

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR...
```

If your API authenticates using a Bearer token in the `Authorization` header (the standard JWT approach), CSRF attacks don't work. Here's why: a browser making a cross-site request cannot add arbitrary request headers. The `Authorization` header is not automatically sent — your JavaScript code has to explicitly add it. Evil.com's hidden form cannot do this.

```
CSRF attack form submission:
  POST /api/transfer
  Cookie: sid=abc123   ← browser adds this automatically
  Body: { amount: 100 }
  (No Authorization header — evil.com can't add one!)

Server checks: no Authorization header → 401 Unauthorized → attack blocked
```

### APIs using CORS properly

If your API is set up to only accept requests from your own domain via CORS headers, cross-origin requests from evil.com will be blocked at the browser level before they even reach your server. However, CORS is not a substitute for CSRF protection — it protects some cases but not all (simple requests bypass CORS preflight checks).

### `sameSite: "strict"` cookies alone

Modern browsers with `sameSite: "strict"` cookies provide strong CSRF protection for most attacks. However, not all browsers fully support this, and it doesn't protect against attacks on the same site (like a comment section that allows HTML). Defense in depth means using both `sameSite` and CSRF tokens.

```
Summary:
  Session-based auth (cookies)       → NEED CSRF protection
  JWT auth in Authorization header   → DO NOT need CSRF protection
  Both sameSite cookies + CSRF token → Maximum protection ✓
```

---

## 12. CSRF Token Rotation — Should You Rotate Per Request?

There are two strategies for how often to issue a new CSRF token:

**Per-session token (simpler):** One token is generated when the session starts and reused for the entire session. Simpler, works well for traditional server-rendered apps, no issues with the back button or multiple tabs.

**Per-request token (more secure):** A new token is generated after every request. Provides stronger protection (a stolen token is only valid for one request) but causes issues when users open multiple tabs or use the back button, because the old token is now invalid.

For most applications, a **per-session token** is the right choice. It provides very strong protection with no UX complications. Per-request tokens are only worth the complexity for very high-security applications like banking.

---

## 13. CSRF and the SameSite Cookie Attribute — Layers of Defense

`sameSite` on cookies and CSRF tokens are **complementary** protection layers, not alternatives. Use both.

```js
// In your session or auth cookie setup:
cookie: {
  httpOnly: true,
  secure:   true,
  sameSite: "strict",  // Layer 1: browser won't send cookie on cross-site requests
  maxAge:   1000 * 60 * 60 * 24,
}

// In your forms:
<input type="hidden" name="_csrf" value="<%= csrfToken %>" />
// Layer 2: server validates token even if somehow the cookie was sent
```

`sameSite: "strict"` prevents the browser from sending the cookie on cross-site requests at all — blocking most CSRF attacks before the request even reaches your server. The CSRF token is the safety net that catches cases where `sameSite` is bypassed or unsupported.

---

## 14. Common Mistakes and How to Fix Them

**Mistake 1: Not regenerating the token after login**

If you use the same CSRF token before and after login, a pre-login token is just as powerful as a post-login one. Regenerate the session (and therefore the CSRF token) after successful authentication.

```js
// After successful login:
req.session.regenerate((err) => {
  // New session = new CSRF token generated by ensureCsrfToken
  req.session.userId = user.id;
  req.session.csrfToken = generateCsrfToken(); // generate fresh token
  // ...
});
```

**Mistake 2: Putting CSRF token in a GET response body and caching it**

If your GET route response is cached (by CDN, proxy, or browser), the cached CSRF token will be reused. CSRF tokens should never be cached. Add `Cache-Control: no-store` headers to any response containing a CSRF token.

```js
app.get("/form", (req, res) => {
  res.set("Cache-Control", "no-store");  // ← prevent caching of CSRF token
  res.render("form", { csrfToken: req.session.csrfToken });
});
```

**Mistake 3: Putting the CSRF token in localStorage**

`localStorage` is readable by any JavaScript on your page. If you have an XSS vulnerability, the attacker's script can read the token from `localStorage` and use it for CSRF. Keep the token in a cookie with `httpOnly: true` or embed it directly in server-rendered HTML.

**Mistake 4: Skipping CSRF protection on "internal" or "non-sensitive" routes**

Every state-changing route is a potential target. Attackers don't just go for bank transfers — they might want to change your email, delete your posts, follow accounts, or send messages from your account. Apply CSRF protection to all POST/PUT/PATCH/DELETE routes.

**Mistake 5: Using a predictable token**

```js
// ❌ Predictable — never do this
const csrfToken = Date.now().toString();
const csrfToken = Math.random().toString();

// ✅ Cryptographically random — always do this
const csrfToken = crypto.randomBytes(32).toString("hex");
```

`Date.now()` and `Math.random()` are not cryptographically random. An attacker might be able to predict or brute-force them. Always use `crypto.randomBytes()`.

---

## 15. Real-World Example — A Complete CSRF-Protected App

This is a realistic bank-style transfer form built with Express, EJS, sessions, and CSRF protection. It demonstrates everything: the attack scenario, the protection, AJAX requests, and proper error handling.

```
Project Structure:
├── server.js
├── utils/
│   └── csrf.js
├── views/
│   ├── home.ejs
│   ├── dashboard.ejs
│   └── transfer.ejs
└── .env
```

```
# .env
PORT=3000
SESSION_SECRET=j3k9Lm2pQ8wZvRxN7cH6bT1sY4uAf5gD
NODE_ENV=development
```

```js
// utils/csrf.js

const crypto = require("crypto");

function generateCsrfToken() {
  return crypto.randomBytes(32).toString("hex");
}

// Ensures a CSRF token exists in the session and exposes it to all EJS views
function ensureCsrfToken(req, res, next) {
  if (!req.session.csrfToken) {
    req.session.csrfToken = generateCsrfToken();
  }
  res.locals.csrfToken = req.session.csrfToken; // available in every EJS template
  next();
}

// Validates the CSRF token on all non-safe HTTP methods
function validateCsrfToken(req, res, next) {
  const safeMethods = ["GET", "HEAD", "OPTIONS"];
  if (safeMethods.includes(req.method)) return next();

  const submitted =
    req.body?._csrf ||
    req.headers["x-csrf-token"] ||
    req.headers["x-xsrf-token"];

  const stored = req.session?.csrfToken;

  if (!submitted || !stored || submitted !== stored) {
    // Return HTML error for browser requests, JSON for API requests
    const wantsJSON = req.headers["accept"]?.includes("application/json") ||
                      req.headers["content-type"]?.includes("application/json");

    if (wantsJSON) {
      return res.status(403).json({
        success: false,
        error: "CSRF validation failed. Request blocked.",
      });
    }

    return res.status(403).send(`
      <h1>403 — Forbidden</h1>
      <p>CSRF token validation failed. This request has been blocked.</p>
      <p>This usually means:</p>
      <ul>
        <li>Your session expired — <a href="/">go back and try again</a></li>
        <li>The form was tampered with</li>
        <li>A CSRF attack was blocked</li>
      </ul>
    `);
  }

  next();
}

module.exports = { generateCsrfToken, ensureCsrfToken, validateCsrfToken };
```

```js
// server.js

const express = require("express");
const session = require("express-session");
require("dotenv").config();

const { ensureCsrfToken, validateCsrfToken, generateCsrfToken } = require("./utils/csrf");

const app = express();

app.set("view engine", "ejs");
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

// ─── Session Middleware ───────────────────────────────────────────────────────
app.use(session({
  name: "sessionId",
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  cookie: {
    httpOnly: true,
    secure: process.env.NODE_ENV === "production",
    sameSite: "strict",           // Layer 1 CSRF defense
    maxAge: 1000 * 60 * 60 * 2,  // 2 hours
  },
}));

// ─── CSRF Middleware (Global) ─────────────────────────────────────────────────
app.use(ensureCsrfToken);    // always ensure a token exists in session
app.use(validateCsrfToken);  // validate on every non-GET request

// ─── Simulated DB ─────────────────────────────────────────────────────────────
const users = {
  "pranav@gmail.com": {
    id: 1, name: "Pranav", password: "Secret@123", balance: 50000
  },
  "riya@gmail.com": {
    id: 2, name: "Riya", password: "Hello@456", balance: 30000
  },
};
const transactions = [];

// ─── Auth Middleware ──────────────────────────────────────────────────────────
function isAuthenticated(req, res, next) {
  if (req.session?.isLoggedIn) return next();
  res.redirect("/login");
}

// ─── PUBLIC ROUTES ────────────────────────────────────────────────────────────

// Home page
app.get("/", (req, res) => {
  res.render("home", {
    isLoggedIn: !!req.session.isLoggedIn,
    username: req.session.username || null,
  });
});

// Login form
app.get("/login", (req, res) => {
  if (req.session.isLoggedIn) return res.redirect("/dashboard");
  // csrfToken is available via res.locals (set by ensureCsrfToken)
  res.render("login", { error: null });
});

// Login POST — validateCsrfToken runs automatically before this
app.post("/login", (req, res) => {
  const { email, password } = req.body;
  const user = users[email];

  if (!user || user.password !== password) {
    return res.render("login", { error: "Invalid email or password." });
  }

  // Regenerate session after login — security best practice
  // Also generates a fresh CSRF token
  req.session.regenerate((err) => {
    if (err) return res.status(500).send("Login failed.");

    req.session.isLoggedIn = true;
    req.session.userId = user.id;
    req.session.username = user.name;
    req.session.email = email;
    req.session.csrfToken = generateCsrfToken(); // fresh token after regenerate

    req.session.save(() => res.redirect("/dashboard"));
  });
});

// Logout
app.post("/logout", isAuthenticated, (req, res) => {
  req.session.destroy(() => {
    res.clearCookie("sessionId");
    res.redirect("/");
  });
});

// ─── PROTECTED ROUTES ─────────────────────────────────────────────────────────

// Dashboard
app.get("/dashboard", isAuthenticated, (req, res) => {
  const user = users[req.session.email];
  const userTransactions = transactions.filter(
    t => t.from === req.session.username || t.to === req.session.username
  );
  res.render("dashboard", { user, transactions: userTransactions });
});

// Transfer form
app.get("/transfer", isAuthenticated, (req, res) => {
  res.set("Cache-Control", "no-store"); // never cache CSRF token pages
  const user = users[req.session.email];
  res.render("transfer", { user, error: null, success: null });
});

// Transfer POST — CSRF validation happens automatically via global middleware
app.post("/transfer", isAuthenticated, (req, res) => {
  const { to, amount } = req.body;
  const parsedAmount = parseFloat(amount);

  const sender = users[req.session.email];
  const recipientEntry = Object.entries(users).find(
    ([, u]) => u.name.toLowerCase() === to?.toLowerCase()
  );

  // Validation
  if (!recipientEntry) {
    return res.render("transfer", {
      user: sender,
      error: `User "${to}" not found.`,
      success: null,
    });
  }

  const [recipientEmail, recipient] = recipientEntry;

  if (recipient.id === sender.id) {
    return res.render("transfer", {
      user: sender,
      error: "You cannot transfer money to yourself.",
      success: null,
    });
  }

  if (isNaN(parsedAmount) || parsedAmount <= 0) {
    return res.render("transfer", {
      user: sender,
      error: "Please enter a valid positive amount.",
      success: null,
    });
  }

  if (parsedAmount > sender.balance) {
    return res.render("transfer", {
      user: sender,
      error: `Insufficient balance. Your balance is ₹${sender.balance}.`,
      success: null,
    });
  }

  // Process transfer
  sender.balance -= parsedAmount;
  recipient.balance += parsedAmount;

  transactions.push({
    id: transactions.length + 1,
    from: sender.name,
    to: recipient.name,
    amount: parsedAmount,
    timestamp: new Date().toISOString(),
  });

  const updatedSender = users[req.session.email];
  res.render("transfer", {
    user: updatedSender,
    error: null,
    success: `Successfully transferred ₹${parsedAmount} to ${recipient.name}. New balance: ₹${updatedSender.balance}`,
  });
});

// ─── AJAX ENDPOINT: Get CSRF Token ───────────────────────────────────────────
// Frontend JS can call this to get a fresh token for fetch/axios requests
app.get("/api/csrf-token", isAuthenticated, (req, res) => {
  res.json({ csrfToken: req.session.csrfToken });
});

// ─── AJAX ENDPOINT: Transfer via fetch (for SPA style) ───────────────────────
// validateCsrfToken runs automatically — expects X-CSRF-Token header
app.post("/api/transfer", isAuthenticated, express.json(), (req, res) => {
  const { to, amount } = req.body;
  const parsedAmount = parseFloat(amount);
  const sender = users[req.session.email];

  const recipientEntry = Object.entries(users).find(
    ([, u]) => u.name.toLowerCase() === to?.toLowerCase()
  );

  if (!recipientEntry || parsedAmount > sender.balance || parsedAmount <= 0) {
    return res.status(400).json({ success: false, error: "Invalid transfer request." });
  }

  const [, recipient] = recipientEntry;
  sender.balance -= parsedAmount;
  recipient.balance += parsedAmount;

  transactions.push({
    id: transactions.length + 1,
    from: sender.name,
    to: recipient.name,
    amount: parsedAmount,
    timestamp: new Date().toISOString(),
  });

  res.json({
    success: true,
    message: `Transferred ₹${parsedAmount} to ${recipient.name}`,
    newBalance: sender.balance,
  });
});

// ─── Error Handler ────────────────────────────────────────────────────────────
app.use((err, req, res, next) => {
  console.error(err);
  res.status(500).send("Internal server error.");
});

app.listen(process.env.PORT, () => {
  console.log(`CSRF demo server running on http://localhost:${process.env.PORT}`);
  console.log("");
  console.log("Test accounts:");
  console.log("  Email: pranav@gmail.com  Password: Secret@123  Balance: ₹50000");
  console.log("  Email: riya@gmail.com    Password: Hello@456   Balance: ₹30000");
});
```

```html
<!-- views/home.ejs -->
<!DOCTYPE html>
<html>
<head><title>Home — CSRF Demo</title></head>
<body>
  <h1>CSRF Protected Banking App</h1>

  <% if (isLoggedIn) { %>
    <p>Welcome back, <strong><%= username %></strong>!</p>
    <a href="/dashboard">Dashboard</a> |
    <form method="POST" action="/logout" style="display:inline">
      <input type="hidden" name="_csrf" value="<%= csrfToken %>" />
      <button type="submit">Logout</button>
    </form>
  <% } else { %>
    <a href="/login">Login</a>
  <% } %>
</body>
</html>
```

```html
<!-- views/login.ejs -->
<!DOCTYPE html>
<html>
<head><title>Login — CSRF Demo</title></head>
<body>
  <h1>Login</h1>

  <% if (error) { %>
    <p style="color:red"><%= error %></p>
  <% } %>

  <form method="POST" action="/login">
    <!--
      CSRF token embedded as hidden field.
      csrfToken is in res.locals — available in every EJS template.
    -->
    <input type="hidden" name="_csrf" value="<%= csrfToken %>" />

    <div>
      <label>Email:</label>
      <input type="email" name="email" required />
    </div>
    <div>
      <label>Password:</label>
      <input type="password" name="password" required />
    </div>
    <button type="submit">Login</button>
  </form>
</body>
</html>
```

```html
<!-- views/dashboard.ejs -->
<!DOCTYPE html>
<html>
<head><title>Dashboard — CSRF Demo</title></head>
<body>
  <h1>Dashboard</h1>
  <p>Balance: <strong>₹<%= user.balance %></strong></p>

  <a href="/transfer">Make a Transfer</a>

  <form method="POST" action="/logout" style="margin-top:20px">
    <input type="hidden" name="_csrf" value="<%= csrfToken %>" />
    <button type="submit">Logout</button>
  </form>

  <h2>Transaction History</h2>
  <% if (transactions.length === 0) { %>
    <p>No transactions yet.</p>
  <% } else { %>
    <table border="1" cellpadding="8">
      <tr><th>From</th><th>To</th><th>Amount</th><th>Time</th></tr>
      <% transactions.forEach(t => { %>
        <tr>
          <td><%= t.from %></td>
          <td><%= t.to %></td>
          <td>₹<%= t.amount %></td>
          <td><%= new Date(t.timestamp).toLocaleString() %></td>
        </tr>
      <% }) %>
    </table>
  <% } %>

  <!--
    AJAX transfer demo — sends CSRF token as X-CSRF-Token header
  -->
  <h2>AJAX Transfer (Fetch API)</h2>
  <div>
    <input type="text"   id="ajaxTo"     placeholder="Recipient name" />
    <input type="number" id="ajaxAmount" placeholder="Amount" />
    <button onclick="ajaxTransfer()">Transfer via Fetch</button>
    <p id="ajaxResult"></p>
  </div>

  <script>
    async function ajaxTransfer() {
      // The CSRF token is already in the page — read from a hidden input or fetch from API
      // For simplicity, we fetch it from the dedicated endpoint
      const tokenRes = await fetch('/api/csrf-token');
      const { csrfToken } = await tokenRes.json();

      const to     = document.getElementById('ajaxTo').value;
      const amount = document.getElementById('ajaxAmount').value;

      const response = await fetch('/api/transfer', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'X-CSRF-Token': csrfToken,   // ← CSRF token in header, not body
        },
        body: JSON.stringify({ to, amount }),
      });

      const data = await response.json();
      document.getElementById('ajaxResult').textContent =
        data.success ? data.message : `Error: ${data.error}`;
    }
  </script>
</body>
</html>
```

```html
<!-- views/transfer.ejs -->
<!DOCTYPE html>
<html>
<head><title>Transfer — CSRF Demo</title></head>
<body>
  <h1>Transfer Money</h1>
  <p>Your balance: <strong>₹<%= user.balance %></strong></p>

  <% if (error) { %>
    <p style="color:red"><%= error %></p>
  <% } %>
  <% if (success) { %>
    <p style="color:green"><%= success %></p>
  <% } %>

  <form method="POST" action="/transfer">
    <!--
      Without this hidden field, the POST will be rejected with 403.
      This is CSRF protection in action.
      Try removing this line and submitting — you will get a 403 error.
    -->
    <input type="hidden" name="_csrf" value="<%= csrfToken %>" />

    <div>
      <label>Transfer to (name):</label>
      <input type="text" name="to" required />
    </div>
    <div>
      <label>Amount (₹):</label>
      <input type="number" name="amount" min="1" required />
    </div>
    <button type="submit">Transfer</button>
  </form>

  <a href="/dashboard">← Back to Dashboard</a>

  <!--
    To manually test CSRF protection:
    1. Copy this URL: http://localhost:3000/transfer
    2. Note the _csrf value in the form
    3. Open DevTools → Network
    4. Try submitting without the _csrf field (use fetch in console):
       fetch('/transfer', { method: 'POST', body: new URLSearchParams({ to: 'Riya', amount: 100 }) })
    5. You will receive 403 — CSRF blocked!
  -->
</body>
</html>
```

---

## 16. Testing CSRF Protection — Simulating the Attack

To truly understand that the protection works, let's see what happens when a request comes in without the CSRF token. Open your browser console on any page and run:

```js
// Simulates what evil.com would try to do — submit a form without the CSRF token
fetch('http://localhost:3000/transfer', {
  method: 'POST',
  headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
  body: 'to=Riya&amount=5000',
  // Notice: no _csrf field in the body!
  // The browser will send the session cookie automatically (credentials: 'include')
  credentials: 'include',
})
.then(r => r.text())
.then(console.log);
```

**Result:** You will receive a `403 Forbidden` response with the CSRF error message. The transfer was blocked even though the session cookie was valid.

Now do the same request WITH the token:

```js
// First get the token
const tokenRes = await fetch('/api/csrf-token');
const { csrfToken } = await tokenRes.json();

// Now make the request with the token
fetch('http://localhost:3000/api/transfer', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-CSRF-Token': csrfToken,
  },
  body: JSON.stringify({ to: 'Riya', amount: 100 }),
  credentials: 'include',
})
.then(r => r.json())
.then(console.log);
```

**Result:** `200 OK` — transfer succeeds.

---

## 17. Quick Reference Cheat Sheet

```
CSRF in one sentence:
  A malicious website tricks your browser into making requests to your app
  using your user's valid cookies. CSRF tokens stop this because the
  malicious site cannot know the secret token.

When do you need it?
  ✓ Session/cookie-based auth + state-changing routes (POST/PUT/DELETE)
  ✗ JWT Bearer token auth (token is not auto-sent by browser)
  ✗ GET routes (should have no side effects)

The three-step implementation:
  1. Generate a random token → store in session
  2. Embed in every form as <input type="hidden" name="_csrf" value="..." />
     OR send as X-CSRF-Token header for AJAX
  3. On every POST/PUT/DELETE, compare submitted token with session token

Defense layers (use all three):
  1. sameSite: "strict" on cookies    → blocks most CSRF at browser level
  2. CSRF token validation             → catches what sameSite misses
  3. httpOnly: true on cookies         → blocks XSS from stealing session
```

---

> ✅ **You now have a complete, deep understanding of CSRF — the attack, the root cause, the protection mechanism, the implementation from scratch, and the modern library approach. The natural next topic is JWT authentication, which by design sidesteps the CSRF problem entirely by using Authorization headers instead of cookies — and understanding CSRF deeply makes you appreciate exactly why JWT + localStorage has its own tradeoffs (XSS vulnerability) while JWT + httpOnly cookie brings CSRF back into the picture.**