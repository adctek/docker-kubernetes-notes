# Building Images

## Container Images

A container image is a standardized package that includes all of the files, binaries, libraries, and configurations to run a container.

There are two important principles of images:

- Images are immutable. Once an image is created, it can't be modified. You can only make a new image or add changes on top of it.
- Container images are composed of layers. Each layer represented a set of file system changes that add, remove, or modify files.

## Dockerfile

A Dockerfile is a text file that list instructions for the Docker daemon to follow when building a container image. When the `docker build` command is executed, the lines in the Dockerfile are processed sequentially to assemble the image. Each line or command in the Dockerfile will generate a new layer for the final container image.

Example:

```bash
FROM python:3.12
WORKDIR /usr/local/app

# Install the application dependencies
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Copy in the source code
COPY src ./src
EXPOSE 5000

# Setup an app user so the container doesn't run as the root user
RUN useradd app
USER app

# Define ENV variable
ENV PATH=$PATH:/usr/local/app

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]
```

| Instruction | Description |
| ----------- | ----------- |
| `FROM python:3.12` | The official puthon:3.12 image is selected as the base image. All the other statements apply on top of pythin:3.12|
| `WORKDIR /usr/local/app` | The working directory is changed to /usr/local/app. Subsequent statements which use relative paths, such as the COPY instructions immediately afterwards, will be resolved to /usr/local/app inside the container. |
| `COPY requirements.txt ./` | Copy requiremets.txt file from host’s working directory. The destination path is `.`, meaning the working directory in the container |
| `RUN pip install --no-cache-dir -r requirements.txt` | Executes `pip install` inside the container’s filesystem, fetching project’s dependencies. |
| `COPY src ./src` | Copy local `src` folder to a new folder named 'src' inside the working directory in the container|
| `EXPOSE 5000` | Sets configuration on the image that indicates port 5000 would be exposed |
| `RUN useradd app` | Executes `useradd add` iside the container to create a new user `app`|
| `USER app` | Sets the default user, in this case `app`,  for all subsequent instructions |
| `ENV PATH=$PATH:/usr/local/app` | Set environment variable `PATH` that will be available within your containers |
| `CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]` | Sets the default command a container using this image will run: `uvicorn app.main:app --host "0.0.0.0" --port 8080` |


### ENTRYPOINT vs CMD

**CMD** and **ENTRYPOINT** are two Dockerfile instructions that together define the command that runs when your container starts.
Because CMD and ENTRYPOINT work in tandem, they can often be confusing to understand. However, they have different effects and exist to increase your image’s flexibility:
- **ENTRYPOINT** sets the process that’s executed when the container starts
- **CMD** is the default set of arguments that are supplied to the ENTRYPOINT process

If **ENTRYPOINT** is not specified, Docker defaults to using `/bin/sh -c` as the **ENTRYPOINT**

In the example below, the container will run `/usr/bin/my-app` at startup:
```bash
ENTRYPOINT ["/usr/bin/my-app"]
```

In this other example, the container will run `/usr/bin/my-app help` at startup:
```bash
ENTRYPOINT ["/usr/bin/my-app"]
CMD[HELP]
```

Having the two separated instrutions allows for more flexible. The image author can set the ENTRYPOINT to the binary that should be run on every invocation, then allow the user to directly execute sub-commands through `docker run`
```bash
docker run my-app version

my-app version 2.0
```


## Building, tagging and publishing

Usually images are built using a Dockerfile, by executing `docker buid` on the folder where the Dockerfile exists.

```bash
docker build .
```
If needed, the base image will be pulled, then the instructions specified in the Dockerfile are run.

The above command creates an image with no name.

---

Tagging images is the method to provide an image with a memorable name. A full image name has the following structure:
```bash
[HOST[:PORT_NUMBER]/]PATH[:TAG]
```

- _HOST_: The optional registry hostname where the image is located. If no host is specified, Docker's public registry at docker.io is used by default.
- _PORT_NUMBER_: The registry port number if a hostname is provided
- _PATH_: The path of the image, consisting of slash-separated components. For Docker Hub, the format follows [NAMESPACE/]REPOSITORY, where namespace is either a user's or organization's name. If no namespace is specified, library is used, which is the namespace for Docker Official Images.
- _TAG_: A custom, human-readable identifier that's typically used to identify different versions or variants of an image. If no tag is specified, latest is used by default.

To tag an image during a build, add the `-t` (--tag) flag:
```bash
docker build -t my-username/my-image .
```

If you've already built an image, you can add another tag to the image by using the `docker image tag` command:
```bash
docker image tag my-username/my-image another-username/another-image:v1
```
---
Once the image is built and tagged, it can be pushed to a registry using the `docker push` command:
```bash
docker push my-username/my-image
```




