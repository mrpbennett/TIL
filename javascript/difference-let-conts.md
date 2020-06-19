# The difference between `let` & `const`

`const` is almost exactly the same as `let`. However, the only difference is that once you’ve assigned a value to a variable using `const`, you can’t reassign it to a new value.

```javascript
const person = {
    age: 35,
};

person.age = 28;
person = {}; // this will throw an err because we're trying to change the value of person

let person = {
    age: 35,
};

person = {}; // will work as we can change a let value
```
