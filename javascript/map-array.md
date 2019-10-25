# `.map()` array method

`.map()` method **creates a new array** with the results of calling a provided function on every element in the calling array.

`.map()` accepts a callback function as one of its arguments and an important parameter of that function is the current value of the item being processed by the function. This is a required parameter. With this parameter, you're able to modify each individual item in an array and create a new function off it.

```JavaScript
const sweetArray = [2, 3, 4, 5, 35]
const sweeterArray = sweetArray.map(sweetItem => {
    return sweetItem * 2
})

console.log(sweeterArray) // [4, 6, 8, 10, 70]
```
