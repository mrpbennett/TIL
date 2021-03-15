# Docker build process 🐳


## Build the image

```zsh
docker build -t <image name> .
```

## Run the image

```zsh
docker run -d <image name>
```

# Check that it's running
```zsh
docker ps

-- Out
CONTAINER ID   IMAGE                   COMMAND                  CREATED          STATUS          PORTS      NAMES
7f413d44d549   sellers_json_analysis   "/usr/local/bin/guni…"   3 seconds ago    Up 2 seconds    5000/tcp   suspicious_dijkstra
```
