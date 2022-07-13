# How to change the base path

An example of why you would want to change the base path. Below is how you would mount static files using FastAPI

```python
# serves static files from the /frontend/dist directory
app.mount("/fe", StaticFiles(directory="./frontend/dist"), name="frontend")
```

The files would then be hosted on `/fe` therefore the url would be `localhost:8000/fe/index.html`

Now the standard build `index.html` after running `vite build` is as follows:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/assets/favicon.17e50649.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Deal Query</title>
    <script type="module" crossorigin src="/assets/index.1cd49a68.js"></script>
    <link rel="stylesheet" href="/assets/index.30ea237b.css">
  </head>
  <body>
    <div id="root"></div>
    
  </body>
</html>
```

As you can see the `src` links don't have `/fe/` for this we need to add a new base path to our build script in `package.json`

```json
// before
"scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
},

// after
"scripts": {
    "dev": "vite",
    "build": "vite build  --base=/fe/",
    "preview": "vite preview"
},
```

See how we have added `--base=/fe/` to give us the following html output:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/fe/assets/favicon.17e50649.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Deal Query</title>
    <script type="module" crossorigin src="/fe/assets/index.1cd49a68.js"></script>
    <link rel="stylesheet" href="/fe/assets/index.30ea237b.css">
  </head>
  <body>
    <div id="root"></div>
    
  </body>
</html>
```

Now our paths are prefixed with `/fe/` lovely....

More information [here](https://vitejs.dev/guide/build.html#public-base-path)