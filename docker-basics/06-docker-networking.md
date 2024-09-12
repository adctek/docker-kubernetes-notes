# Docker Networking

## Type of Networks

Docker comes with five built-in network drivers that implement core networking functionality:
- bridge
- host
- none
- overlay
- IPvLAN
- macvlan

### Bridge

Software-based bridge between host and the containers. Containers that connect to the network can communicate with each other, but they’re isolated from those outside the network.
Because the network is bridged to your host, containers are also able to communicate on your LAN and the internet. Also, for multiple instances of the same container image, port bindings can be published on different ports in the host.

### Host

Containers that use the host network mode share the host network stack without any isolation. They aren’t allocated their own IP addresses, and port binds will be published directly to your host network interface. This means a container process that listens on port 80 will bind to <your_host_ip>:80.

### None

Networking stack of a container is completely isolated. Within the container, only the loopback device is created.

### Overlay

distributed networks that span multiple Docker hosts. The network allows all the containers running on any of the hosts to communicate with each other without requiring OS-level routing support.

### IPvLAN

Advanced driver that offers precise control over the IPv4 and IPv6 addresses assigned to your containers, as well as layer 2 and 3 VLAN tagging and routing.

This driver is useful when integrating containerized services with an existing physical network. IPvLAN networks are assigned their own interfaces, which offers performance benefits over bridge-based networking.

### Macvlan

Advanced option that allows containers to appear as physical devices on the network. It works by assigning each container in the network a unique MAC address.

## Using Docker Network

Creating a network
```bash
$ docker network create demo-network -d bridge
b7def066ed05479356f067068b2e3bfa34f141e96ccf13ea4dfa6d291aed0c21
```

Start container in a network
```bash
$ docker run -it --rm --name my-ubuntu --network demo-network ubuntu:latest
```

Connecting running container to network
```bash
$ docker network connect demo-network my-app
```

Removing container from network
```bash
$ docker network disconnect demo-network my-app
```
Using host networking
```bash
$ docker run -d --name nginx --network host nginx:latest
```
Since the container is using a host network, and NGINX listens on port 80 by default, the NGINX server is accessible on `localhost:80`, even though no ports have been explicitly bound:
```bash
$ curl localhost:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
```

Disabling container network
```bash
$ docker run -it --rm --network none my-app
/ # ping google.com
ping: bad address 'google.com'
```

## Managing Networks

List all your Docker networks

```bash
$ docker network list                         
NETWORK ID     NAME           DRIVER    SCOPE
43bf8f5df6b7   bridge         bridge    local
b7def066ed05   demo-network   bridge    local
a9ddd20e58a2   host           host      local
38672f05baf9   none           null      local
```

To delete a network, disconnect or stop all the Docker containers that use it, then pass the network’s ID or name:
```bash
$ docker network rm demo-network
```

Delete all unused networks
```bash
$ docker network prune
```
