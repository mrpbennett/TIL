# `querySelector()` & `querySelectorAll()`

The Document method `querySelector()` returns the first **Element** within the document that matches the specified selector, or group of selectors. If no matches are found, null is returned.

```javascript
element = document.querySelector(selectors);
```

**selectors** - A DOMString containing one or more selectors to match. This string must be a valid CSS selector string.

The return value will be an `HTMLElement` **object** representing the first element in the document that matches your CSS selector.

### Examples

The first element in the document with the class "myclass" is returned:

```javascript
var el = document.querySelector('.myclass');
```

---

The Document method `querySelectorAll()` returns a static (not live) NodeList representing a list of the document's elements that match the specified group of selectors.

```javascript
elementList = parentNode.querySelectorAll(selectors);
```

**selectors** - A DOMString containing one or more selectors to match against. This string must be a valid CSS selector string; if it's not, a SyntaxError exception is thrown

The return value will be a non-live NodeList containing one Element object for each element that matches at least one of the specified selectors or an empty NodeList in case of no matches.

### Examples

To obtain a [NodeList](https://developer.mozilla.org/en-US/docs/Web/API/NodeList) of all of the <p> elements in the document:

```javascript
var matches = document.querySelectorAll('p');
```

Here, we get a list of `<p>` elements whose immediate parent element is a `<div>` with the class `.highlighted` and which are located inside a container whose ID is `test`.

```javascript
var container = document.querySelector('#test');
var matches = container.querySelectorAll('div.highlighted > p');
```

## More examples

Lets say we have three HTML elements with the same class and we wish to access them with say a click listener

```html
<button class="show-modal">Show modal 1</button>
<button class="show-modal">Show modal 2</button>
<button class="show-modal">Show modal 3</button>
```

Using `.querySelector('.show-modal')` is only going to get the first one. If you were to print it to the console you would get something like:

```bash
> button.show-modal
```

But if you were to use `.querySelectorAll('.show-modal`) and log it to the console you will get an array of elements.

```zsh
> NodeList(3) [button.show-modal, button.show-modal, button.show-modal]
```

Which in turns means that you're able to treat.

```js
const btnOpenModel = document.querySelectorAll('.show-modal')
```

As an array and loop over the items if needed like so, allowing us to add something like a click event listener for each button.

```js
for (let i = 0; i < btnOpenModel.length; i++) {
  btnOpenModel[i].addEventListener('click', function () {
    console.log('btn clicked');
  });
}
```