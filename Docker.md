# Docker

Images are saved state of containers. This is how containers are stored. They are made of a set of layers. Each layer represent a command to build the image. Run:

```sh
docker history <image-name>
```

to get image layers.

Docker files are series of command to build an image.

- List docker image command:

    ```sh
    docker image --help
    ```

- Start (run) a container:

    ```sh
    docker run <iamge>
    ```

- Give a name to cotainer to use instead of ID:

    ```sh
    docker run --name <name> <image>
    ```

- Stop a running container:

    ```sh
    docker stop <container-id>
    ```

- Remove containters:

    ```sh
    docker rm <container-id>

    # all stopped containers (-q for quiet)
    docker rm $(docker ps -a -q)
    ```

- List containers:

    ```sh
    # running containers
    docker ps

    # all containers
    docker ps -a
    ```

- Run shell (and login as root) inside the container `-i` for _interactive_, and `-t` to allocate a _pseudo-tty_):

    ```sh
    docker exec -it <containerId> /bin/bash
    ```

- Copy files from container to host:

    ```sh
    docker cp <containerId>:/file/path/within/container /host/path/target
    ```

- Display container log and follow up (`-f`):

    ```sh
    docker logs -f <container-id>
    ```

- Get port forward:

    ```sh
    docker port <container-id> <port>
    ```

- List what has changed on the container:

    ```sh
    docker diff <container-id>
    ```

- Return low-level information on Docker objects:

    ```sh
    docker inspect <container-id>
    ````

## Dockerfile

A `RUN` command runs a command inside a container **and create an image** (layer) out of it.

Best practices for `CMD` and `ENTRYPOINT` (ref: [Docker ENTRYPOINT & CMD: Dockerfile best practices](https://medium.freecodecamp.org/docker-entrypoint-cmd-dockerfile-best-practices-abc591c30e21)):

> Both `CMD` and `ENTRYPOINT` instructions define what command gets executed when running a container. There are few rules that describe how they interact.
>
> 1. Dockerfiles should specify at least one of `CMD` or `ENTRYPOINT` commands.
> 1. `ENTRYPOINT` should be defined when using the container as an executable.
> 1. `CMD` should be used as a way of defining default arguments for an `ENTRYPOINT` command or for executing an ad-hoc command in a container.
`CMD` will be overridden when running the container with alternative arguments.

Example:

```dockerfile
FROM ubuntu
ENTRYPOINT ["top", "-b"]
CMD ["-c"]
```

You can replace `-c` options with `-o %CPU` when you run the image:

```sh
docker run <image> -o %CPU
```