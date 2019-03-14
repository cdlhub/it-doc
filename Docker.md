# Docker

## Notes

* Images are saved state of containers. They are made of a set of layers. Each layer represent a command to build an image. Run `docker history <image-name>` to show image layers.
* Docker files are series of command to build an image.

## Commands on Images

* `docker image --help`: list commands that operate on images.
* `docker pull <repo/image:tag>`: download image from a registry like docker hub.
* `docker image ls`: list pulled images.
* `docker rmi <image>`: remove pulled image.
* `docker rmi -f $(docker image ls -q)`: force to remove all pulled images.
* Tag local image (after build and test) for remote registry. Then, push image to remote registry.

    ```sh
    docker tag <image:tag> <repo/image:tag>
    docker push <repo/image:tag>
    ```

## Commands on Containers

* `docker run [options] <image> [command]`: start a container from an image.

  Options:
  * `--name <name>`: give a name to the container for later reference.
  * `-d`: detach mode, runs the container in background.
  * `-it`: run a command on start in interactive mode in a pseudo-tty.
  * `-p <local_port>:<container_port>`: forward local port to container port.
  * `--env VAR=value`: set environment variable.

  `[command]` replaces the `[CMD]` directive of Dockerfile, not the `[ENTRYPOINT]`.
* `docker ps [options]`: list containers.

  Options:
  * `-a`: all containers, even stopped ones.
  * `-q`: list IDs only.
* `docker stop <container>`: stop a running container.
* `docker rm [-f] <container>`: remove a container (`-f` to force, if the container is running).
* `docker rm -f $(docker ps -aq)`: force removal of all containers.
* `docker exec -it <container> /bin/bash`: run shell (and login as root) inside the container `-i` for _interactive_, and `-t` to allocate a _pseudo-tty_).
* `docker logs -f <container>`: Display and follow up (`-f`) container log.
* `docker diff <container>`: list what has changed on the container file system since its start.
* `docker port <container> [<port>]`: display forwarded ports.
* `docker inspect <container>`: return low-level information on Docker objects.
* `docker cp <container:/file/path/within/container> </host/path>`: copy files from container to host.

## Dockerfile

Note that each command in a docker file creates a layer, hence an image.

## Best Practices

* Minimize the number of packages you need per image.
* Execute only one application process per container.
* Minimize layers. For instance, run:

    ```dockerfile
    RUN apt-get update && apt-get install -y ...
    ```

    instead of

    ```dockerfile
    RUN apt-get update
    RUN apt-get install -y ...
    ```

* Sort packages by name for easier maintainance:

    ```dockerfile
    RUN apt-get update && apt-get install -y \
        apache2 \
        git \
        memcached \
        mysql
    ```

* Both `CMD` and `ENTRYPOINT` instructions define what command gets executed when running a container (ref: [Docker ENTRYPOINT & CMD: Dockerfile best practices](https://medium.freecodecamp.org/docker-entrypoint-cmd-dockerfile-best-practices-abc591c30e21)):
    > 1. Dockerfiles should specify at least one of `CMD` or `ENTRYPOINT` commands.
    > 1. `ENTRYPOINT` should be defined when using the container as an executable.
    > 1. `CMD` should be used as a way of defining default arguments for an `ENTRYPOINT` command or for executing an ad-hoc command in a container.
    >
    > `CMD` will be overridden when running the container with alternative arguments.

    Example:

    ```dockerfile
    FROM ubuntu
    ENTRYPOINT ["top", "-b"]
    CMD ["-c"]
    ```

    You can then replace `-c` options with `-o %CPU` when you run the image:

    ```sh
    docker run <image> -o %CPU
    ```

## Instructions

`ADD` and `COPY` are used to copy a file to the container. With `ADD` instruction, you can also specify a URL, or an archive, or a zipped file, while the `COPY` instruction does not handle URL, or unpacking/untarring of files.

A `RUN` command runs a command inside a container **and create an image** (layer) out of it.

## Building Images

Build an image from a docker file or the file `Dockerfile` in `<path>`:

```sh
docker build -t <name:tag> <path>
```