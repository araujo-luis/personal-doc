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
   
   `-p`: Port. It is written this way `-p 8080:8080`
	    

 - Remove container

	    docker rm [CONTAINER_ID OR NAME]

	  If you want to remove multiple container you can run this command: `docker rm container1 container2 container3`

	  The container must be stopped in order to remove it, but you can force with the `-f ` option, like this: `docker rm -f container1`
      