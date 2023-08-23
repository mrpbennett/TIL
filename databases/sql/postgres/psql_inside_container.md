# How to connect to a Postgres docker container

Following on from
[How to SSH into a Docker Container](https://www.freecodecamp.org/news/how-to-ssh-into-a-docker-container-secure-shell-vs-docker-attach/)

I learnt how to connect to Postgres with `psql` while inside a Docker container.

Check which containers are running

```bash
➜ docker container ls


CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS         PORTS                    NAMES
9b1204e89d7f   postgres:latest   "docker-entrypoint.s…"   8 minutes ago   Up 8 minutes   0.0.0.0:5050->5432/tcp   postgres_de
```

Now we need to connect to `postgres_de`

```bash
➜ docker exec -it postgres_de bash

# Now we're inside the container

root@9b1204e89d7f:/#
```

Now it's time to run `psql` as we're inside the container we don't have to worry
about the exposed port. The port `5050` is for use outside of the container. For
example connecting to the database via Datagrip or something similar.

To get things running we need to specify a post and user like so:

```bash
root@9b1204e89d7f:/# psql -p 5432 -U postgres
```

And we're in!!! We can now query the database from the command line

```bash
postgres=# SELECT * FROM some_table
```
