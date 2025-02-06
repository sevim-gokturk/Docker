# Docker Compose Basics

## Introduction

Docker Compose is a tool for managing multiple **Docker containers** using a simple **YAML** file. It is useful for **microservices**, testing, and multi-container applications.

## Installation

Before using Docker Compose, make sure you have **Docker** installed.
- **Docker Desktop** includes Docker Compose.
- On **Linux**, you may need to install it manually.

## docker-compose.yml File

A `docker-compose.yml` file defines **services** (containers) and how they work together.

### Basic Structure

```yaml
version: '3.8'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  database:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
```

### Key Elements
- **services**: Defines the containers.
- **image**: Specifies the Docker image.
- **build**: Uses a `Dockerfile` to build an image.
- **ports**: Maps host and container ports.
- **volumes**: Shares data between host and container.
- **environment**: Sets environment variables.

## Basic Commands

- `docker-compose up` → Start containers.
- `docker-compose down` → Stop and remove containers.
- `docker-compose ps` → List running containers.
- `docker-compose logs` → Show logs.
- `docker-compose exec <service_name> <command>` → Run a command inside a container.
- `docker-compose build` → Build an image from a `Dockerfile`.

## Multi-Container Setup

Docker Compose allows different services to communicate easily.

```yaml
version: '3.8'
services:
  app:
    build: .
    depends_on:
      - database
  database:
    image: postgres
```

### Networking
- **Docker Compose** automatically creates a **network** for all services.
- Containers can communicate using their **service name**.

## Conclusion

Docker Compose simplifies running multiple **Docker containers**.

