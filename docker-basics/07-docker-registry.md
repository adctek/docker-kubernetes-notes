# Docker Registry

## Docker Hub

Docker provides a service - Docker Hub - for finding and sharing container images.

Docker Hub provides the following features:
- Repositories: Push and pull container images.
- Builds: Automatically build container images from GitHub and Bitbucket and push them to Docker Hub.
- Webhooks: Trigger actions after a successful push to a repository to integrate Docker Hub with other services.

## Setup Private Registry Server

You can run a Docker registry on your own server:
```bash
docker run -d -p 5000:8080 --restart=always --name registry-server registry:2
```

This command pulls the official image for the open source Docker Registry from Docker Hub.


## Using a Docker Registry

Login to the registry server (Docker Hub):
```bash
$ docker login
```

Search the Docker Hub registry by image name, username, or description:
```bash
$ ocker search alpine
NAME                         DESCRIPTION                                     STARS     OFFICIAL
alpine                       A minimal Docker image based on Alpine Linux…   10992     [OK]
grafana/alpine               Alpine Linux with ca-certificates package in…   7         
jitesoft/alpine              Alpine Linux.                                   1         
...
```

Download a container image (Alpine Linux):
```bash
$ docker pull alpine
```
You could also specify a certain image version with:

```bash
$ docker pull alpine:3.14
```

To add a container image to a registry, first tag the image with the network address and port of your Docker registry along with the image name.

By default the operation assumes Docker Hub as the registry server:
```bash
$ docker image tag my-image:latest docker-hub-account/my-image:latest
```

If running a private (or using other public) registry:
```bash
$ docker image tag my-image:latest registryserver.com:8080/repo/my-image:latest
```

Then, use the `docker push` command to add the image to the registry:
```bash
$ docker image push docker-hub-account/my-image: latest
```
```bash
$ docker image push registryserver.com:8080/repo/my-image:latest
```
