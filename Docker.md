# Docker

- Copy files from container to host:

	```sh
	docker cp <containerId>:/file/path/within/container /host/path/target
	```
- Run shell inside the container:

	```sh
	docker exec -it <containerId> /bin/bash
	```
- List containers:

	```sh
	docker ps
	```