# Docker

Official Documentation: <https://docs.docker.com/>

## Docker Fundamentals

The **daemon** creates and manages Docker objects, such as images, containers, networks, and volumes. Docker architecture. Docker uses a **client-server architecture**. The Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing your Docker containers.

Type: `docker version` for checking the docker version for the client and the server.

Type: `docker info` useful to check how many containers, images and volumes there are.

Docker command line structure:  
`docker <command> <sub-command> (options)`

### Images vs Containers

- Images are the binaries, libraries and source code of the application we want to run.
- A container is an instance of that image running as a process

### Docker container run

Gives the container a virtual IP on a private network inside docker engine.  
Opens up prot 80 on host and forwards to port 80 in container

- To run a new container: `docker container run -d -p 80:80 --name webhost nginx:latest`
  - `p` = `--publish` **PORTS**: 8080:80 [HOST PORT] : [CONTAINER PORT]
  - `d` = `--detach` allows the container to be running in the background
  - `--name` to assign a name to the container (if not specified, random name)
- `docker container stop [container's ID or name]` to stop running the container
- `docker container top [container's ID or name]` to check the processes running inside the container
- `docker rm -f [container's ID or name]` to stop and remove the container

### Containers vs VM

- Containers aren't mini-VM's
  - They are just processes
  - Limited to what resources they can access
  - Exit when process tops

### What's going on inside containers

- `docker container top` : process list in one container
- `docker container inspect` : details of one container config
- `docker container stats` : performance stats for all containers

### Getting a Shell inside Containers

- `docker container run -ti` : start new container interactively
- `docker container exec -ti` : run additional command in existing container

For example to run a ubuntu container: `run container --name ubuntu -ti ubuntu`
`$ exit`  
To start again the container: `run container start -ai ubuntu`

### Docker Networks

- To quick check port: `docker container port <container>`
- Each container connected to a private virtual network "bridge"
- Each virtual network routes through NAT firewall on host IP
- All container on a virtual network can talk to each other without -p
- Best practice is to create a new virtual network for each app:
  - network "my_web_app" for mysql and php/apache containers
  - network "my_apy" for mongo and nodejs containers
- To check the IP Address:  
  `docker container inspect --format "{{ .NetworkSettings.IPAddress }} <container>`

Networks commands:

- Show networks: `docker network ls`
- Inspect a network: `docker network inspect`
- Create a network: `docker network create --driver`
- Attach a network to container: `docker network connect`
- Detach a network from container: `docker network disconnect`

1. **--network bridge**: is the default Docker virtual network, which is NAT'ed behind the HOST IP.
2. **--network host**: it gains performance by skipping virtual networks but sacrifices security of container model
3. **--network none**: removes eth0 and only leaves you with localhost interface in container

#### Docker Networks: DNS

DNS Basics: <https://dyn.com/blog/dns-why-its-important-how-it-works/>

- Containers shouldn't rely on IP's for inter-communication
- DNS for friendly names is built-in if you use custom networks
- This gets way easier with Docker Compose...

**FORGET ABOUT IP'S** : Static IP's and using IP's for talking to containers is an anti-pattern. Do your best to **avoid** it.

Docker daemon has a built-in DNS server that containers use by default.

**ATTENTION** : --network bridge by default does NOT have the DNS system enabled.  
It's easier to just create a new network and attach the containers to it, instead of using the default bridge network and use the --link commands.

Assigning the same network alias to different containers:

- `docker container run -d --name elks1 --network-alias search --network elastic-network elasticsearch:2`
- `docker container run -d --name elks2 --network-alias search --network elastic-network elasticsearch:2`

### Docker Images

Official Docker Hub: <https://hub.docker.com/>

An image is the app binaries and dependencies. Metadata about the image data and how to run the image.  
Official definition: "An image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime."  
It's NOT a complete Operating System. NO KERNEL, no kernel modules (e.g. drivers) all of these are provided by the local machine or host.

#### Image and their layers

- Images are made up of file system changes and metadata
- Each layer is uniquely identified and only stored once on a host
- This saves storage space on host and transfer time on push/pull
- A container is just a single read/write layer on top of a image
- `docker image history` and `inspect` commands can teach us

#### Image Tagging

`<user>/<repo>:<tag>` : default tag is 'latest' if not specified

- To tag an image: `docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]`  
  Example: `docker image tag nginx pabloando/nginx`

- To push an image: `docker image push pabloando/nginx`
  - To login to docker: `docker login`
    - username: pabloando
  - To logout: `docker logout`

### Dockerfile basics

Dockerfile reference: <https://docs.docker.com/engine/reference/builder/>

In a nutshell: Dockerfile is a recipe to create a Docker image

To build a Dockerfile: `docker image build -t my-image .`  
To build with a different name: `docker build -f some-dockerfile`

#### FROM command

**Always** specify the version of the image we are starting from.

#### ENV command - Environment Variables

One reason they were chose as preferred way to inject key/value is they work everywhere, on every OS and config

- Example inside of a Dockerfile: `ENV NGINX_VERSION 1.11.10-1~jessie`

#### RUN command

- Always chain commands using && to minimize the number of layers
- In the same run command delete the unnecessary files.

#### WORKDIR command

- Change working directory, its like a "cd"
- using WORKDIR is prefered to using 'RUN cd /some/path'

#### EXPOSE command - ports

- By default, no ports are going to be exposed.
- If we want to connect to a contiener, even the EXPOSE ports are declared in the Dockerfile. We must use `-p` running the container.

#### COPY command

- copy the file from the host to the container

#### CMD command

- only one CMD allowed, so if there are multiple, last one wins.
- Required: run this command when container is luanched
- Example: `CMD ["nginx", "-g", "daemon off;"]`

### VOLUMES

- Containers are **usually** immutable and ephemeral
- "immutable infrastructure": only re-deploy containers, never change
- This is the ideal scenario, but what about databases, or unique data?
- Docker gives us features to ensure these "separation of concerns"
- This is known as "persistent data"
- Two ways: VOLUMES and BIND MOUNTS
- Volumes: make special location outside of container UFS
- Bind Mounts: link container path to host path

Attention: Volumes need **MANUALLY** deletion.  
**NAMED VOLUMES** are the proper solution  
Example: `docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql`  
Explanation: `mysql-db:/var/lib/mysql`

- the left part `mysql-db` its the name of the volume
- the right part `/var/lib/mysql` it is where the volume will be alocated inside the container

There is another way to do things: `docker volume create`  
Required to do this before 'docker run' to use custom drivers and labels

### BIND MOUNTING

- Maps a host file or directory to a container file or directory
- Basically just two locations pointing to the same file(s).
- Again, skip UFS, and host files overwrite any in container.
- Can't be used in Dockerfile, must be at `container run`:
  - ... run -v /Users/bret/stuff:/path/container (linux)
  - ... run -v //c//Users/bret/stuff:/path/container (windows)

Example of bind mounting: `docker container run -d --name ginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx`  
En windows `${pwd}`
