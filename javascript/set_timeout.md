# setTimeOut

Ever created a pop up message like a `flash()` in Flask and couldn't get rid? This is where `setTimeout()` comes in. In the below example, I grab the flash message by it's `id` the toggle a class of `hidden` on it after a 3 second delay.

```js
setTimeout(() => {
	flash = document.getElementById("flash");
	flash.classList.toggle("hidden");
}, 3000);
```

Comes in very handy and gives a good error message experience.
