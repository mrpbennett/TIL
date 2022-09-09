# How to get the current url of a page

Sometimes we need to find the current url / location of a website. This could be
use for routing needs, conditional styling or passing it into a function. Pretty
easy when you know how...

We simply use the following off the
[`window`](https://developer.mozilla.org/en-US/docs/Web/API/Window) object. The
[`location`](https://developer.mozilla.org/en-US/docs/Web/API/Window/location)
object has the following:

```javascript
ancestorOrigins: DOMStringList {length: 0, item: function, contains: function}
assign: function()
hash: ""
host: "localhost:5173"
hostname: "localhost"
href: "http://localhost:5173/"
origin: "http://localhost:5173"
pathname: "/"
port: "5173"
protocol: "http:"
reload: function()
replace: function()
search: ""
toString: function()
valueOf: function()
Symbol(Symbol.toPrimitive): undefined
Location Prototype
```

You can see here that we need the `href` key. Therefore we would need the below
to access the current url

```javascript
window.location.href
```

We could use like the below

```javascript
const url = window.location.href

if (url === 'some-url') {
  // do something...
}
```
