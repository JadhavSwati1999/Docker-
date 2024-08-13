# Docker
Docker Introduction 
Difference Between Containers And Virtual Machines

Resource Utilization: Containers share the host operating system kernel, making them lighter and faster than VMs. VMs have a full-fledged OS and hypervisor, making them more resource-intensive.
Portability: Containers are designed to be portable and can run on any system with a compatible host operating system. VMs are less portable as they need a compatible hypervisor to run.
Security: VMs provide a higher level of security as each VM has its operating system and can be isolated from the host and other VMs. Containers provide less isolation, as they share the host operating system.
Management: Managing containers is typically easier than managing VMs, as containers are designed to be lightweight and fast-moving.
Why are Containers lightweight?
Containers are lightweight because they use a technology called containerization, which allows them to share the host operating system’s kernel and libraries, while still providing isolation for the application and its dependencies. This results in a smaller footprint compared to traditional virtual machines, as the containers do not need to include a full operating system. Additionally, Docker containers are designed to be minimal, only including what is necessary for the application to run, further reducing their size.

# What is docker?
Docker is a containerization platform that provides an easy way to containerize your applications, which means, using Docker you can build container images, run the images to create containers, and also push these containers to container registries such as Docker-Hub, Quay, and so on.

In simple words, you can understand that containerization is a concept or technology, and Docker implements Containerization.

# Docker Architecture:

The above picture, clearly indicates that Docker Deamon is the brain of Docker. If Docker Deamon is killed or stops working for some reason, Docker will also stop working.

# Docker LifeCycle
We can use the above Image as a reference to understand the lifecycle of Docker.

There are three important things,

docker build -> builds docker images from Dockerfile
docker run -> runs container from docker images
docker push -> push the container image to public/private registries to share the docker images.

Understanding the terminology
Docker daemon
The Docker daemon (dockerd) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.

Docker client
The Docker client (docker) is the primary way that many Docker users interact with Docker. When you use commands such as docker run, the client sends these commands to dockerd, which carries them out. The docker command uses the Docker API. The Docker client can communicate with more than one daemon.

Docker Desktop
Docker Desktop is an easy-to-install application for your Mac, Windows, or Linux environment that enables you to build and share containerized applications and microservices. Docker Desktop includes the Docker daemon (dockerd), the Docker client (docker), Docker Compose, Docker Content Trust, Kubernetes, and Credential Helper. For more information, see Docker Desktop.

Docker registries
A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own private registry.

Dockerfile
Dockerfile is a file where you provide the steps to build your Docker Image.

Images
An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization.

# Docker Volumes
Problem Statement
It is a very common requirement to persist the data in a Docker container beyond the lifetime of the container. However, the file system of a Docker container is deleted/removed when the container dies.

Solution
There are 2 different ways how docker solves this problem.

Volumes
Bind Directory on a host as a Mount
Volumes
Volumes aim to solve the same problem by providing a way to store data on the host file system, separate from the container’s file system so that the data can persist even if the container is deleted and recreated.


Volumes can be created and managed using the docker volume command.

Once a volume is created, you can mount it to a container using the -v or — mount option when running a docker run command.
Bind Directory on a host as a Mount
Bind mounts also aim to solve the same problem but in a completely different way.

Using this way, the user can mount a directory from the host file system into a container. Bind mounts have the same behavior as volumes, but are specified using a host path instead of a volume name.

docker run -it -v <host_path>:<container_path> <image_name> /bin/bash

Key Differences between Volumes and Bind Directory on a Host as a Mount
Volumes are managed, created, mounted, and deleted using the Docker API. However, Volumes are more flexible than bind mounts, as they can be managed and backed up separately from the host file system, and can be moved between containers and hosts.

In a nutshell, Bind Directory on a host as a Mount is appropriate for simple use cases where you need to mount a directory from the host file system into a container, while volumes are better suited for more complex use cases where you need more control over the data being persisted in the container.

Docker Networking
Networking allows containers to communicate with each other and with the host system. Containers run isolated from the host system and need a way to communicate with each other and with the host system.

By default, Docker provides two network drivers for you, the bridge and the overlay drivers.

docker network ls
Bridge Networking

The default network mode in Docker. It creates a private network between the host and containers, allowing containers to communicate with each other and with the host system.


If you want to secure your containers and isolate them from the default bridge network you can also create your own bridge network.

docker network create -d bridge my_bridge
This new network can be attached to the containers when you run these containers.

docker run -d --net=my_bridge --name db training/postgres
This way, you can run multiple containers on a single host platform where one container is attached to the default network and the other is attached to the my_bridge network.

These containers are completely isolated from their private networks and cannot talk to each other.



However, you can at any point in time, attach the first container to my_bridge network and enable communication.

docker network connect my_bridge web

Host Networking
This mode allows containers to share the host system’s network stack, providing direct access to the host system’s network.

To attach a host network to a Docker container, you can use the — network=” host” option when running a docker run command. When you use this option, the container has access to the host’s network stack and shares the host’s network namespace. This means that the container will use the same IP address and network configuration as the host.

docker run --network="host" <image_name> <command>

Keep in mind that when you use the host network, the container is less isolated from the host system, and has access to all of the host’s network resources. This can be a security risk, so use the host network with caution.

Additionally, not all Docker image and command combinations are compatible with the host network, so it’s important to check the image documentation or run the image with the — network=” bridge” option (the default network mode) first to see if there are any compatibility issues.

Overlay Networking
This mode enables communication between containers across multiple Docker host machines, allowing containers to be connected to a single network even when they are running on different hosts.

Macvlan Networking
This mode allows a container to appear on the network as a physical host rather than as a container.
