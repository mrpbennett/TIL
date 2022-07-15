# Using Docker compose 

Docker compose allows you to develop in a container locally and accept code changes...and much more

### Example docker-compose

The below example would be good for a FastAPI application

```docker
services:
    app:
        build: .
        container_name: <container_name>
        command: uvicorn main:app --host 0.0.0.0 --port 80 --reload
        ports:
            - 8080:80
        volumes:
            - .:/app
```

- `build:`: This is the folder we're building the container from. In this case its root.
- `command:`: This is a custom cmd with the `--reload` flag which reloads the container after code changes. The `command` replaces the `CMD` in the image.
- `ports:`: Here we're exposing port `8080` which is mapped to port `80` thats mentioned in the image
- `volumes:` This maps the root with the `/app` folder within the container. If changes are made in `root` changes will be synced to `/app`.

#### Running 

```zsh
docker-compose up --build
```

To build our container with docker-compose we use the above command with the `--build` flag.

#### Checking all files

Sometimes we may need check for changes in more files. This can be done by installed [watchfiles](https://watchfiles.helpmanual.io/)

```zsh
pip install watchfiles
```

Then append the `command` with `--reload-include *` which will check look for changes in **all** files. More information on using `uvicorn` with watch files can be found [here](https://www.uvicorn.org/settings/#reloading-with-watchfiles)