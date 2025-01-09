---
tags:
  - development
  - binary
  - conversion
  - js
---

# Convert binary array to string 

```js
const binaryArray = [72, 101, 108, 108, 111, 32, 87, 111, 114, 108, 100, 33];

const result = Buffer.from(binaryArray).toString('utf-8');

console.log(result); // Output: "Hello World!"

const binaryArray2 = Array.from(Buffer.from(result, 'utf-8'));

console.log(binaryArray2); // Output: [72, 101, 108, 108, 111, 32, 87, 111, 114, 108, 100, 33]
```
