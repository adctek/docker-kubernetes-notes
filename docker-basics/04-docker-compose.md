# Docker Compose

Docker Compose is a tool for defining and running multi-container applications. With it it's possible to control an entire application stack, by managing services, networks, and volumes in a single, comprehensible YAML configuration file.

## Docker Compose Benefits and Uses

- Simplified control: Docker Compose allows to define and manage multi-container applications in a single YAML file.

- Rapid application development: Compose caches the configuration used to create a container. When a service that has not changed is restarted, Compose re-uses the existing containers. 

- Portability across environments: Compose supports variables in the Compose file. These variables can be used to customize composition for different environments

Compose has traditionally been focused on development and testing workflows:
- Development environments
- Automated testing environments
- Single host deployments


## The Compose File

The compose file is executed with the command:

```bash
$ docker compose up
```

The default names for Compose files are:
- `compose.yaml` (preferred)
- `compose.yml`
- `docker-compose.yaml` (for backwards compatibility)
- `docker-compose.yml` (for backwards compatibility)

If the file has a different name than the defaults, use the `-f` (`––file`) flags to specify an alternate file name:

```bash
$ docker compose -f custom-compose-file.yml up
```

### Compose File Format

We need to specify the version of the Compose file format, at least one service, and optionally volumes and networks:
```bash
version: "3"
services:
  ...
volumes:
  ...
networks:
  ...
```

Services refer to the containers’ configuration. Settings include:

- Container Image to use.
  ```bash
  services: 
    my-service:
      image: ubuntu:latest
    ...
  ```
  If the image needs to first be built
  ```bash
  services: 
    my-custom-app:
      build: /path/to/dockerfile/
    ...
  ```
  
  That creates an image with no name. To assign a name: 
  ```bash
  services: 
    my-custom-app:
      build: /path/to/dockerfile/
      image: my-project-image
    ...
  ```
  
- Networking
  
  Docker containers communicate between themselves in networks created, implicitly or through configuration, by Docker Compose. A service can communicate with another service on the same network by simply referencing it by the container name and port.
  
  To expose ports:
  ```bash
  services:
    network-example-service:
        image: karthequian/helloworld:latest
        ports:
        - "8080:80"
  ```  
  
  To define additional virtual networks to segregate containers:
  ```bash
  services:
    network-example-service:
        image: karthequian/helloworld:latest
        networks: 
        - my-shared-network
        ...
    another-service-in-the-same-network:
        image: alpine:latest
        networks: 
        - my-shared-network
        ...
    another-service-in-its-own-network:
        image: alpine:latest
        networks: 
        - my-private-network
        ...
  networks:
    my-shared-network: {}
    my-private-network: {}
  ```
  
- Dependencies
  
  To create a dependency chain between our services so that some services get loaded before (and unloaded after) other ones.
  ```bash
  services:
    kafka:
        image: wurstmeister/kafka:2.11-0.11.0.3
        depends_on:
        - zookeeper
        ...
    zookeeper:
        image: wurstmeister/zookeeper
        ...
  ```
  
- Environment Variables
  
  Define static environment variables, as well as dynamic variables, with the ${} notation:
  ```bash
  services:
    database: 
      image: "postgres:${POSTGRES_VERSION}"
      environment:
        DB: mydb
        USER: "${USER}"
    ```
  
- Volumes
  ```bash
  services:
    volumes-example-service:
        image: alpine:latest
        volumes: 
        - my-named-global-volume:/my-volumes/named-global-volume
        - /tmp:/my-volumes/host-volume
        - /home:/my-volumes/readonly-host-volume:ro
        ...
    another-volumes-example-service:
        image: alpine:latest
        volumes:
        - my-named-global-volume:/another-path/the-same-named-global-volume
        ...
  volumes:
    my-named-global-volume: 
  ```

### Compose File Example

https://github.com/dockersamples/example-voting-app/blob/main/docker-compose.yml

## Compose Commands

To start all the services defined in your compose.yaml file:
```bash
$ docker compose up
```

Compose can also run in the background as a daemon when launched with the `-d` option:
```bash
$ docker compose up -d
```

After the first time, use `start` to start the services:
```bash
$ docker compose start
```

To safely stop the active services
```bash
$ docker compose stop
```

To stop and remove the running services:
```bash
$ docker compose down 
```

To monitor the output of running containers and debug issues:
```bash
$ docker compose logs
```

To lists all the services along with their current status:
```bash
$ docker compose ps
```
