# LINUX > DOCKER

## SUMMARY
> [!summary]
> Docker containerization platform for Linux

## THEORY

### WHAT IS DOCKER?
Docker is a platform for developing, shipping, and running applications in containers.

### KEY CONCEPTS
- **Images** - Read-only templates for containers
- **Containers** - Running instances of images
- **Dockerfile** - Instructions to build images
- **Docker Compose** - Multi-container applications

### BASIC COMMANDS
```bash
# List running containers
docker ps

# Run a container
docker run -it ubuntu bash

# Build image from Dockerfile
docker build -t myapp .

# Use Docker Compose
docker-compose up -d
```

## QUESTIONS

> [!tip]- When should I use Docker?
> Use Docker for application isolation, development environment consistency, and easy deployment.

- - -
