# Theme Switching using `prefers-color-scheme`

Here is my component for switching themes but also takes the users default preference into consideration using `prefers-color-scheme` and daisyUI.

Here we use `useEffect` to detect the users preference when the component mounts.

```javascript
const prefersLightTheme = window.matchMedia('(prefers-color-scheme: light)')
const prefersDarkTheme = window.matchMedia('(prefers-color-scheme: dark)')

React.useEffect(() => {
  if (prefersLightTheme.matches) {
    document.querySelector('html').setAttribute('data-theme', 'winter')
  } else if (prefersDarkTheme.matches) {
    document.querySelector('html').setAttribute('data-theme', 'dim')
  }
}, [])
```

The conditionals set in `useEffect` will change the `html` attribute `data-theme` this is what DaisyUI uses to change the overall theme. The toggle provided then allows the user to switch back to another theme if required by using `onClick` on the `input` like so.

```jsx
input
  type="checkbox"
  value="synthwave"
  className="toggle theme-controller"
  onClick={toggleTheme}
/>
```

```jsx
import React from 'react'

export default function ThemeSwitcher() {
  const [theme, setTheme] = React.useState('winter')
  const toggleTheme = () => {
    setTheme(theme === 'dim' ? 'winter' : 'dim')

    document.querySelector('html').setAttribute('data-theme', theme)
  }

  const prefersLightTheme = window.matchMedia('(prefers-color-scheme: light)')
  const prefersDarkTheme = window.matchMedia('(prefers-color-scheme: dark)')

  React.useEffect(() => {
    if (prefersLightTheme.matches) {
      document.querySelector('html').setAttribute('data-theme', 'winter')
    } else if (prefersDarkTheme.matches) {
      document.querySelector('html').setAttribute('data-theme', 'dim')
    }
  }, [])

  return (
    <label className="flex cursor-pointer gap-2">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="20"
        height="20"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth="2"
        strokeLinecap="round"
        strokeLinejoin="round"
      >
        <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"></path>
      </svg>

      <input
        type="checkbox"
        value="synthwave"
        className="toggle theme-controller"
        onClick={toggleTheme}
      />

      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="20"
        height="20"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth="2"
        strokeLinecap="round"
        strokeLinejoin="round"
      >
        <circle cx="12" cy="12" r="5" />
        <path d="M12 1v2M12 21v2M4.2 4.2l1.4 1.4M18.4 18.4l1.4 1.4M1 12h2M21 12h2M4.2 19.8l1.4-1.4M18.4 5.6l1.4-1.4" />
      </svg>
    </label>
  )
}
```
