# Docker build process 🐳


## Build the image

```zsh
docker build -t <image name> .
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

## Pushing to a registery

```zsh
docker container commit <container ID> <image name>:latest
```
