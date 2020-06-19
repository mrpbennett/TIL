# Default arguments

If a function has multple arguments but only one is passed, it will return `undefined` to get around this we can use default argument values

```javascript
const someFunc = (x, y) => {
    return x * y;
};

console.log(someFunc(10, 10)); // returns 100
console.log(someFunc(10)); // returns undefined (NaN) due to no 2nd argument
```

To get around the above we can assign a default value to an argument like so.

```javascript
const someFunc = (x, y = 10) => {
    return x * y;
};

console.log(someFunc(10, 10)); // returns 100
console.log(someFunc(10)); // returns 100 as we assigned a default value of 10
```

You can also pass `undefined` as an argument if you werent to pass the first argument

```javascript
const someFunc = (x = 10, y = 10) => {
    return x * y;
};

console.log(someFunc(undefined, 10)); // returns 100 still
```
