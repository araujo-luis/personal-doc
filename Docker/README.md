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
   
   `-p`: Port. It is written this way `-p 8080:80` first number is your local port, second number is the container port. Map TCP port 80 in the container to port 8080 on the Docker host.
	    

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