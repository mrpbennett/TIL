# String Methods

Some of these I knew already, but there are a couple here that were new too me and I also learnt about the `!` operator again...refreshing my memory on how to use it.

```javascript
let name = 'Paul Bennett';
let whitespace = '     Paul Bennett';

// checks length of a string
console.log(name.length);

// convert to uppercase
console.log(name.toUpperCase());

// convert to lowercase
console.log(name.toLowerCase());

// includes
let pwds = 'abc123xyz';
console.log(pwds.includes('abc')); // returns true

// Trim
console.log(whitespace.trim()); // trims whitespace on strings

// IsValidPassword
let isPasswordValid = (password) => {
    return password.length >= 8 && !password.includes('password');
};

console.log(isPasswordValid('abc123password')); // false
console.log(isPasswordValid('abc123Swordfish')); // true
console.log(isPasswordValid('abc123')); // false
```

The `!` operator flips the boolean value for example. The below function would only return true if `password` **DID NOT** include `password` Thanks to adding `!` before the statement.

```javascript
let isPasswordValid = (password) => {
    return password.length >= 8 && !password.includes('password');
};
console.log(isPasswordValid('abc123password')); // false
console.log(isPasswordValid('abc123Swordfish')); // true
```
