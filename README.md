# Devops

DOCKER
Docker is a platform designed to automate the deployment of applications inside lightweight, portable containers. These containers bundle an application and all its dependencies, allowing it to run consistently across different environments, from development to production.
Key Concepts:
1.	Containers are lightweight, isolated environments that run applications. Unlike virtual machines, containers share the host system's kernel, making them more efficient.
2.	Images: Read-only templates used to create containers. An image contains everything needed to run an application, including the code, libraries, and system tools.
3.	Dockerfile: A script containing a series of instructions for building a Docker image. It defines how the image should be constructed.
4.	Docker Hub: A cloud-based registry where you can find and share Docker images. It serves as a repository for both public and private images.
5.	Docker Compose: A tool for defining and managing multi-container Docker applications. You use a YAML file to configure the services, networks, and volumes.
Benefits of Using Docker:
•	Portability: Applications can run on any system that supports Docker without worrying about the underlying infrastructure.
•	Isolation: Each container runs in its environment, avoiding conflicts between applications and their dependencies.
•	Scalability: Easily scale applications up or down by adding or removing containers.
•	Efficiency: Faster startup times and lower resource overhead compared to traditional virtual machines.
Use Cases:
•	Microservices architecture
•	Continuous integration and deployment (CI/CD)
•	Simplifying development environments
•	Running applications in production with consistent performance






1.	Container Commands
•	Command: docker run
o	Example:  docker run -d -p 8080:80  --name my-nginx nginx
o	Description: Runs a NGINX container in detached mode and maps port 8080 on the host to port 80 in the container.
•	Command: docker ps
•	Command: docker ps -a  # all containers
•	Command: docker stop my-nginx
•	Command: docker start my-nginx
•	Command: docker restart my-nginx
•	Command: docker rm my-nginx
•	Command: docker exec -it my-nginx /bin/bash
o	Description: Opens a bash shell inside the my-nginx container.
•	Command: docker logs my-nginx
o	Description: Displays the logs from the my-nginx container.

2.	Image Commands
•	Command: docker pull ubuntu:latest
o	Description: Pulls the latest Ubuntu image from Docker Hub.
•	Command: docker build -t my-app .
o	Description: Builds an image named my-app from the current directory’s Dockerfile
•	Command: docker images
•	Command: docker rmi my-app
•	Command: docker tag my-app my-app:1.0
o	Description: Tags the my-app image with version 1.0.
•	Command: docker network ls
o	Description: Lists all networks available in Docker.
•	Command: docker network create my-network
o	Description: Creates a new network named my-network.
•	Command: docker network inspect my-network
o	Description: Displays detailed information about my-network.
•	Command: docker network rm my-network
o	Description: Removes the network named my-network.

3.	Volume Commands
Docker volumes are a key feature for managing persistent data in your containers. Command: docker volume ls
o	Description: Lists all volumes managed by Docker.
•	Command: docker volume create my-volume
o	Description: Creates a new volume named my-volume.
•	Command: docker volume inspect my-volume
o	Description: Displays detailed information about my-volume.
•	Command: docker volume rm my-volume
o	Description: Removes the volume named my-volume.
4.	Commands system
•	Command: docker info
o	Description: Displays system-wide information about Docker.
•	Command: docker version
o	Description: Displays the version information of Docker.
•	Command: docker system df
o	Description: Shows disk usage by images, containers, and volumes.
•	Command: docker system prune
o	Description: Removes unused containers, networks, images, and optionally volumes.

5.	Docker Compose Commands
•	Command: docker-compose up -d
o	Description: Starts services defined in the docker-compose.yml file in detached mode.
•	Command: docker-compose down
o	Description: Stops and removes all containers defined in the docker-compose.yml file.
•	Command: docker-compose logs
o	Description: Displays logs from all services defined in the docker-compose.yml file.
•	Command: docker-compose build
o	Description: Builds or rebuilds services defined in the docker-compose.yml file.
•	Command: docker inspect my-nginx
o	Description: Displays detailed information about the my-nginx container.
•	Command: docker commit my-nginx my-nginx:v2
o	Description: Creates a new image from the changes made in the my-nginx container.
•	Command: docker save -o my-image.tar my-app
o	Description: Saves the my-app image to a .tar file.
•	Command: docker load -i my-image.tar
o	Description: Loads an image from a .tar file.
