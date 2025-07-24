# linux > docker

## Summary
> [!summary]
> Docker containerization platform for Linux

## Theory

### What is Docker?
Docker is a platform for developing, shipping, and running applications in containers.

### Key Concepts
- **Images** - Read-only templates for containers
- **Containers** - Running instances of images
- **Dockerfile** - Instructions to build images
- **Docker Compose** - Multi-container applications

### Basic Commands
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

## Questions

> [!tip]- When should I use Docker?
> Use Docker for application isolation, development environment consistency, and easy deployment.

- - -
#linux