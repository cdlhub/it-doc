# Docker

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
