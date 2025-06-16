
---

### ✅ Manual reverse of an array

```js
let arr = [1, 2, 3];
let rev = arr.reduceRight((acc, val) => {
  acc.push(val);
  return acc;
}, []);
console.log(rev); // [3, 2, 1]

```
