# `.forEach()` method

`.forEach()` allows us to loop over an array.

This would be handy if we wanted to grab all the items of an array and display them somehow, like a note taking app.

`arr.forEach(callback(currentValue [, index [, array]])[, thisArg])`

```javascript
const notes = [
    'Note 1: starting `node notes.js`',
    'Note 2: clean exit - waiting for changes before restart',
    'Note 3: watching extensions: js,mjs,json',
];

notes.forEach((x) => console.log(x)); // loops and prints out array
```
