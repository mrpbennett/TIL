# `find()` method

The `find()` method returns the **value** of the **first element** in the provided array that satisfies the provided testing function.

```javascript
const array1 = [5, 12, 8, 130, 44];

const found = array1.find((element) => element > 10);

console.log(found);
// expected output: 12 - because 12 was the first element that was greater than 10
```

The find method executes the callback function once for each index of the array until the callback returns a truthy value. If so, find immediately returns the value of that element. Otherwise, find returns undefined.

callback is invoked for every index of the array, not just those with assigned values.

-   If you need the **index** of the found element in the array, use `findIndex()`.
-   If you need to find the **index of a value**, use `Array.prototype.indexOf()`. (It’s similar to `findIndex(),` but checks each element for equality with the value instead of using a testing function.)
