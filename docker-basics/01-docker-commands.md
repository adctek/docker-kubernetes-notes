# Docker commands

## General Commands

| Command | Description |
| ------- | ----------- |
| `docker -d` | Start the docker daemon |
| `docker info` | Display system-wide information |
| `docker --help` | Get help with Docker |

## Container Commands

| Command | Description |
| ------- | ----------- |
| `docker run --name <container> <image>` | Create and run container named <container> from image <image> |
| `docker run -p <host_port>:<container_port> <image>` | Run container from image and publish container’s port to the host |
| `docker run -d <name>` | Run detached container (in the background) |
| `docker start <container>` | Start container by name or ID |
| `docker stop <container>` | Stop container by name or ID |
| `docker rm <container>` | Remove container (in stopped state) |
| `docker exec -it <container> <command>` | Excecute command <command> on running container |
| `docker logs -f <container>` | Show container logs |
| `docker inspect <container>` | Inspect running container by name or ID |
| `docker ps` | List running containers |
| `docker ps --all` | List all (running and stopped) containers |
| `docker container stats` | Show resource usage |

## Image Commands

| Command | Description |
| ------- | ----------- |
| `docker images` | List images on the local computer |
| `docker build -t <image> .` | Build image from Dockerfile |
| `docker build -t <image> . –no-cache ` | Build image from Dockerfile without cache |
| `docker rmi <image>` | Delete image |
| `docker image prune` | Delete all unused images |

## Registry Commands

| Command | Description |
| ------- | ----------- |
| `docker login -u <username>` | Login into Docker |
| `docker pull <image>` | Pull image from Docker registry |
| `docker search <image>` | Search Docker registry for image |
| `docker push <username>/<image>` | Publish image to Docker registry |
