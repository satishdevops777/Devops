# Docker

## What is Docker?

Docker is an open-source platform designed to simplify the development, deployment, and management of applications by using containers. Containers are lightweight, standalone, and executable packages of software that include everything needed to run an application—code, runtime, libraries, environment variables, and configurations. Docker ensures that applications behave the same regardless of the environment.

---

## Why Docker?

### Consistency Across Environments
Docker solves the classic "it works on my machine" problem by providing a consistent runtime environment across development, staging, and production.

### Lightweight and Fast
- Containers share the host system's kernel, making them more lightweight than virtual machines.
- Startup times are faster since containers don’t require a full OS boot.

### Portability
- Containers can run on any machine with Docker installed, whether it's a developer's laptop, a testing server, or a cloud environment.

### Efficient Resource Utilization
- Docker containers use fewer system resources compared to traditional VMs, as they don't include a full OS.

### Scalability
- Docker simplifies horizontal scaling by allowing developers to quickly spin up new containers to handle increased load.

### Integration with CI/CD Pipelines
- Docker seamlessly integrates into CI/CD workflows, enabling automated testing, deployment, and delivery.

### Open-Source Ecosystem
- Docker has a vibrant community and a vast library of ready-to-use container images available on Docker Hub.

---

## Dockerfiles for Different Languages

A Dockerfile is a script containing instructions to assemble a Docker image. Here's how you can create Dockerfiles for various programming languages:

### 1. Python

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

### 2. Java

```dockerfile
FROM openjdk:11
WORKDIR /app
COPY target/myapp.jar .
CMD ["java", "-jar", "myapp.jar"]
```

### 3. Node.js

```dockerfile
FROM node:18-alpine
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["node", "app.js"]
```

### 4. Go

```dockerfile
FROM golang:1.19
WORKDIR /app
COPY . .
RUN go build -o main .
CMD ["./main"]
```

---

## Docker Compose

Docker Compose is a tool for defining and managing multi-container Docker applications. You use a `docker-compose.yml` file to configure your application's services.

### Key Features
- Simplifies multi-container setups.
- Allows easy scaling with a single command.
- Integrates well with CI/CD pipelines.

### Example Docker Compose File
A web app with a backend (Python Flask) and a database (MySQL):

```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - db
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: exampledb
```

---

## Key Docker Concepts

### Containers
- Lightweight, isolated environments that run applications.
- Share the host system's kernel, making them more efficient than virtual machines.

### Images
- Read-only templates used to create containers.
- Contain everything needed to run an application, including code, libraries, and system tools.

### Dockerfile
- A script containing a series of instructions for building a Docker image.

### Docker Hub
- A cloud-based registry to find and share Docker images.
- Contains both public and private images.

### Docker Compose
- A tool for defining and managing multi-container Docker applications.

---

## Benefits of Using Docker

- **Portability**: Applications can run on any system that supports Docker.
- **Isolation**: Each container runs in its environment, avoiding conflicts between applications and their dependencies.
- **Scalability**: Easily scale applications by adding or removing containers.
- **Efficiency**: Faster startup times and lower resource overhead compared to traditional virtual machines.

---

## Common Use Cases

- Microservices architecture
- Continuous integration and deployment (CI/CD)
- Simplifying development environments
- Running applications in production with consistent performance

---

## Common Docker Commands

### Container Commands
- Run a container:
  ```bash
  docker run -d -p 8080:80 --name my-nginx nginx
  ```
- Mount a volume:
  ```bash
  docker run -v myvolume:/data -d nginx
  ```
- List running containers:
  ```bash
  docker ps
  ```
- Stop a container:
  ```bash
  docker stop my-nginx
  ```
- Start a container:
  ```bash
  docker start my-nginx
  ```
- Remove a container:
  ```bash
  docker rm my-nginx
  ```
- Access a running container:
  ```bash
  docker exec -it my-nginx /bin/bash
  ```

### Image Commands
- Pull an image:
  ```bash
  docker pull ubuntu:latest
  ```
- Build an image:
  ```bash
  docker build -t my-app .
  ```
- List images:
  ```bash
  docker images
  ```
- Remove an image:
  ```bash
  docker rmi my-app
  ```

### Volume Commands
- List volumes:
  ```bash
  docker volume ls
  ```
- Create a volume:
  ```bash
  docker volume create my-volume
  ```
- Remove a volume:
  ```bash
  docker volume rm my-volume
  ```

### Networking Commands
- List networks:
  ```bash
  docker network ls
  ```
- Create a network:
  ```bash
  docker network create mynetwork
  ```
- Connect a container to a network:
  ```bash
  docker network connect mynetwork <container_id>
  ```

### Docker Compose Commands
- Start services:
  ```bash
  docker-compose up -d
  ```
- Scale services:
  ```bash
  docker-compose up --scale app=3
  ```
