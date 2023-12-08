# Dockerfile Instructions
## RUN Instruction:

- ``RUN`` is used to execute commands while building the Docker image.
- It allows you to run commands inside the Docker image at build time
to install packages, update software, download dependencies, create directories, etc.
- Each ``RUN`` instruction in a Dockerfile creates a new layer in the image. This means that 
changes made by a ``RUN`` command are committed to the image 
and become part of its filesystem.

## CMD Instruction:

- ``CMD`` can be used to define the default command that will be executed when a container 
starts from the immage that was created. 
- It specifies what command should be run when the container is launched, and it can be overridden by providing a command at runtime with ``docker run ...``.
- Only the last ``CMD`` instruction in a Dockerfile is effective; it defines the command and parameters to be executed when the container starts.
- If a Dockerfile has multiple ``CMD`` instructions, only the last one takes effect.

## ENTRYPOINT Instruction:

- Alternativly to ``CMD`` there is the ``ENTRYPOINT`` instruction.
- It provides a fixed starting command for the container
- ``CMD`` instruction can be used to provide additional arguments to the ``ENTRYPOINT`` instruction

# Images
Every instruction in the DOCKERFILE creates a layer. The layers are readonly and Docker caches them.
If something in the project needs to be changed you have to rebuild the image. Docker will then rebuild all layers
that are directly affected by the change. These could also be layers that preceed that layer where changes were introduced.

> Docker identifies and rebuilds any invalidated layers due to the introduced changes, ensuring the consistency of subsequent layers.

# Container
Note that Docker creates a thin **writable layer** on top of the last read-only layer. This layer represents the running container and is instantiated from the image. It stores any type of modification during runtime, such as creating and modifiying files, and temporary data storage within the container.


# Docker commands

Docker commands can be categorised into four main groups (ChatGPT 3.5):
- **Image Commands**: These commands are focused on managing Docker images. They include actions like building, tagging, pushing, pulling, inspecting, and removing images. Image commands enable users to create, manipulate, and interact with Docker images.

- **Container Commands**: These commands are centered around managing Docker containers. They include actions such as creating, starting, stopping, restarting, pausing, removing, and inspecting containers. Container commands allow users to control the lifecycle of Docker containers, including their execution and behavior.

- **Management Commands**: This category comprises commands related to Docker daemon, networks, volumes, and other system-level functionalities. Management commands facilitate tasks such as managing Docker daemon, networks, volumes, configuring Docker settings, and handling the overall Docker environment.

- **Runtime Commands**: Runtime commands encompass actions that directly affect the runtime behavior of containers, such as executing commands within a running container, monitoring container resource usage, and managing container logs and stats. These commands provide insights into container performance and allow users to interact with running containers during their execution.

> **Reference**: https://docs.docker.com/engine/reference/run/

> **Tip**: To list all all build-in main commands of docker write ``docker --help``. To list all options to a specific command add ``docker <cmd> --help``.

# Abbreviations

> HP for host port |
> CP for container port

# Image Commands
Building an image from a Dockerfile:

> ``docker build .``

When creating a Docker image using a Dockerfile, you can specify 
the resulting image name using the ``-t`` or ``--tag`` flag and optionally a tag (version):

> ``docker build -t <IMAGE>[:<TAG>] <DOCKERFILE>``

Starting a container from an image in attached mode (attached to Terminal session):

> ``docker run <IMAGE>``

To start it in detached mode:

> ``docker run -d <IMAGE>``

When invoking a docker image you can specify the port mapping and the name of the resulting container instance.

> ``docker run -p <HP:CP> --name <CONTAINER> <IMAGE>``

Creating a container that can be interacted with and provides a tty

> ``docker run -i -t <IMAGE>``
>
> or
>
> ``docker run -it <IMAGE>``

Creating a container that gets automatically removed when the container stops

> ``docker run --rm <IMAGE>``

# Container commands

To stop a container use:

> ``docker stop <CONTAINER>``

Restart existing *stopped* container (detached):

> ``docker start <CONTAINER>``

