

---


Yes — your `server.js` **does load dotenv correctly**:

```js
import dotenv from "dotenv";
dotenv.config();
```

So the next issue is **ES module import timing**.

Even though `dotenv.config()` appears before:

```js
import app from "./app.js";
```

ES module imports are resolved/evaluated before the body of `server.js` runs. If `app.js` imports your session middleware, then `session.middleware.js` can execute **before** `dotenv.config()` has run.

### Fix

Change your `server.js` from:

```js
import dotenv from "dotenv";
dotenv.config();

import app from "./app.js";
import connectDB from "./config/db.js";
```

to:

```js
import "dotenv/config";

import app from "./app.js";
import connectDB from "./config/db.js";

const PORT = process.env.PORT || 5000;

const startServer = async () => {
  try {
    await connectDB();

    app.listen(PORT, () => {
      console.log(`Server has started as port ${PORT}`);
    });
  } catch (err) {
    console.log("Server starting failed : " + err.message);
    process.exit(1);
  }
};

startServer();
```

### Why `dotenv/config` fixes it

With:

```js
import "dotenv/config";
```

Node loads dotenv as part of the module-loading process, so your environment variables are available when the imported modules are evaluated.

Your flow is probably:

```text
server.js
   ↓
app.js
   ↓
session.middleware.js
   ↓
MongoStore.create()
   ↓
process.env.MONGO_URI
```

Previously, `process.env.MONGO_URI` could be `undefined` at the time `MongoStore.create()` executes.

With `import "dotenv/config"`:

```text
dotenv loads .env
       ↓
server.js imports app.js
       ↓
session.middleware.js
       ↓
process.env.MONGO_URI ✅
       ↓
MongoStore.create() ✅
```

### Also verify your MongoDB server

Once this is fixed, if you get a **different** error such as:

```text
MongooseServerSelectionError: connect ECONNREFUSED
```

then your environment variable is working, but MongoDB itself isn't running.

Your URI:

```text
mongodb://localhost:27017/session-auth-api
```

is valid for a MongoDB server running locally on port `27017`.

So first make the `dotenv/config` change and run:

```bash
npm run dev
```

I expect this particular `MongoStore` error to disappear.