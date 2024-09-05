# Running Containers

## General command

`docker run [OPTIONS] image[:TAG] [COMMAND]`

### Using tags

Tags represent an image version. Check the container registry (Docker Hub) for the different tags associated with an image.

```bash
$ docker run alpine:3.19
Unable to find image 'alpine:3.19' locally
3.19: Pulling from library/alpine
46b060cc2620: Pull complete 
Digest: sha256:95c16745f100f44cf9a0939fd3f357905f845f8b6fa7d0cde0e88c9764060185
Status: Downloaded newer image for alpine:3.19
```

If no tag is specified in the 'docker run' command, it defaults to 'latest'.

```bash
$ docker run alpine
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
c6a83fedfae6: Pull complete 
Digest: sha256:0a4eaa0eecf5f8c050e5bba433f58c052be7587ee8af3e8b3910ef9ab5fbe9f5
Status: Downloaded newer image for alpine:latest
```

### Foreground and background

By default containers run in the foreground. To run a container without occupying the terminal use the `-d` (`--detach`) option. This allows to use other commands while the container is running.

The container ID will be displayed.

```bash
$ docker run -d nginx
76b8eb3a6290fd8c2a15a473e70a094a3f9a7a885e735d4fe538bdc5949ac34b

$ docker ps          
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
76b8eb3a6290   nginx     "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   80/tcp    sad_matsumoto
```

To bring a running container to the foreground use `docker attach`

```bash
$ docker attach 76b8eb3a6290
^C
2024/09/05 00:09:33 [notice] 1#1: signal 2 (SIGINT) received, exiting
```

### Interactive mode and commands

To interact with the container use the flags `-i` (`--interactive`) and `-t` (`--tty`). You can also specify a command to execute on the container when it starts.

```bash
docker run -it ubuntu /bin/bash

root@b523cec46bf6:/# cat /etc/os-release
PRETTY_NAME="Ubuntu 24.04 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
```

### Volume mount

Data stored in the container is not persistent. Removing the container also removes any data it contains. To use persistent data with containers use filesystem mounts.

```bash
$ docker run -p 8080:8080 -v /opt/jenkis_data:/var/jenkins_home jenkins/jenkins
```

The `-v` (`--volume`) flag takes two parameters:
- source: the external volume (`/data/jenkins_data`)
- target: the mount location inside the container (`/var/jenkins_home`)
