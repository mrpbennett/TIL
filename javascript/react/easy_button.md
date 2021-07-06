# Easy tailwind button

Buttons are everywhere! Creating an easy to use React button component is pretty easy with Tailwind CSS

```javascript
import React from 'react';

export const Button = ({ children, color, onClick }) => {
  return (
    <button
      className={`bg-${color}-500 text-white active:bg-${color}-600 font-bold uppercase text-sm px-6 py-3 rounded shadow hover:shadow-lg outline-none focus:outline-none mr-1 mb-1 ease-linear transition-all duration-150 hover:bg-${color}-800`}
      onClick={onClick}>
      {children}
    </button>
  );
};

export default Button;
```

The above button allows you to change the color, without creating multiple components You can go abit further by adding the ability to change the padding by adding the following:

```javascript
import React from 'react';

export const Button = ({ children, color, onClick, px, py }) => {
  return (
    <button
      className={`bg-${color}-500 text-white active:bg-${color}-600 font-bold uppercase text-sm px-${px} py-${py} rounded shadow hover:shadow-lg outline-none focus:outline-none mr-1 mb-1 ease-linear transition-all duration-150 hover:bg-${color}-800`}
      onClick={onClick}>
      {children}
    </button>
  );
};

export default Button;
```

Adding the `px-${px} py-${py}` allows us to add numeric values for setting the padding left & right `(px`) and the padding top & bottom `(py)`. Adding these gives the ability to have flexible button. There is a way to go further...

```javascript
import React from 'react';

export const Button = ({ children, color, onClick, px, py, radius }) => {
  return (
    <button
      className={`bg-${color}-500 text-white active:bg-${color}-600 font-bold uppercase text-sm px-${px} py-${py} ${radius} shadow hover:shadow-lg outline-none focus:outline-none mr-1 mb-1 ease-linear transition-all duration-150 hover:bg-${color}-800`}
      onClick={onClick}>
      {children}
    </button>
  );
};

export default Button;
```

Above we added in `${radius}` here we can pass in something like `rounded` to give us rounded corners or `rounded-full` to give the button a pill like effect. Using the above we could create a button like so:

```jsx
<Button onClick={() => console.log('clicked')} color='blue' px="6" py="3" radius="rounded">Click Me</Button>
```

One caveat with this button is that you will need to know how to use TailWind utilities. However this is all good and well, but if your style doesn't require a custom padding or radius you can leave those off.
