# Chronos 

Chronos is a replacement for cron. It is a distributed and fault-tolerant scheduler that runs on top of Apache Mesos that can be used for job orchestration.

### Adding a PulsePoint Job

```bash
curl -L -H 'Content-Type: application/json' -X POST -d @./<folder>/<task>.json http://lga-chronos.pulse.prod:4400/scheduler/iso8601
```

- `<folder>` = dir where jobs are stored
- `<task>` = file name of the job

### Adding a Docker Job

A docker job takes the same format as a scheduled job or a dependency job and runs on a Docker container. To configure it, an additional container argument is required, which contains a type (required), an image (required), a network mode (optional), mounted volumes (optional), parameters (optional) and whether Mesos should always pull the latest image before executing or not (optional).

Endpoint: **/v1/scheduler/iso8601** or **/v1/scheduler/dependency**
Method: **POST**
Example: bash `curl -L -H 'Content-Type: application/json' -X POST -d '{json hash}' chronos-node:8080/v1/scheduler/iso8601`


```json
{
  "schedule": "R/2014-09-25T17:22:00Z/PT2M",
  "name": "dockerjob",
  "container": {
    "type": "DOCKER",
    "image": "libmesos/ubuntu",
    "network": "BRIDGE",
    "volumes": [
      {
        "containerPath": "/var/log/",
        "hostPath": "/logs/",
        "mode": "RW"
      }
    ]
  },
  "cpus": "0.5",
  "mem": "512",
  "fetch": [],
  "command": "while sleep 10; do date =u %T; done"
}
```

Mesos 0.22.0 added support for forcibly pulling the latest version of your Docker image before launching the task, and this behavior can be enabled in Chronos by adding the forcePullImage boolean to your container configuration.

```json
{
  "container": {
    "type": "DOCKER",
    "image": "libmesos/ubuntu",
    "forcePullImage": true
  }
}
```
Chronos will default to not doing a docker pull if the image is already found on the executing node. The alternative approach is to use versions/tags for your images.

There is also support for passing in arbitrary docker config options.

```json
{
  "container": {
    "type": "DOCKER",
    "image": "libmesos/ubuntu",
    "parameters": [
      { "key": "a-docker-option", "value": "xxx" },
      { "key": "b-docker-option", "value": "yyy" }
    ]
}
```

### Things to know.

```bash
"R/2021-04-29T18:00:00.00Z/P1D",
```

`R` without number means "repeat infinitely" then start date `2021-04-29T18:00:00.00Z` - tomorrow at 18:00. Then time interval `P1D` means 1 day, so every day it will repeat itself

```bash
"epsilon": "PT30M"
```
If chronos by some reason misses the scheduled time, then within epsilon it will be able still run the task will add the json files. Then build your Docker image (json files are not necessary for docker image) it will push the docker image and then schedule the chronos tasks with those json files

More documentation can be found [here](https://mesos.github.io/chronos/docs/api.html#adding-a-docker-job)
