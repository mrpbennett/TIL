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
