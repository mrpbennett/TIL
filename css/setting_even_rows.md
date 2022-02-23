# Giving a background color to rows in a table

Sometimes you want to make the table stand out in terms of giving each other (even) row a different background color.

We can do this by extending out TailWind CSS file like so

```js
module.exports = {
  content: ['./src/**/*.{js,jsx,ts,tsx}'],
  theme: {
    container: true,
    extend: {
      backgroundColor: ['even'], //  <--- here
    },
  },
  plugins: [],
};
```

With the addiontion of `['even']` we can use this like so for background colors

```html
<tr class="even:bg-blue-100">
    ...
```