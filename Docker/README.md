
# Docker

Docker container commands
- List running containers

	    docker ps
		 
- List all containers
		
	    docker ps -a

- Run containers 

	  docker run [OPTIONS]  [IMAGE_NAME]


	Options:
  
   `-d`: Detach mode. It will run your container in the background, if you want to see the container logs you should run this command: `docker logs [CONTAINER_ID]`

    docker run -d nginx

   
   `-p`: Port. It is written this way `-p HOST:CONTAINER` first number is your local port, second number is the container port. Map TCP port 80 in the container to port 8080 on the Docker host.

    docker run -d -p 8080:80 nginx


   `-e`: Environment variables following this format [VARIABLE]=[VALUE]

	docker run -d -p 8080:80 -e WORKDIR=/usr/html/data -e VERSION=1.1 nginx


   `-v`: Create a new volume or match volume name following this format [LOCALPATH]={CONTAINER PATH}. We can use a name or we can bind with a folder

		docker run -d -p 8080:80 -v mysql=/var/lib/mysql mysql
		docker run -d -p 8080:80 -v /my-path/app=/usr/data/html nginx
	
 - Remove container

	    docker rm [CONTAINER_ID OR NAME]

	  If you want to remove multiple container you can run this command: `docker rm container1 container2 container3`

	  The container must be stopped in order to remove it, but you can force with the `-f ` option, like this: `docker rm -f container1`

- Run commands inside a container on start up
		On start up

       docker run -it [IMAGE_NAME] 		
       docker run -it [IMAGE_NAME] bash
       docker run -it [IMAGE_NAME] /bin/bash

	On a running container (depending your linux distribution, you will need )

	    docker exec -it [CONTAINER_ID] bash
	    docker exec -it [CONTAINER_ID] /bin/bash
	    docker exec -it [CONTAINER_ID] sh
       
- Inspect container data

		docker container inspect [CONTAINER_ID OR NAME]
## Networks

When you start a container you connect with a `bridge network`. All containers on a virtual network can talk to each other without -p
- Create network
		
		docker network create [NETWORK_NAME]
- Attach container to a specific network

		docker run [YOUR_OPTIONS] --net [NETWORK_NAME OR ID] nginx

- Inspect 
		
		docker network inspect [NETWORK_NAME]
## Images

- Docker login, write user and password.

			docker login

- Tag existeing docker images

		docker image tag [YOUR_BASE_IMAGE_TAG] [NEW_TAG]
		docker image tag nginx l222p/my-custom-nginx:1.1

- Publish on DockerHub
	
		docker image push [YOUR_IMAGE]
		docker image push l222p/my-custom-nginx

- Create images

		docker build -t [YOURIMAGE] [DOCKER FILE LOCATION]
		docker build -t my-image .

## Volumes

- List Volumes

		docker volume ls

- Volumes remain when you remove container. Remove volumes
	
		docker volume rm [VOLUME_NAME]
		docker volume rm mysql_volume

- Create (we can create at run time when using docker run). We can create volume ahead of time 

		docker volume create [YOUR NAME]