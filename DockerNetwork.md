# Docker Network
Docker networks configure communications between ```neighboring containers and external services```. Containers must be connected to a Docker network to receive any network connectivity.
The communication routes available to the container depend on the network connections it has.

There are different types of container types
- bridge
- host
- overlay
- Macvlan

## Bridge
Bridge networks create a software-based bridge between your host and the container. 
Containers connected to the network can communicate with each other.Each container in the network is assigned its own IP address. Because the network’s bridged to your host, containers are also able to communicate on your LAN and the internet.

## Host
In Docker, the "Host Network" mode allows a container to share the ```network namespace with the Docker host```. 
This means that the container directly uses the networking stack of the host system rather than getting its own isolated network stack.
```Performance Benefits:``` Because the container bypasses Docker's network abstraction layer, 
it can achieve better network performance compared to other network modes like bridge or overlay

## Overlay
Overlay networks are ```distributed networks that span multiple Docker hosts```. 
The network allows all the containers running on any of the hosts to communicate with each other without requiring OS-level routing support
Overlay networks are required when containers on different Docker hosts need to communicate directly with each other. 
These networks let you set up your own distributed environments for high availability.