- Stop services:
  ```bash
  docker-compose down
  ```
- View logs:
  ```bash
  docker-compose logs
  ```

---

## Docker Hub

Docker Hub is a cloud-based repository for Docker images. It allows users to:
1. Search for Images: Thousands of pre-built images (e.g., MySQL, Nginx).
2. Push Custom Images: Share images created locally with others.
3. Automated Builds: Trigger builds directly from a connected Git repository.

### Example
Pull and run an Nginx image:

```bash
docker pull nginx:latest
docker run -d -p 8080:80 nginx
```

---

## Dockerfile Commands

A Dockerfile is a text file with instructions to build a Docker image. Below are the commonly used commands:

1. **FROM**
   Specifies the base image.
   ```dockerfile
   FROM python:3.9-slim
   ```
   - Purpose: Every Dockerfile must start with FROM. It defines the base image to use for the new image.
   - Example: `FROM ubuntu:20.04`

2. **WORKDIR**
   Sets the working directory inside the container.
   ```dockerfile
   WORKDIR /app
   ```
   - Purpose: Ensures all subsequent commands (like RUN, COPY) are executed in this directory.

3. **COPY**
   Copies files or directories from the host to the container.
   ```dockerfile
   COPY requirements.txt .
   ```
   - Purpose: Transfers files from the build context into the image.

4. **ADD**
   Similar to COPY, but can also fetch files from remote URLs or extract archives.
   ```dockerfile
   ADD https://example.com/file.tar.gz /app/
   ```
   - Purpose: More powerful than COPY, but generally avoided unless needed.

5. **RUN**
   Executes a command in the container during the image build.
   ```dockerfile
   RUN pip install --no-cache-dir -r requirements.txt
   ```
   - Purpose: Used for installing packages or setting up the environment.

6. **CMD**
   Specifies the default command to run when the container starts.
   ```dockerfile
   CMD ["python", "app.py"]
   ```
   - Purpose: This command can be overridden at runtime.

7. **ENTRYPOINT**
   Defines a command that is fixed and cannot be overridden easily.
   ```dockerfile
   ENTRYPOINT ["python", "script.py"]
   ```
   - Purpose: Ensures the container always executes the specified command.

8. **ENV**
   Sets environment variables in the container.
   ```dockerfile
   ENV APP_ENV=production
   ```
   - Purpose: Makes it easier to configure the container.

9. **EXPOSE**
   Documents the port on which the application runs.
   ```dockerfile
   EXPOSE 5000
   ```
   - Purpose: Informative; doesn’t actually publish the port. You still need `-p` or `-P` in `docker run`.

10. **VOLUME**
    Creates a mount point for a volume.
    ```dockerfile
    VOLUME /data
    ```
    - Purpose: Used for persistent data storage.

11. **LABEL**
    Adds metadata to the image.
    ```dockerfile
    LABEL maintainer="Satish Reddy <satish@example.com>"
    ```
    - Purpose: Useful for providing image details.

12. **USER**
    Sets the user for executing commands.
    ```dockerfile
    USER nonrootuser
    ```
    - Purpose: Avoids running containers as root for security reasons.

13. **ARG**
    Defines variables that users can pass at build time.
    ```dockerfile
    ARG APP_VERSION=1.0
    ```
    - Purpose: Differentiate images with dynamic variables.

14. **SHELL**
    Specifies the shell to use for subsequent commands.
    ```dockerfile
    SHELL ["/bin/bash", "-c"]
    ```

15. **HEALTHCHECK**
    Adds a health check to the container.
    ```dockerfile
    HEALTHCHECK --interval=30s CMD curl -f http://localhost/ || exit 1
    ```
    - Purpose: Monitors the container's health.

---

## Docker and Kubernetes (K8s)

### The Relationship Between Docker and Kubernetes
- **Docker** focuses on containerization: creating, deploying, and running containers.
- **Kubernetes** is an orchestration platform designed to manage containers at scale across clusters.

### Benefits of Combining Docker and Kubernetes
1. **Scalability**: Kubernetes allows automatic scaling of Docker containers to handle varying loads.
2. **High Availability**: Kubernetes ensures containers are always running, restarting failed ones and spreading workloads across nodes.
3. **Load Balancing**: Kubernetes distributes network traffic across containers to optimize resource utilization.
4. **Simplified Deployment**: Kubernetes manages the deployment and updates of containers seamlessly.
5. **Integration with CI/CD**: Both tools integrate well to enable efficient and reliable software delivery pipelines.

### Use Cases
- Running microservices applications in production.
- Managing multi-container applications.
- Enabling DevOps practices like Continuous Deployment and Blue-Green Deployments.

By leveraging Docker and Kubernetes together, organizations achieve agility, resilience, and efficiency in their software delivery and operations.
