# Getting the current URL in React

```javascript
const url = typeof window !== 'undefined' ? window.location.href : '';
```

This will allow you to store the current url of the page. This is handy if you want to style certain objects with ternary operators. Like so

### 

```javascript
class Sidebar extends React.Component {
  ...
  render() {
    const url = typeof window !== 'undefined' ? window.location.href : '';

    return (

       <span className={`font-bold text-xl md:text-2xl ${ url === 'http://localhost:8000/' ? 'text-gray-100' : 'text-gray-900'} tracking-wider hover:underline`}>
           Menu
       </span>
```

### React Functional Component

```javascript
const Header = () => {
  const url = typeof window !== 'undefined' ? window.location.href : '';

  return (
  ...
```

### Ternary Operator

```javascript
{url === 'http://localhost:8000/' ?  'condition 1' : 'condition 2'}
```
 
