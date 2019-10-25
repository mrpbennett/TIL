# Rendering lists in React using `.map()`

JavaScript libraries like React utilize `.map()` to render items in a list. This requires JSX syntax however as `.map()` method is wrapped in mustache-like JSX syntax. Here's a good example of a React component.

```javascript
import React from "react";
import ReactDOM from "react-dom";

const names = ["john", "sean", "mike", "jean", "chris"];

const NamesList = () => (
  <div>
    <ul>{names.map(name => <li key={name}> {name} </li>)}</ul>
  </div>
);

const rootElement = document.getElementById("root");
ReactDOM.render(<NamesList />, rootElement);
```

The individual list items are rendered using `.map()` to iterate over the names array initially created
