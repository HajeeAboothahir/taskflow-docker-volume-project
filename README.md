# TaskFlow – Docker Containerization & Persistent Storage

## Project Overview

This project demonstrates the containerization and deployment of a Python-based web application using Docker.

The primary objective is to understand Docker fundamentals and implement persistent storage for containerized applications using Docker Named Volumes.

The project simulates a real-world DevOps scenario where application containers can be removed and recreated without losing critical application data.

## DevOps Objectives

- Containerize an application using Docker.
- Build and manage custom Docker images.
- Deploy and manage Docker containers.
- Configure host-to-container port mapping.
- Implement persistent storage using Docker Named Volumes.
- Understand container lifecycle management.
- Verify data persistence after container deletion and recreation.
- Perform application health verification.
- Deploy a containerized application on AWS EC2.

## Technology Stack

| Technology | Purpose |
|---|---|
| AWS EC2 | Cloud deployment |
| Linux | Server administration |
| Docker | Containerization |
| Dockerfile | Image creation |
| Docker Named Volumes | Persistent storage |
| Git | Version control |
| GitHub | Source code management |
| Python Flask | Sample application |

## Architecture

```text
             User / Browser
                   |
                   |
             EC2 Public IP
                   |
               Port 3000
                   |
                   v
           Docker Container
               Port 5000
                   |
                   v
             Flask Application
                   |
                   v
              SQLite DB
                   |
                   v
            Docker Volume
             taskflow-data
                   |
                   v
               tasks.db
```

## Project Structure

```text
taskflow-project/
│
├── app.py
├── requirements.txt
├── Dockerfile
├── .dockerignore
├── .gitignore
├── README.md
│
├── templates/
│   └── index.html
│
└── static/
    └── style.css
```

## Implementation Steps

### 1. Build Docker Image

```bash
docker build -t taskflow:v1 .
```

Verify the image:

```bash
docker images
```

### 2. Create Docker Named Volume

```bash
docker volume create taskflow-data
```

Verify:

```bash
docker volume ls
```

### 3. Deploy the Container

```bash
docker run -d \
--name taskflowcontainer \
-p 3000:5000 \
-v taskflow-data:/data \
taskflow:v1
```

### 4. Verify Container Status

```bash
docker ps
```

View application logs:

```bash
docker logs taskflowcontainer
```

### 5. Verify Application Health

```bash
curl http://localhost:3000/health
```

Expected response:

```json
{"status": "healthy"}
```

### 6. Verify Docker Volume

```bash
docker volume inspect taskflow-data
```

Check database storage on the EC2 host:

```bash
sudo ls -lah /var/lib/docker/volumes/taskflow-data/_data/
```

Expected database file:

```text
tasks.db
```

## Practical DevOps Scenario: Persistent Storage

### Problem

When a Docker container is deleted, data stored inside its writable container layer is lost.

For stateful applications, this can result in data loss.

### Solution

Docker Named Volumes provide persistent storage independently of the container lifecycle.

### Test 1: Remove the Existing Container

```bash
docker rm -f taskflowcontainer
```

### Test 2: Verify the Volume

```bash
docker volume ls
```

The `taskflow-data` volume should still exist.

### Test 3: Recreate the Container

```bash
docker run -d \
--name taskflowcontainer \
-p 3000:5000 \
-v taskflow-data:/data \
taskflow:v1
```

### Test 4: Verify Application Data

Access the application again and verify that previously created tasks are available.

**Result:** Successfully verified persistent storage after container deletion and recreation.

## Important Docker Commands

| Command | Purpose |
|---|---|
| `docker build` | Build an image |
| `docker images` | List images |
| `docker run` | Create and start a container |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `docker logs` | View container logs |
| `docker inspect` | Inspect container configuration |
| `docker exec -it` | Execute commands inside a container |
| `docker stop` | Stop a container |
| `docker rm -f` | Remove a container |
| `docker volume create` | Create a named volume |
| `docker volume ls` | List volumes |
| `docker volume inspect` | Inspect a volume |

## Key DevOps Concepts Demonstrated

- Docker image creation and container deployment.
- Container lifecycle management.
- Port mapping and application accessibility.
- Persistent storage using Docker Named Volumes.
- Separation of application containers and persistent data.
- Container replacement without application data loss.
- Basic application health verification.
- Cloud-based container deployment using AWS EC2.

## Future Enhancements

- Docker Hub image publishing.
- Jenkins CI/CD pipeline integration.
- Automated Docker image builds.
- Kubernetes deployment using AWS EKS.
- Persistent Volumes and Persistent Volume Claims.
- Helm-based application deployment.
- Prometheus and Grafana monitoring.
- Trivy container image vulnerability scanning.

## Author

**Hajee Syed Aboothahir**

GitHub: https://github.com/HajeeAboothahir

---

**Key Takeaway:** Containers are replaceable, but application data must be persistent. This project demonstrates how Docker Named Volumes help preserve application data independently of the container lifecycle.
