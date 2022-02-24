# Checking Arrays

This will always return false because nothing is equal to `[]`

```js
const someArray = []

if (someArray === []) console.log(true)  // false
```

The way to do this would be to check it's length like so:

```js
const someArray = []

if (someArray.length === 0) console.log(true) // true
```