# Key Down Events

Adding `keydown` events allows you to give the user ability to move around the website or action an item on key press

```js
document.addEventListener('keydown', function (e) {
  console.log(e.key);
});
```

The above will print to the console all the key press from the user. This will allow you to do some logic on the `Escape` key for example. Like closing down a modal.