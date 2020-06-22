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

// item - is each element of the array
// index - is the position of the element within the array

// Standard function
nodes.forEach(function(item, index){
    console.log(`[${index}] => ${item}`)
})

// Arrow function
notes.forEach((item, index) => {
    console.log(`[${index}] => ${item}`)
})

// returns
[0] => Note 1: starting `node notes.js`
[1] => Note 2: clean exit - waiting for changes before restart
[2] => Note 3: watching extensions: js,mjs,json
```