Restarting existing container (attached):

> ``docker start -a <CONTAINER>``

Reattach to a detached running container:

> ``docker attach <CONTAINER>``

Fetching the logs from a detached container:

> ``docker logs <CONTAINER>``

# Management commands

Listing all images

> ``docker images``

Removing a image

> ``docker rmi <IMAGE>``

Listing running containers

> ``docker ps``

Listing running and stopped containers

> ``docker ps --all``

Removing a stopped container

> ``docker rm <CONTAINER>``

Removing an image

> ``docker rmi <IMAGE>``

Inspecting the configuration of an image

> ``docker image inspect <IMAGE>``

Transfering data from host to container and vice versa

> ``docker cp <source_path> <CONTAINER>:/<container_path>``
>
> ``docker cp <CONTAINER>:/<container_path> <source_path>``

Clone/rename an image 

> ``docker tag <OLD_IMAGE_Name>:[<OLD_TAG>] <NEW_IMAGE_Name>:[<NEW_TAG>]``

Pushing an image to Docker Hub (must login to account) 

> ``docker push <DockerHubID>/<IMAGE>:[<TAG>]``

Pulling an image from Docker Hub

> ``docker pull <DockerHubID>/<IMAGE>:[<TAG>]``

Removing all stopped containers

> ``docker container prune``

Remove all dangling images (not associated by a container or needed by another image)

> ``docker image prune``


# Volumes and Bind Mounts

A Docker container provides it's own filesystem that sits on top of the static image layers (writeable layer). Files created by applications running inside a container will be stored in this writeable layer.

There are three types of Volumes
1. Anonymous Volumes
2. Named Volumes
3. Bind Mounts

## Volumes
If a container will be stoped (not removed) changes made to the filesystem in the writeable layer will persist. But if a container gets removed, all data that was written will be lost.

Therefore to retain data between container removals, Docker has this concept of **Volumes** and **Bind Mounts**. Volumes are managed by Docker. Bind Mounts are managed by the user.

Volumes are specialy designed directories **outside** the container's filesystem. If a volume is a so called **Named Volume** data will persist even if the container is removed. Data that is stored in Named Volumes can be shared across multiple containers that are based on the same image.

This is not the case for **Anonymous Volumes**.

> You do not have direct access to data of a volume on your host machine. Volumes are managed by Docker.

Named Volumes can't be produced in the Dockerfile. They must be created when running the image.

> ``docker run -v <VOLUME_NAME>:/<PATH_INSIDE_CONTAINER_TO_BE_MAPPED> <CONTAINER>``

# Bind Mounts

Bind Mounts allow for mounting or linking folders outside the container to folders inside the container. So any changes made to files and folders outside the container will directly affect files and folders inside the container, and vice versa. This can be used to share configuration files, code or other resources.

Bind Mounts like Volumes must be created when running the image-container.

> ``docker run -v <ABS_PATH_TO_DIR_TO_BE_BOUND_ON_HOST>:<PATH_IN_CONTAINER> <CONTAINER>``

When bind mounting a directory from the host it might happen, that files and folders in the container get overriden. For example when specifing ``RUN npm install`` in the Dockerfile it creates a **node_modules** directory on the container which contains all dependent modules. Now when bind mounting the the working directory on the host that node_modules directory will be overriden or deleted because it does not exist on the host. Now the application crashes because it misses dependencies.

To avoid this from happening you can specify an annoymous volume in addition to the bind mount when running the container. This will create the appropriate directory on the host machine. ``nodule_modules`` directory in this case:

> ``docker run -v <ANONYMOUS_VOLUME> -v <ABS_PATH_TO_DIR_TO_BE_BOUND_ON_HOST>:<PATH_IN_CONTAINER> <CONTAINER>``

TODO: i do not really understand why this works. Docker somehow manages the node_modules directories on both the host and the container without overriding files in neither direction. If a new file is created on the host in node_module dir it is not synchronized to the container, and vice versa. I guess the created annoymous volume acts as a mapping of files and folders between host and a specific container.