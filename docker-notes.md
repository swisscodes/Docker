# 🐋 The Ultimate Docker Cheatsheet

A practical reference guide covering daily container management, image building, volume persistence, networking, and system cleanup.

1. ## System Info & Diagnostics

## Essential commands to inspect your local Docker installation and runtime environment

### Display system-wide information (containers, images, driver details)

    docker info

### Show client and server daemon versions

    docker version

### Display real-time resource usage statistics (CPU, memory, network I/O) for all running containers

    docker stats

2. # Managing Images

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

3. # Container Lifecycle (Creating & Running)

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

4. # Inspecting & Interacting with Running Containers

## Commands to debug, check logs, or jump inside active containers

### List running containers

    docker ps

### List ALL containers (running + stopped)

    docker ps -a

### List only container IDs (useful for scripts)

    docker ps -aq

### View live container stdout/stderr logs

    docker logs my-web-server

### Follow live log output continuously (-f) with timestamps (-t)

    docker logs -ft my-web-server

### Execute a command inside an ALREADY RUNNING container

    docker exec -it my-web-server bash

### Inspect detailed low-level JSON configuration of a container

    docker inspect my-web-server

### View processes currently running inside a container

    docker top my-web-server

5. # Stopping & Removing Containers

### Gracefully stop a running container (SIGTERM)

    docker stop my-web-server

### Force-kill a running container immediately (SIGKILL)

    docker kill my-web-server

### Start a previously stopped container

    docker start my-web-server

### Remove a stopped container

    docker rm my-web-server

### Force-remove a running container

    docker rm -f my-web-server

### Stop ALL running containers (PowerShell / Bash)

    docker stop $(docker ps -q)

### Remove ALL stopped containers

    docker rm $(docker ps -aq -f "status=exited")

6. # Volumes & Data Persistence

## Volumes decouple persistent data from the container lifecycle

### Create a named volume

    docker volume create my-db-data

### List all volumes

    docker volume ls

### Inspect volume location on host drive

    docker volume inspect my-db-data

### Mount a named volume to a container (-v volume_name:container_path)

    docker run -d --name postgres-db -v my-db-data:/var/lib/postgresql/data postgres:16

### Mount a local host folder directly (Bind Mount)

    docker run -d -p 8080:80 -v C:\Users\Admin\app:/usr/share/nginx/html nginx

### Delete an unused volume

    docker volume rm my-db-data

7. # Docker Networking

## Connect containers together on custom isolated virtual networks

### List available networks (bridge, host, none)

    docker network ls

### Create a custom user-defined bridge network

    docker network create my-app-net

### Run containers attached to the custom network (enables automatic DNS by container name)

    docker run -d --name db --network my-app-net postgres:16
    docker run -d --name web --network my-app-net -p 8080:80 my-web-app

### Connect an existing running container to a network

    docker network connect my-app-net my-web-server

### Remove a network

    docker network rm my-app-net

8. # Docker Compose

    Manage multi-container applications using docker-compose.yml

### Start all services defined in docker-compose.yml in background

    docker compose up -d

### Build images first, then start containers

    docker compose up -d --build

### View aggregated logs from all services in real time

    docker compose logs -f

### List containers managed by the current Compose file

    docker compose ps

### Stop and remove all containers, networks, and volumes defined in the Compose file

    docker compose down

### Stop and remove containers AND delete associated named volumes (-v)

    docker compose down -v

9. # System Cleanup & Maintenance

## Reclaim disk space by clearing out unused images, stopped containers, and dangling build caches

### Remove all stopped containers, unused networks, and dangling images

    docker system prune

### Comprehensive purge: remove ALL unused containers, networks, images (not just dangling ones), and build cache

    docker system prune -a --volumes

### View current Docker disk space consumption

    docker system df
