# `findIndex()` method

The `findIndex()` method returns the **index** of the first element in the array **that satisfies the provided testing function**. Otherwise, it returns -1, indicating that no element passed the test.

This is a good method for search an array of objects. As `indexOf` always uses `===` which means it's not a great fit for an array of objects

```
arr.findIndex(callback( element[, index[, array]] )[, thisArg])
```

```javascript
const notes = [
    {
        title: 'Specialized Tarmac SL7',
        body: 'New race bike',
    },
    {
        title: 'VA87 Keeb',
        body: 'mechcanical keyboard for my desk',
    },
    {
        title: 'JavaScript 101',
        body: 'Learnings of JS, Node & React',
    },
];

const find = notes.findIndex(function (n) {
    return n.title === 'VA87 Keeb';
});
console.log(find); // prints 1 because VA87 Keeb is the title of the 2nd object in the array
```

The `findIndex()` method executes the `callback` function once for every index in the array until it finds the one where `callback` returns a **truthy** value.

If such an element is found, `findIndex()` immediately returns the element's index. If callback never returns a truthy value (or the array's length is 0), `findIndex()` returns `-1`.
