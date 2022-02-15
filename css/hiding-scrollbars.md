# Hiding scroll bars in Tailwind CSS

To hide scroll bars with TailWind we can add a new `@layer` to utilities classes to our `input.css`. Here we can tell the browser we dont want those ugly scroll bars


```css
@layer utilities {
  /* Chrome, Safari and Opera */
  .no-scrollbar::-webkit-scrollbar {
    display: none;
  }

  .no-scrollbar {
    -ms-overflow-style: none; /* IE and Edge */
    scrollbar-width: none; /* Firefox */
  }
}
```