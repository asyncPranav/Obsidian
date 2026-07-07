
---


- #### xyz.js
	
```js
console.log("xyz.js file exeecuted");
```

- #### math.js
	
```js
console.log("math.js file executed");

const add = (a, b) => {
  return a + b;
};

function subtract(a, b) {
  return a - b;
}

const multiply = function (a, b) {
  return a * b;
};

export { add, subtract, multiply };
```

- #### app.js
	
```js
import { add, subtract, multiply } from "./utils/math.js";
import "./utils/xyz.js";

console.log(add(3, 5));
console.log(subtract(10, 4));
console.log(multiply(3, 7));
```

- ### Output of app.js
-





