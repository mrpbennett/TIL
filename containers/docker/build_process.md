# Docker build process 🐳

Get in the root where the Dockerfile is located.


## Build the image

```zsh
docker build -t <image name> .

docker build -t <registry>/<image name>:latest .

```
## Pushing to a registery

```zsh
docker container commit <container ID> <image name>:latest

docker push <registry>/<image name>:latest
```

## Run the image

```zsh
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

## Docker File in it's simplest form

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
