# Using a local environment variable

Using a local variable in Node.js is pretty straight forward. The simple syntax would be as follows:

`process.env.LOCAL_VARIABLE` which would be reading `LOCAL_VARIABLE=123456789` from your `.env` file.

All local variables are required within a `.env` file, which is where your local variables live. To get your Node application to read these variables, it's best to use something like [dotenv](https://www.npmjs.com/package/dotenv) an NPM package that allows your application to read the environment file. Once install it needs to be required in your application.

```javascript
const dotenv = require('dotenv').config();
```

[Link](https://medium.com/the-node-js-collection/making-your-node-js-work-everywhere-with-environment-variables-2da8cdf6e786)
