
---

```js
const fs = require("fs");
const https = require("https");

console.log("async.js file running");

let a = 239302;
let b = 8392290;

https.get("https://dummyjson.com/products/1", (res) => {
  console.log("data fetched successfully");
});

setTimeout(() => {
  console.log("settimeout called after 5 seconds");
}, 5000);

fs.readFile("./file.txt", "utf8", (err, data) => {
  console.log("file data : " + data);
});
```