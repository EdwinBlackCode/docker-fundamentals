# Docker Fundamentals

A beginner-friendly repository to learn Docker fundamentals on Linux.

This repository explains:

- What Docker is
- Why containers are useful
- Docker architecture
- Docker vs Virtual Machines
- Installing Docker on Linux
- Docker images and containers
- Volumes and persistence
- Networking basics
- Useful Docker commands
- Common troubleshooting scenarios

The goal of this project is educational.
It is designed for developers, data analysts, DevOps beginners, data engineers, and anyone who wants to understand modern containerized environments.

---

# Table of Contents

- [What is Docker?](#what-is-docker)
- [Why Use Docker?](#why-use-docker)
- [Docker vs Virtual Machines](#docker-vs-virtual-machines)
- [Docker Architecture](#docker-architecture)
- [Requirements](#requirements)
- [Install Docker on Ubuntu](#install-docker-on-ubuntu)
- [Verify Installation](#verify-installation)
- [Run Your First Container](#run-your-first-container)
- [Docker Images](#docker-images)
- [Docker Containers](#docker-containers)
- [Useful Docker Commands](#useful-docker-commands)
- [Docker Volumes](#docker-volumes)
- [Docker Networks](#docker-networks)
- [Docker Compose](#docker-compose)
- [Troubleshooting](#troubleshooting)
- [Repository Structure](#repository-structure)
- [Learning Goals](#learning-goals)
- [Useful Resources](#useful-resources)

---

# What is Docker?

Docker is a platform that allows developers to package and run applications inside lightweight, isolated environments called containers.

Containers include:

- Application code
- Dependencies
- Libraries
- Runtime environment
- Configuration

This makes applications portable and reproducible across different environments.

Example:

A PostgreSQL database running in Docker will behave the same on:

- Ubuntu
- Windows
- macOS
- Cloud servers
- CI/CD pipelines

---

# Why Use Docker?

Docker solves many common development and deployment problems.

## Benefits

### Portability

Applications run consistently across environments.

### Isolation

Each container has its own environment.

### Lightweight

Containers share the host operating system kernel.

### Fast Startup

Containers start in seconds.

### Reproducibility

Teams can reproduce the exact same environment.

### Scalability

Containers work well with orchestration systems like Kubernetes.

---

# Docker vs Virtual Machines

| Feature | Docker Containers | Virtual Machines |
|---|---|---|
| Startup Time | Seconds | Minutes |
| Resource Usage | Low | High |
| Includes Full OS | No | Yes |
| Performance | Near-native | Higher overhead |
| Isolation | Process-level | Hardware-level |
| Portability | Excellent | Good |

## Virtual Machines

Virtual Machines include:

- Full guest operating system
- Virtual hardware
- Hypervisor

## Docker Containers

Containers share the host OS kernel.

This makes them:

- Smaller
- Faster
- More efficient

---

# Docker Architecture

Docker uses a client-server architecture.

## Docker Client

The command line interface used by users.

Example:

```bash
docker ps
```

## Docker Daemon

The background service responsible for:

- Building images
- Running containers
- Managing networks
- Managing volumes

## Docker Engine

Core runtime environment.

## Docker Hub

Public registry for Docker images.

Official images:

- PostgreSQL
- MySQL
- Redis
- MongoDB
- NGINX

---

# Requirements

## Operating System

Recommended:

- Ubuntu 22.04+

## Permissions

- sudo access

## Recommended Resources

- 4 GB RAM minimum
- 10 GB free disk space

---

# Install Docker on Ubuntu

This installation uses the official Docker repository.

## Step 1 — Remove Old Versions

```bash
sudo apt remove docker docker-engine docker.io containerd runc
```

This removes older Docker packages that may conflict with the latest installation.

---

## Step 2 — Update Package Index

```bash
sudo apt update
```

Updates the list of available packages.

---

## Step 3 — Install Required Packages

```bash
sudo apt install ca-certificates curl gnupg
```

These packages are required to:

- Download packages securely
- Verify repository signatures

---

## Step 4 — Add Docker GPG Key

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

This adds Docker's official signing key.

---

## Step 5 — Add Docker Repository

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

This adds the official Docker repository.

---

## Step 6 — Update Package Index Again

```bash
sudo apt update
```

Refreshes package information including the new Docker repository.

---

## Step 7 — Install Docker Engine

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

This installs:

- Docker Engine
- Docker CLI
- Container runtime
- Buildx
- Docker Compose

---

# Verify Installation

## Check Docker Version

```bash
docker --version
```

Example output:

```bash
Docker version 28.x.x
```

---

## Check Docker Service

```bash
sudo systemctl status docker
```

The service should appear as:

```bash
active (running)
```

---

# Run Your First Container

## Hello World Example

```bash
docker run hello-world
```

What happens:

1. Docker searches for the image locally
2. If missing, Docker downloads it from Docker Hub
3. Docker creates a container
4. The container runs and exits

---

# Docker Images

Images are read-only templates used to create containers.

Examples:

- Ubuntu
- PostgreSQL
- Redis
- MongoDB

## Pull an Image

```bash
docker pull postgres:17
```

## List Images

```bash
docker images
```

## Remove an Image

```bash
docker rmi postgres:17
```

---

# Docker Containers

Containers are running instances of Docker images.

## Run a Container

```bash
docker run -it ubuntu bash
```

## List Running Containers

```bash
docker ps
```

## List All Containers

```bash
docker ps -a
```

## Stop a Container

```bash
docker stop container_name
```

## Start a Container

```bash
docker start container_name
```

## Remove a Container

```bash
docker rm container_name
```

---

# Useful Docker Commands

## Download an Image

```bash
docker pull nginx
```

## Run an NGINX Web Server

```bash
docker run -d -p 8080:80 nginx
```

## View Logs

```bash
docker logs container_name
```

## Execute Commands Inside a Container

```bash
docker exec -it container_name bash
```

## View Resource Usage

```bash
docker stats
```

## Remove Unused Resources

```bash
docker system prune
```

---

# Docker Volumes

Volumes provide persistent storage for containers.

Without volumes:

- Data disappears when containers are deleted.

With volumes:

- Data survives container recreation.

## Create a Volume

```bash
docker volume create postgres_data
```

## List Volumes

```bash
docker volume ls
```

## Remove a Volume

```bash
docker volume rm postgres_data
```

---

# Docker Networks

Networks allow containers to communicate.

## Create a Network

```bash
docker network create app_network
```

## List Networks

```bash
docker network ls
```

## Remove a Network

```bash
docker network rm app_network
```

---

# Docker Compose

Docker Compose allows managing multiple containers using YAML configuration files.

Example:

```yaml
services:
  postgres:
    image: postgres:17
    ports:
      - "5432:5432"
```

## Start Services

```bash
docker compose up -d
```

## Stop Services

```bash
docker compose down
```

---

# Troubleshooting

## Permission Denied Error

Error:

```bash
permission denied while trying to connect to the Docker daemon socket
```

Solution:

```bash
sudo usermod -aG docker $USER
```

Then:

- Log out
- Log back in

---

## Docker Service Not Running

Start Docker manually:

```bash
sudo systemctl start docker
```

Enable Docker at boot:

```bash
sudo systemctl enable docker
```

---

## Remove All Containers

```bash
docker rm -f $(docker ps -aq)
```

---

## Remove All Images

```bash
docker rmi -f $(docker images -aq)
```

---

# Repository Structure

```text
learn-docker-from-zero/
│
├── README.md
├── docs/
├── examples/
├── scripts/
└── images/
```

---

# Learning Goals

After completing this repository you should understand:

- Docker fundamentals
- Images and containers
- Installing Docker on Linux
- Container lifecycle management
- Docker networking basics
- Persistent storage with volumes
- Basic Docker Compose usage

---

# Useful Resources

## Official Documentation

- https://docs.docker.com/

## Docker Hub

- https://hub.docker.com/

## Ubuntu Documentation

- https://ubuntu.com/

---

# Future Topics

Planned future repositories:

- Docker Compose Deep Dive
- PostgreSQL with Docker
- MongoDB with Docker
- Redis with Docker
- SQL Server with Docker
- Airflow + PostgreSQL
- Data Engineering Labs
- Kubernetes Basics

---

# License

This project is licensed under the MIT License.

---

# Author

Created by Edwin Javier Torres Reyes

- GitHub: https://github.com/EdwinBlackCode
- LinkedIn: https://www.linkedin.com/in/edwintorresreyes/

Focused on:
- Data Analytics
- Docker
- SQL
- Data Engineering
- Salesforce

