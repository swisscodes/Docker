# 🐋 The Ultimate Docker Cheatsheet

A practical reference guide covering daily container management, image building, volume persistence, networking, and system cleanup.

1. System Info & Diagnostics

## Essential commands to inspect your local Docker installation and runtime environment

### Display system-wide information (containers, images, driver details)

    docker info

### Show client and server daemon versions

    docker version

### Display real-time resource usage statistics (CPU, memory, network I/O) for all running containers

    docker stats

2. Managing Images

## Images are read-only templates used to create Docker containers

### List all top-level images locally

    docker images

### List ALL images (including hidden intermediate layers)

    docker images -a

### Pull an image from Docker Hub without running it

    docker pull nginx:alpine

### Build an image from a Dockerfile in the current directory

    docker build -t my-app:1.0 .

### Build an image using a specific Dockerfile path

    docker build -f docker/Dockerfile.dev -t my-app:dev .

### Remove a specific image

    docker rmi nginx:alpine

### Force-remove an image (even if referenced by a stopped container)

    docker rmi -f my-app:1.0

3. Container Lifecycle (Creating & Running)

## Commands for running, stopping, and restarting containers

### Run a basic container (fetches image automatically if missing)

    docker run hello-world

### Run in background / detached mode (-d) with port mapping (-p host:container)

    docker run -d -p 8080:80 nginx

### Run with a custom container name (--name)

    docker run -d --name my-web-server -p 8080:80 nginx

### Interactive terminal session inside a container (-it)

    docker run -it ubuntu bash

### Run a temporary container that automatically deletes itself on exit (--rm)

    docker run --rm python:3.11 python -c "print('Temporary task complete')"

### Pass environment variables (-e)

    docker run -d --name my-postgres -e POSTGRES_PASSWORD=secret-pass postgres:16

### Set automatic restart policy (--restart)

    docker run -d --name my-db --restart unless-stopped postgres:16
