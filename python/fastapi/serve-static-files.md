# How to serve static files

When using FastAPI to build a backend you will need a frontend to serve the static files. This could be React or a simple html page.

### Front end

Here we will use [Vite.js](https://vitejs.dev) and React to serve the static files. 

To add a frontend directory to your project run the following command:

```bash
> vite <name of directory>
```

Once you have that created and built your frontend you will need to build your static files by running:

```bash
> npm run build
```

This will create a `dist` directory which will hold all your static files. 

### Mounting your frontend

Now to serve your frontend static files we need to mount the directory that holds them.

```python
from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles  # add this

app = FastAPI()

app.mount("/static", StaticFiles(directory="static"), name="static")  # add this
```

**What is "Mounting"**

"Mounting" means adding a complete "independent" application in a specific path, that then takes care of handling all the sub-paths.

**Details**

The first `"/static"` refers to the sub-path this "sub-application" will be "mounted" on. So, any path that starts with `"/static"` will be handled by it.

The `directory="static"` refers to the name of the directory that contains your static files.

The `name="static"` gives it a name that can be used internally by FastAPI.

#### Example

```python
app.mount("/fe", StaticFiles(directory="./frontend/dist"), name="frontend")
```

In the above example, I have mounted the directory `./frontend/dist` on `/fe` to access this from localhost for example I will need to go to the following path. `localhost:8000/fe/index.html` 

This will statically load my Vite build files. Now all that is needed instead of having two running applications you only need one. Because once you start your FastAPI server you can just go to the `host/fe/index.html` to load your front end.