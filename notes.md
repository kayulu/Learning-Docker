# Dockerfile
## RUN Instruction:

- ``RUN`` is used to execute commands while building the Docker image.
- It allows you to run commands inside the Docker image at build time 
to install packages, update software, download dependencies, create directories, etc.
- Each ``RUN`` instruction in a Dockerfile creates a new layer in the image. This means that 
changes made by a ``RUN`` command are committed to the image 
and become part of its filesystem.

## CMD Instruction:

- ``CMD`` is used to define the default command that will be executed when a container 
starts from the built image. 
- It specifies what command should be run when the container is launched, and it can be overridden by providing a command at runtime.
- Only the last ``CMD`` instruction in a Dockerfile is effective; it defines the command and parameters to be executed when the container starts.
- If a Dockerfile has multiple ``CMD`` instructions, only the last one takes effect.

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

To list all all build-in main commands of docker write ``docker --help``. To list all options to a specific command add ``docker <cmd> --help``.

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

# Container commands

To stop a container use:

> ``docker stop <CONTAINER>``

List running containers:

> ``docker ps``

List all containers:

> ``docker ps --all``

Restart existing *stopped* container (detached):

> ``docker start <CONTAINER>``

Restarting existing container (attached):

> ``docker start -a <CONTAINER>``

Reattach to a detached running container:

> ``docker attach <CONTAINER>``

Fetching the logs from a detached container:

> ``docker logs <CONTAINER>``