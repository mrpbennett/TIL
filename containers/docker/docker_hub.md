# Using Docker hub

I have been looking to host my own Docker registry but before I went through
that, I was looking at Docker Hub.

Two things I learnt about using Docker hub that differ to what I am use too. We
have our own reigstry at work it's hosted here: <http://registry.xxx.com/> and
out catalog is here: <https://registry.xxx.com/v2/_catalog>

I would use my work registry like so:

```bash
docker build --push -t registry.xxx.com/tam/<image_name>:<tag> .
```

But in Dockerhub the tag is the image name in my case.

```bash
docker build --push -t mrpbennett/pnfb-registry:<tag> .
```

I have creared a single repository called `pnfb-registry` as I am treating this
as my own registry of images. Therefore each tag will be now the image name,
here is the difference between the two.

**Repositories**: A repository in Docker Hub is a collection of Docker images
with the same name and various tags. Think of a repository as a higher-level
grouping that holds different versions or variants of a software application or
service. Each repository contains one or more Docker images, each identified by
a unique combination of a name and a tag.

For example, if you're working on a web application, you might have a repository
named "web-app." Inside this repository, you could have images tagged as "v1.0,"
"v2.0," and "latest," each representing a different version of your application.

**Registries**: A registry is a service that stores and distributes Docker
images. Docker Hub itself is a registry. A registry is essentially a server that
hosts Docker repositories and provides the infrastructure for image storage,
versioning, distribution, and management.

Docker Hub Registry is a public registry where you can store and share your
Docker images with the community. Other registries, such as Amazon Elastic
Container Registry (ECR) or Google Container Registry (GCR), can also be used to
store private or public images.

You can think of a registry as a larger concept that encompasses one or more
repositories. It's the system that manages the storage and access of images.
