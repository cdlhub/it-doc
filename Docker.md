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

- Run shell (and login as root) inside the container:

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
