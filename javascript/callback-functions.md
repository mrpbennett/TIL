# What is a Callback?

Simply put: A callback is a function that is to be executed after another function has finished executing — hence the name ‘call back’. For example:

The `.filter` function takes one argument that is a "callback" function.

```JavaScript
const dogs = animals.filter((animals) => {
    return animals.species === 'dog'
})
```


More complexly put: In JavaScript, functions are objects. Because of this, functions can take functions as arguments, and can be returned by other functions. Functions that do this are called **higher-order functions**. Any function that is passed as an argument is called a **callback function**.

[Link](https://codeburst.io/javascript-what-the-heck-is-a-callback-aba4da2deced)
