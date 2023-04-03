# How to add GA4 into React

First we need to install the following packages

- [`react-router-dom`](https://www.npmjs.com/package/react-router-dom)
- [`react-ga4`](https://www.npmjs.com/package/react-ga4)

These two will allow us to implement Google Analytics into our React App. 

Inside a `utils` / `modules` directory within your `src` add the following:

```javascript
// ga.js

import ReactGA from 'react-ga4'

// initialize google analytics
ReactGA.initialize('G-XXX')

// custom pageview with the location from react router
export const pageView = path => {
  return ReactGA.send({hitType: 'pageview', page: path})
}

// custom event with label being an optional parameter
export const customEvent = (category, action, label = '') => {
  return ReactGA.event({
    category: category,
    action: action,
    label: label,
  })
}
```

This allows us to import a simple `PageView` function and also a `customEvent` function, inside anywhere in our app. We then call those functions like so...


```javascript
import {useLocation} from 'react-router-dom'
import {pageView} from './utils/ga'

useEffect(() => {
    let location = useLocation()
    pageView(location.pathname)
  }, [])
```
