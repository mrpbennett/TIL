# Docker build process 🐳

First create your docker file within the root of your project. Below you can see a Dockerfile in a simple form, which uses a python image, runs pipenv to install the dependencies and then copies all files in the root dir to `/app` and then runs the script.

## Dockerfile for Python using Pipenv

```Dockerfile
# select an image to use from Docker - https://hub.docker.com/_/python
FROM python:3.9.2-slim-buster

# install required modules via pipenv
COPY Pipfile Pipfile.lock ./
RUN python -m pip install --upgrade pip
RUN pip install pipenv && pipenv install --dev --system --deploy

# copy all files to /app and list it as the WORKDIR
COPY . /app
WORKDIR /app

# run the script app.py
CMD python app.py
```

Once we have the Dockerfile created it's time to build our image.

## Build the image

Include the period at the end, as this tells docker to use the root

```zsh
docker build -t <image name> .

# use this to build to a registry
docker build -t <registry>/<image name>:latest .

```
## Pushing to a registery

```zsh
docker container commit <container ID> <image name>:latest

docker push <registry>/<image name>:latest
```

## Run the image

```zsh
docker run <image name>

# use this if you're using enviroment variables
docker run -d --env-file .env <image name>
```

Using `--env-file .env` allows you to add your enviroment variables into your container.

## Check that it's running (locally)
```zsh
docker ps

-- Out
CONTAINER ID   IMAGE                   COMMAND                  CREATED          STATUS          PORTS      NAMES
7f413d44d549   sellers_json_analysis   "/usr/local/bin/guni…"   3 seconds ago    Up 2 seconds    5000/tcp   suspicious_dijkstra
```


