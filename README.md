# Devops

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


Basic Structure of a Dockerfile
dockerfile
Copy code
# Specify the base image
FROM <base_image>

# Set environment variables (optional)
ENV <key>=<value>

# Copy files to the container
COPY <source> <destination>

# Add files from URL or archive
ADD <source> <destination>

# Install dependencies or run commands
RUN <command>

# Set working directory
WORKDIR <path>

# Expose ports
EXPOSE <port>

# Define default commands or arguments
CMD ["executable", "param1", "param2"]

# Alternate: define entrypoint
ENTRYPOINT ["executable", "param"]

# Optional: define build-time arguments
ARG <name>[=<default_value>]

# Maintain metadata
LABEL <key>=<value>
Key Dockerfile Instructions
FROM:
Specifies the base image to build on.
Example:

dockerfile
Copy code
FROM ubuntu:20.04
RUN:
Executes commands during the image build. Often used to install dependencies.
Example:

dockerfile
Copy code
RUN apt-get update && apt-get install -y python3
COPY vs ADD:

COPY: Copies files/directories from the host to the container.
ADD: Similar to COPY, but also handles URLs and tar files.
Example:
dockerfile
Copy code
COPY app/ /app/
ADD archive.tar.gz /app/
CMD:
Specifies the default command to execute when the container starts. Overrides can be passed during docker run.
Example:

dockerfile
Copy code
CMD ["python3", "app.py"]
ENTRYPOINT:
Similar to CMD, but arguments to the entrypoint are not overridden by default. Used for making scripts executable.
Example:

dockerfile
Copy code
ENTRYPOINT ["python3"]
CMD ["app.py"]
WORKDIR:
Sets the working directory for commands and applications.
Example:

dockerfile
Copy code
WORKDIR /app
ENV:
Sets environment variables in the container.
Example:

dockerfile
Copy code
ENV APP_ENV=production
EXPOSE:
Declares the port the container will listen on. This is for documentation purposes; it does not publish ports.
Example:

dockerfile
Copy code
EXPOSE 8080
Sample Dockerfiles
1. Node.js Application
dockerfile
Copy code
# Use official Node.js image
FROM node:14

# Set working directory
WORKDIR /usr/src/app

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install

# Copy application code
COPY . .

# Expose application port
EXPOSE 3000

# Run the application
CMD ["npm", "start"]
2. Python Application
dockerfile
Copy code
# Base image
FROM python:3.9-slim

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Install dependencies
RUN apt-get update && apt-get install -y gcc

# Set working directory
WORKDIR /app

# Copy dependencies
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Expose Flask application port
EXPOSE 5000

# Run the application
CMD ["python", "app.py"]
3. Custom NGINX
dockerfile
Copy code
# Use official NGINX image
FROM nginx:latest

# Copy custom configuration
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port 80
EXPOSE 80

# Start NGINX
CMD ["nginx", "-g", "daemon off;"]
Best Practices
Minimize Layers: Combine RUN statements to reduce the number of image layers.

dockerfile
Copy code
RUN apt-get update && apt-get install -y \
    curl \
    git && \
    rm -rf /var/lib/apt/lists/*
Use .dockerignore: Exclude unnecessary files from the build context. Example .dockerignore:

bash
Copy code
node_modules
*.log
Specify Base Image Tags: Avoid latest to ensure reproducibility.

Use Multi-Stage Builds: To reduce image size by separating build and runtime stages. Example:

dockerfile
Copy code
# Stage 1: Build
FROM golang:1.17 AS builder
WORKDIR /app
COPY . .
RUN go build -o main .

# Stage 2: Runtime
FROM alpine:latest
WORKDIR /app
COPY --from=builder /app/main .
CMD ["./main"]
Keep Secrets Secure: Do not hard-code sensitive information. Use environment variables or secret management solutions.

Let me know if you need help with specific Dockerfile examples!







What is Docker?
Docker is an open-source platform that enables developers and system administrators to build, deploy, and manage applications within lightweight, portable, and consistent containers. Containers package an application and its dependencies, making it easy to run the application across various environments without compatibility issues.

Why Docker?
Docker provides significant advantages for developers, DevOps engineers, and organizations. Here’s why Docker is widely used:

1. Portability
Docker containers include everything an application needs to run: code, runtime, libraries, and dependencies.
Containers can run consistently across different environments (development, testing, production, or cloud platforms like AWS, Azure, and GCP).
2. Lightweight and Efficient
Containers share the host OS kernel, reducing overhead compared to traditional virtual machines (VMs).
A single server can run multiple containers efficiently, using fewer resources.
3. Faster Deployment and Scaling
Applications can be deployed or updated quickly as containers start almost instantly.
Scaling applications horizontally is straightforward by deploying additional containers.
4. Simplified Development and Testing
Developers can create consistent development environments by defining dependencies and configurations in a Dockerfile.
Testing is simplified, as containers can mimic production environments.
5. Version Control for Applications
Docker allows version control for application dependencies and configurations using Docker images.
Images can be shared through Docker Hub or private registries, making collaboration easier.
6. Isolation
Containers provide process isolation, ensuring that an application in one container does not interfere with others.
This isolation also enhances security by limiting the scope of potential vulnerabilities.
7. DevOps Integration
Docker integrates seamlessly into CI/CD pipelines, allowing automated testing, building, and deployment of containerized applications.
It simplifies workflows for both development and operations teams.
8. Cost Savings
Containers optimize resource utilization by running multiple applications on the same host, reducing the need for additional infrastructure.
Real-World Use Cases
Microservices: Breaking down applications into smaller, manageable services that can be independently deployed.
Continuous Integration/Continuous Deployment (CI/CD): Streamlining the development-to-production pipeline.
Hybrid Cloud and Multi-Cloud: Running applications across different cloud platforms without modification.
Legacy Application Modernization: Packaging legacy applications in containers to run on modern infrastructure.
Docker vs Virtual Machines
Feature	Docker Containers	Virtual Machines
Performance	Lightweight; shares host OS kernel	Heavyweight; includes entire OS
Startup Time	Seconds or milliseconds	Minutes
Resource Usage	Low (shares resources with host)	High (dedicated resources)
Isolation	Process-level isolation	Full isolation with separate OS
Portability	High (consistent across environments)	Medium (requires compatible hypervisor)
Core Docker Concepts
Images: Immutable templates used to create containers. Built using Dockerfiles.
Containers: Runtime instances of Docker images.
Dockerfile: A script that defines how to build an image.
Docker Hub: A public registry to share and distribute Docker images.
Volumes: Mechanism to persist data outside containers.
