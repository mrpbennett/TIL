# `indexOf()` method

The `indexOf()` method returns the first index at which a given element can be found in the array, or -1 if it is not present.

```
<array>.indexOf(<element to search for>, <index to search from>)
```

The index to seach from at. If the index is greater than or equal to the array's length, -1 is returned, which means the array will not be searched. If the provided index value is a negative number, it is taken as the offset from the end of the array. Note: if the provided index is negative, the array is still searched from front to back.

Return Value - The first index of the element in the array; -1 if not found.

```javascript
const deskItems = ['laptop', 'keyboard', 'mouse', 'screen'];

console.log(deskItems.indexOf('keyboard')); // will return 1
console.log(deskItems.indexOf('laptop', 1)); // return -1 - because I begin my search at index 1 in the array
```
