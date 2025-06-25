# Important Terminology
- *Docker client*
Command used to control most of the Docker workflow and talk to remote Docker servers.
- *Docker server*
`dockerd` command used to start the Docker server process that builds and launches containers via a client.
- *Docker or  OCI images*
Docker and OCI image consist of one or more filesystem layers and important metadata, representing the files required to run a containerized application. An image has a repository address, a name and a tag, which is used to identify a particular release of an image. 
A Docker image is any image that is compatible with the docker toolset, while an OCI image is specifically an image that meets the *Open Container Initiative* standard and is guaranteed to work with any OCI-compliant tool.
- *Linux container*
This is a container that has been instantiated from a Docker or OCI image. A specific container can exist only once; however, you can easily create multiple containers from the same image. *Docker container* is a misnomer since Docker simply leverages the operating system's container functionality.
- *Atomic or immutable host*
This is a finely tuned OS image, that supports container hosting and atomic OS upgrades (like [Fedora CoreOS](https://fedoraproject.org/en/coreos/).

# The Docker landscape
## Support and Adoption
Primary high-level OCI ([Open Container Initiative](https://www.opencontainers.org/)) certified runtime:
- [containered](https://containerd.io/):  
default high-level runtime in modern versions of Docker and Kubernetes

Lower-level OCI-certified runtimes can be used by containerd to manage and create containers:
- [runc](https://github.com/opencontainers/runc)
often used as the default lower-level runtime by `containered`
- [crun](https://github.com/containers/crun)
written in C and designed to be fast and have a small memory footprint
- [Kata Containers](https://katacontainers.io/)
from Intel, Hyper and the OpenStack Foundation is a virtualized runtime that can run a mix of containers and virtual machines
- [gVisor](https://github.com/google/gvisor)
from Google is a sandboxed runtime, implemented entirely in user space
- [Nabla Containers](https://nabla-containers.github.io/)
another sondboxed runtime designed to significantly reduce the attack surface of Linux containers
## Architecture
The fundamental user-facing structure of Docker is a client/server model, with the basic system interaction being a client talking though an API to a server. Sitting behind the Docker API are "containerd" and "runc". Docker heavily leverages kernel mechanisms such as:
- iptables
- virtual briding
- Linux control groups (cgroups)
- Linux namespaces
- Linux capabilities
- secure computing mode
- various filesystem drivers
### Client/Server Model
The easiest way to think about Docker is as two parts: 
- **client**
tells the server what to do; can address any number of servers
- **server/[daemon](https://en.wikipedia.org/wiki/Daemon_(computing))**
servers are used to build, run and manage the containers; can communicate with the image registries when told by the client
The daemon can run on any number of servers in the infrastructure
- **registry (optional)**
stores Docker images and their metadata

![[Server-Client-Registry-Model.png]]

The Docker infrastructure has a `docker` client and a `dockerd` server, but is not entirely monolithic. The Server orchestrates other components, including `containerd-shim-runc-v2`, used to interact with `runc` and `containered`. 
Each Docker host has one Docker server running, that manages any number of containers. You can use the `docker` command-line tool to talk to the server, either from the server itself or a secured remote client.

### Network Ports and Unix Sockets
The docker command-line tool and the `dockerd` daemon can talk to each other ober Unix sockets and network ports.
Three ports have been registered by Docker Inc. with the IANA:
- **TCP port 2375** (unencrypted traffic)
- **port 2376** (encrypted SSL connections)
- **port 2377** (Docker Swarm mode)

>[!warning] Using other ports
>Using other ports is easily configurable, but if not needed you should use the default setting.

By default only Unix sockets are used for communication with the local Docker daemon, ensuring the most secure installation. Additionally it is recommended to not use network ports with Docker, due to the lack of user authentication and role-based access controls within the Docker daemon.

>[!note]
>Usually the Unix socket si located in `/var/run/docker.sock`, but recent versions may create the `docker.sock` file in the user's home directory inside `.docker/run/` and link `_/var/run/docker.sock` to this location.

### Robust Tooling 
Docker has launched its own orchestration tools:
- [Compose](https://github.com/docker/compose)
- [Docker Desktop](https://www.docker.com/products/docker-desktop)
- [Swarm mode](https://docs.docker.com/engine/swarm)

### Command-Line Tool
This tool is the main interface of Docker and is available on various platforms as part of the main Docker distribution.
The client is a [[Go]] program and the command-line tool compiles directly from the Go source.
The command-line tool is used for:
- Building a container image
- Pulling images from a registry to a Docker daemon or pushing them up to a registry from the Docker daemon
- Starting a container on a Docker server either in the foreground or background
- Retrieving the Docker logs from a remote server
- Interactively running a command inside a running container on a remote server
- Monitoring statistics about your container
- Getting a process listing from your container
### Docker Engine API
The Docker daemon has an API and this is what the command-line tool uses to communicate with the daemon.
The API is open and [documented](https://dockr.ly/2wxCHnx), which allows the use of external tools to create, inspect and manage the images and containers under the daemons management.
Docker maintains [SDKs for Python and Ruby](https://dockr.ly/2wxCHnx) and there are additional libraries (f.e. [Go](https://github.com/fsouza/go-dockerclient), [Ruby](https://github.com/upserve/docker-api)) maintained by third parties.
>[!note] 
>Two things that are not supported by the API are
>- *endpoints*
>  require streaming
>- *terminal access*
>  running remote shells or executing the container in interactive mode
>
>In these cases it is easier to use the client libraries or the command-line tool

### Container Networking
The default networking configuration is the so called *bridge mode*. 
The Docker server acts like a bridge between the "outside world" and the containers, which could be thought of as hosts on a mini virtual network.
Each container has a virtual Ethernet interface and an IP address allocated to it, which is connected to the Docker server.
Docker lets you bind and expose individual or groups of ports on the host to the container so the outside world can reach the containers on those ports.

>[!note]
>The traffic is largely managed by the [vpnkit](https://github.com/moby/vpnkit) library

>[!note]
>Docker allocates the IP addresses of the private subnet of containers from an unused [RFC 1918](https://www.rfc-editor.org/rfc/rfc1918) private subnet block. 
>It detects which network blocks are unused on the host and allocates one of those to the virtual network.

The virtual subnet of the containers is bridged to the local network of the host through the `docker0` interface on the server.

>[!summary]
>The containers are on a virtual network together and can talk to eachother directly, but to get to the host or the outside world, they have to pass through the `docker0` virtual bridge interface on the Docker server.

![[Bridge mode.png]]

>[!hint]
>There are a lot of ways to configure Docker's network layer and you can find the details of Docker networking in the [documentation](https://dockr.ly/2otp461)
## Getting the Most from Docker
>[!abstract]
>All Tools have a number of great use cases and others that are not so good. 
>You can, for example, open a glass bottle with a hammer. But that has its downsides.
>

Docker's architecture is aimed at applications that are either stateless or where the state ist externalized into data stores like databases or caches.
Databases that run well in Docker are now often deployed this way, but it is not the simple path. 
Applications like web frontends, backend APIs and short running tasks like maintenance scripts that might normally be handled by `cron`.
### Containers are not Virtual Machines
Linux containers can be thought of as very lightweight wrappers around a single Unix process, which might spawn other processes, but one statically compiled binary can also be all that is inside a container.

Virtual machines are designed as stand-ins for real hardware and have a long(er)-lived nature than containers, which can exist for months or run a task for a minute and then be destroyed.

>[!note]
>If you run Docker on a mac or Windows system, you leverage a Linux virtual machine to run the Docker server `dockerd`.
>On Linux, `dockerd` can be run natively and do not need a virtual machine to be run.

### Limited Isolation
Containers are isolated from one another, but the default container configuration has them all sharing CPU and memory on the host system, as they are colocated Unix processes. Unless you constrain them, containers can compete for resources.
Limits on CPU and memory use are encouraged through Docker, but are not the default.
Containers often share one or more common filesystem layers, but that means if you update a shared image, you may need to rebuild and redeploy containers that are using the older image.

## The Docker Workflow
### Filesystem Layers

# Dockerfile
## Anatomy of a Dockerfile
A typical *Dockerfile* could look like the one below, which creates a container for a Node.js-based application:
```Dockerfile
FROM node:18.13.0

ARG email = "anna@example.com"
LABEL "maintainer" = $email
LABEL "rating" = "Five Stars" "class" = "First Class"

USER root

ENV AP /data/app
ENV SCPATH /etc/supervisor/conf.d

RUN apt-get -y update

# The daemons
RUN apt-get -y install supervisor
RUN mkdir -p /var/log/supervisor

# Supervisor Configuration
COPY ./supervisordf/conf.d/ * $SCPATH/

# Application Code
COPY *.js* $AP/

WORKDIR $AP

RUN npm install

CMD ["supervisord", "-n"]
```

>[!note]
>Remember that each line in a Dockerfile creates a new image layer that is stored by Docker. When you build new images, Docker will only need to build layers that deviate from the previous builds, meaning that you can reuse all the layer that have not changed.

>[!tip] 
>You could build a Node instance from a plain, base Linux image, alternatively you could explore [Docker Hub](https://registry.hub.docker.com/) for official images for Node. The Node.js community also maintains a series of [Docker images](https://registry.hub.docker.com/_/node) and tags that allow you to quickly determine what versions are available.

1. The following base image will provide you with an Ubuntu Linux image running Node 18.13.0. The image is locked to the Node point release 18.13.0.
```Dockerfile
FROM node:18.13.0
```

2. With the ARG parameter you can set variables and their default values, which are only available during the image build process.
```Dockerfile
ARG email = "anna@example.com"
```
3. For the `maintainer` label we use the value of the email build argument that was defined in the previous line. This means that this label can be changed anytime we build this image.
   
>[!info]
   >You can add labels to images and containers via key/value pairs that can be used to identify Docker images and containers.
   >You can use the `docker image inspect` command to see the labels applied to any image.
   
```Dockerfile
LABEL "maintainer" = $email
LABEL "rating" = "Five Start" "class" = "First Class"
```
4. The `USER` instruction is used to change the user on which Docker runs the processes within the container. The default user is `root`.
>[!warning]
>Due to potential security risks, production containers should be run in the context of an unprivileged user. 

```Dockerfile
USER root
```
5. `ENV` instructions allows you to set shell variables that are used by the running application for configuration and are available during the build process.
```Dockerfile
ENV AP /data/app
ENV SCPATH /etc/supervisor/conf.d
```
6. In the following code, you use a collection of `RUN` instructions to start and create the required file structure and install required software dependencies
>[!warning]
> It is not recommended to run commands like `apt-get -y update` or `dnf -y update` in your applications Dockerfile. This is because it requires crawling the repository index each time you run the build, which means that your build is not guaranteed to be repeatable since the package version might change between builds.
> Instead you can base your application image on another image that already has these updates applied to it and where the versions are in a known state. It will be faster and more repeatable.

```Dockerfile
RUN apt-get -y update

# The daemon
RUN apt-get -y install supervisor
RUN mkdir -p /var/log/supervisor
```
7. You use the build variables defined in the previous section to save work and protect you from typos
>[!note]
>The `COPY` instruction is used to copy files from the local filesystem into your image. Most often this includes your application code and required support files. 
>As the files are in your image you no longer need access to the local filesystem to access them once the image is build.

>[!tip]
>It is advised to combine a few logically grouped commands into a single line.
>It is possible to use the `COPY` and `RUN` instructions combined to copy a complex script to your image and then execute it.

```Dockerfile
# Supervisor Configuration
COPY ./supervisord/conf.d* $SCPATH/

# Application Code
COPY *.js* $AP/
```
8. With the `WORKDIR` instruction, you change the working directory in the image for the remaining build instructions and the default process that launches with any resulting container.
>[!warning]
>The order of commands in a Dockerfile can have significant impact on ongoing build times. You should try to order commands that change between every single build closer to the bottom. This is because every layer after the first introduced change will need to be rebuild.

9. You end the Dockerfile with the `CMD` instruction, which defines the command that launches the process that you want to run within the container.
   Here you are using `supervisord` as a process manager to help improve the resiliency of the node application within the container and ensure that it stays running
   >[!tip]
   >It is advised to run only a single process within a container. This allows you to easily scale individual functions horizontally within your architecture.
   
```Dockerfile#
CMD ["supervisord", "-n"]
```

# Working with Images
## Building an Image
1. First we clone a Git repo that contains an example application called "docker-node-hello":
>[!note]
>the `--config core.autocrlf=input` option ensures that line endings are not altered from the Linux standard that is expected.

```
$ git clone https://github.com/spkane/docker-node-hello.git \ --config core.autocrlf=input
```

2. We then change to the directory and look at the contents
```
$ cd docker-node-hello
$ tree -a -I .git
```
 3. You should see the following:
>[!note]
>The *.dockerignore* file allows you to define files and directories that you do not want to upload to the Docker host when you are building the image.
>The *supervisord* directory contains the configuration files for supervisord that you will use to start and monitor the application.
     
```
 .
├── .dockerignore
├── .gitignore
├── Dockerfile
├── index.js
├── package.json
└── supervisord
    └── conf.d
        ├── node.conf
        └── supervisord.conf
```
4. Run the build
>[!note]
> At the end of the build command, you will see a `.` This tells Docker the build context and what files it should upload to the server. This build context is what the .dockerignore file is filtering so that we do not upload more than we need.
> 

```
docker image build -t example/docker-node-hello:latest .
```

## Running Your Image
1. When the image is successfully build you can run it on your Docker host with:
```
$ docker container run --rm -d -p 8080:8080 example/docker-node-hello:latest
```

2. This tells Docker to create a running container in the background with the `example/docker-node-hello:latest` tag and maps it to port 8080 in the container to port 8080 on the Docker host.
>[!tip]
>You can use `docker container ls` to see the running applications in action.

3. When the container is running you can enter the IP address of the Docker host and the port number into a web browser or you use a command-line tool like curl.
>[!note]
>You can determine the Docker host IP address with the `docker context list` command.
>If the `DOCKER ENDPOINT` is set to a Unix socket, the IP address is most likely `127.0.0.1`
### Build Arguments
When inspecting the image we can see that the maintainer label was set to `anna@example.com`
```
$ docker image inspect \ example/docker-node-hello:latest | grep maintainer
```

If we want to change the maintainer label, we rerun the build and provide a new value for the email ARG via the `--build-arg` command-line argument
```
$ docker image build --build-arg email=me@example.com \ -t example/docker-node-hello:latest .
```

After the build has finished, we can inspect the image again.
```
$ docker image inspect \ example/docker-node-hello:latest | grep maintainer
```
### Environment Variables as Configuration
In the `index.js` file you can find the `$WHO` variable, which determines who the application is going to say hello to:
```
var DEFAULT_WHO = "World"; 
var WHO = process.env.WHO || DEFAULT_WHO;

app.get('/', function (req, res) { 
	res.send('Hello ' + WHO + '. Wish you were here.\n'); 
});
```
To change this by passing in environment variables, you first need to stop the container and for this you need the container ID.
You can get the container ID by using 
```
$ docker container ls
```
Then we stop the container with
```
$ docker container stop containerID
```

You can change the names by adding a `-env` argument to the docker container run command
```
$ docker container run --rm -d \
--publish mode=ingress,published=8080,target=8080 \ 
--env WHO="Sean and Karl" \ 
example/docker-node-hello:latest
```

>[!note]
>You could shorten the docker command to
>```
>$ docker container run --rm -d -p 8080:8080 \ 
>-e WHO="Sean and Karl" \ 
>example/docker-node-hello:lates
>```
### Custom Base Images
Base images are the lowest-level images that other Docker images will build upon, most often based on a minimal install of a Linux distribution.

If it is preferable to build one's own image, f.e. maintaining a consistent OS image across all deployments, building it will result in better deployment and easier application distribution.

A middle ground is to build images using Alpine Linux. Something to keep in mind is that Alpine Linux is based on the musl standard library, which could impact Java-based applications and DNS resolution.

## Storing Images
### Public Registries
The [Docker Hub](https://hub.docker.com/) is an image registry for public images, which also include official images for Linux distributions and much more.
>[!note]
>A downside of public registries is that they are not local to the network on which the application is deployed. This could have a big impact on software development and deployment, caused by internet latencies and outages.
### Private Registries
The other option to store images is to use a internal private registry, which can interact with the Docker clients. 
Private registries are [the open source Distribution](https://github.com/distribution/distribution), [Harbor](https://goharbor.io/) and [Red Hat Quay](https://www.redhat.com/en/technologies/cloud-computing/quay).
#### Running a Private Registry
We are going to create a simple secure registry using SSL and HTTP basic authentication

First clone a Git repository that contains the basic files to set up a simple authenticated Docker registry:
```
$ git clone https://github.com/spkane/basic-registry \
--config core.aitocrlf=input
```

When you have done that change to the new directory and examine the files
```
$ cd basic registry
$ ls
```

If your Docker server is available via `localhost` (127.0.0.1) then you can use the files unmodified by copying them:
```
$ cp config.yaml.sample config.yaml
$ cp registry.key.sample registry
$ cp registry.crt.sample registry.cert
$cp htpasswd.sample htpasswd
```

>[!note]
>If your Docker Server is on a remote IP address, then you will need to:
>  1. copy `config.yaml.sample` to `config.yaml`
>  2. edit `config.yaml` and replace the IP with the IP of your Docker Server
>  3. Create a SSL keypair
>```
>$ openssl req -x509 -nodes -sha256 -newkey rsa:4096 \ 
>-keyout registry.key -out registry.crt \ 
>-days 14 -subj '{/CN=ipaddress}'
>```
>4. Use the `htpasswd.sample` file or create your own username and password
>```
>$ docker container run --rm --entrypoint htpasswd g \
> -Bbn username password > htpasswd
>```
#### Testing the private Registry
To test a private registry you have authenticate against it, so you need to make sure that the IP address in the docker login matches the IP address of your Docker server that is running the registry.

>[!note]
>`myuser` is the default username and `myuser-pw!` is the default password. If you generated your own `htpasswd` these will be used.

```docker
$ docker login 127.0.0.1:5000
Username: registryUsername
Password: registryPassword
Login Succeeded
```

>[!warning]
>The registry container has an embedded SSL key and is not using any external storage, which means that it contains a secret, when you delete the running container all your images will also be deleted.
>
>In production, you will want to have your containers pull secrets from a secrets management system and use some type of redun‐ dant external storage, like an object store. If you want to keep your development registry images between containers, you could add something like `--mount type=bind,source=/tmp/registrydata,target=/var/lib/registry `to your docker container run command to store the registry data on the Docker server.

Then you can try to push the image into the registry:
```
$ docker image tag my-registry 127.0.0.1:5000/my-registry
$ docker image push 127.0.0.1:5000/my-registry
Using default tag: latest
The push refers to repository [127.0.0.1:5000/my-registry]
f09a0346302c: Pushed
…
4fc242d58285: Pushed
latest: digest: sha256:c374b0a721a12c41d5b298930d11e658fbd37f22dc2a0fac7d6a2…
```

After that try to pull the image from the repository:
```
$ docker image pull 127.0.0.1:5000/my-registry
Using default tag: latest
latest: Pulling from my-registry
Digest: sha256:c374b0a721a12c41d5b298930d11e658fbd37f22dc2a0fac7d6a2ecdc0ba5490
Status: Image is up to date for 127.0.0.1:5000/my-registry:latest
127.0.0.1:5000/my-registry:latest
```

If you did not encounter any errors the registry is working correctly and you can stop the registry:
```
$ docker container stop registry
```
### Authenticate to a Registry
By default Docker assumes the registry will be Docker Hub, so you need a account.
### Logging into a Registry
>[!note]
>You should log into Docker Hub using a [limited-privilege personal access token](https://docs.docker.com/go/access-tokens) 

To log into your account use:
```docker
$ docker login
Username: hub_username
Password: hub_password/token
```
### Pushing images into a Repository
Ensure that you are logged into your Docker Hub
```
docker login
```

>[!note]
>You used the command `docker image build -t example/docker-node-hello:latest .` to build the docker-node-hello image, but the Docker client interpreted `example/docker-node-hello:latest` as `docker.io/example/docker-node-hello:latest`.
>`docker.io` signifies the image registry hostname and `example/docker-node-hello` is the repository inside the registry that contains the image
>

You can update the tags on a image that you already created with the following
```
$ docker image tag example/docker-node-hello:latest \ 
docker.io/username/docker-node-hello:latest
```
>[!caution]
>You should not enter a spacebar after `\` in the first line, rather press enter right after it

You can rebuild the image with the new naming convention with the following command inside the `docker-node-hello` directory that was created when we cloned it from github
```
~/docker-node-hello$ docker image build -t docker.io/${}/docker-node-hello:latest .
```

To verify that the image is on the server run the command
```
~/docker-node-hello$ docker image ls username/docker-node-hello
```

After that you can push the image to the Docker repository with
```
docker image push username/docker-node-hello:latest
```

### Pulling images from a Repository
You can download images from public repositories with the `image pull` command
```
docker image pull username/docker-node-hello:latest
```

> [!tip]
> To download images from private repositories you need to log in with credentials that have access to the repository before you are able to pull the image.

## Exploring Images in Docker Hub
You can explore available images by running the `docker search` command
```
$ docker search node
```

## Optimizing Images
### Keeping Images Small
### Multistage builds
Multistage builds are recommended for most production containers as the constrain containers to a very small size.
A Dockerfile for a multistage build can look like this:
```Dockerfile
# Build container 
FROM docker.io/golang:alpine as builder 
RUN apk update && \ 
	apk add git && \ 
	CGO_ENABLED=0 go install -a -ldflags '-s' \
	github.com/spkane/scratch-helloworld@latest 
# Production container 
FROM scratch 
COPY --from=builder /go/bin/scratch-helloworld /helloworld 
EXPOSE 8080 
CMD ["/helloworld"]
```
- The `FROM` command has been extended so that you can name the image during the build phase
- The first line `FROM docker.io/golang as builder` means you want to base the build on the golang image and will refer to this build as `builder`
- The second `FROM` with the name `scratch` tells Docker to start from an empty image
- `COPY --from=builder /go/bin/scratch-helloworld /helloworld` allows you to copy the binary into the `scratch` image, ensuring the smallest container possible

Let's try to build this.
```
# create a directory
$ mkdir /tmp/multi-build 
$ cd /tmp/multi-build 
# use Vim to copy the example above into a file called "Dockerfile"
$ vi Dockerfile
# Start the multistage build
docker image build .
```

### Layers are Additive
Images layers are strictly additive, meaning once a layer is created, nothing can be removed from it. This means you cannot make earlier layers smaller by deleting files in a subsequent layer. Doing that only masks the older version with the modified or removed version in the new layer.

The most common way to deal with this is by stringing commands together in a single Docker line. To do this you can simply use the `&&` operator together with the `\` operator, which indicates that a command continues after the newline, to increase readability.
You could, for example rewrite
```Dockerfile
FROM docker.io/fedora 
RUN dnf install -y httpd 
RUN dnf clean all 
CMD ["/usr/sbin/httpd", "-DFOREGROUND"]
```
as 
```Dockerfile
FROM docker.io/fedora 
RUN dnf install -y httpd && \ 
	dnf clean all 
CMD ["/usr/sbin/httpd", "-DFOREGROUND"]
```

## Troubleshooting Broken Builds
### Debugging Pre-BuildKit Images
Consider the following error:
```
$ DOCKER_BUILDKIT=0 docker image build -t example/docker-node-hello:latest \
 --no-cache . 
 
Sending build context to Docker daemon 9.216kB 
Step 1/14 : FROM docker.io/node:18.13.0
 ---> 9ff38e3a6d9d 
 … 
Step 6/14 : ENV SCPATH /etc/supervisor/conf.d 
 ---> Running in e903367eaeb8 
Removing intermediate container e903367eaeb8
 ---> 2a236efc3f06 
Step 7/14 : RUN apt-get -y update-all 
 ---> Running in c7cd72f7d9bf
E: Invalid operation update-all
The command '/bin/sh -c apt-get -y update-all' returned a non-zero code: 100
```

When troubleshooting, you should remember that almost all Docker images are layered on top of other Docker images and that you can start a container from any image.

Looking at the output of step 6 we know that the build process is running a container created in step 5, `Running in e903367eaeb8`. The next line tells us that the container is removed after adding a default variable `ENV SCPATH /etc/supervisor/conf.d` .  In the final line we are given the image ID for the newly generated image `2a236efc3f06`, which is the image we need to troubleshoot as it is the last success step in the build.

With this information we can run the lower layer as a container itself:
```
$ docker container run --rm -ti 2a236efc3f06 /bin/bash
root@b83048106b0f:/#
```
You can now run any command that might determine what is causing the build to fail. 

### Debugging BuildKit Images