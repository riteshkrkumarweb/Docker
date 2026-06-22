# 🐳 Docker Complete Step-by-Step Documentation

# 1. What is Docker?

Docker is a containerization platform that allows developers to package an application along with all its dependencies into a portable and isolated environment called a **Container**.

### Why Docker?

Without Docker:

```text id="34z9x5"
My Machine
├── Python 3.11
├── FastAPI
├── Pandas
├── Scikit-Learn
└── Other Dependencies
```

Problem:

* Works on your machine
* May fail on another machine

With Docker:

```text id="d9z87m"
Docker Container
├── Python
├── FastAPI
├── ML Model
├── Dependencies
└── Application Code
```

Result:

* Runs consistently everywhere
* Easy deployment
* No dependency conflicts

---

# 2. Docker Architecture

```text id="n5k3qz"
Application Code
       │
       ▼
Dockerfile
       │
       ▼
Docker Build
       │
       ▼
Docker Image
       │
       ▼
Docker Run
       │
       ▼
Docker Container
```

---

# 3. Important Docker Terms

| Term       | Meaning                          |
| ---------- | -------------------------------- |
| Docker     | Containerization Platform        |
| Image      | Blueprint of an Application      |
| Container  | Running Instance of an Image     |
| Dockerfile | Instructions to Build an Image   |
| Docker Hub | Cloud Storage for Images         |
| Registry   | Storage for Docker Images        |
| Volume     | Persistent Storage               |
| Network    | Communication Between Containers |
| Build      | Create Image                     |
| Run        | Start Container                  |
| Tag        | Version Identifier               |

---

# 4. Install Docker

Install Docker Desktop.

Verify Installation:

```bash id="2ugr9i"
docker --version
```

```bash id="ndrfm2"
docker compose version
```

---

# 5. Create Project Structure

Example FastAPI Project:

```text id="0t8ep0"
insurance-api/
│
├── app.py
├── requirements.txt
├── Dockerfile
└── .dockerignore
```

---

# 6. Create Dockerfile

Dockerfile tells Docker how to build the image.

Example:

```dockerfile id="pf8hy5"
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["uvicorn","app:app","--host","0.0.0.0","--port","8000"]
```

---

# 7. Create .dockerignore

```text id="x8g4jv"
venv/
.git/
__pycache__/
.env
*.log
```

Purpose:

* Faster Builds
* Smaller Images
* Better Security

---

# 8. Build Docker Image

### Syntax

```bash id="r1nyh7"
docker build -t image_name .
```

### Example

```bash id="0u5a1q"
docker build -t insurance-api .
```

Explanation:

```text id="f7dr5x"
docker build    → Build Image
-t              → Tag
insurance-api   → Image Name
.               → Current Directory
```

Verify:

```bash id="z3l1wh"
docker images
```

---

# 9. Run Docker Container

### Syntax

```bash id="5wq1l2"
docker run image_name
```

### Example

```bash id="4hh68q"
docker run insurance-api
```

For Web Applications:

```bash id="j52m1v"
docker run -p 8000:8000 insurance-api
```

Explanation:

```text id="u6bhw4"
Host Port  → Container Port

8000       → 8000
```

Access:

```text id="bz8wot"
http://localhost:8000
```

Swagger:

```text id="xg74jq"
http://localhost:8000/docs
```

---

# 10. Check Running Containers

```bash id="tf06lg"
docker ps
```

All Containers:

```bash id="m08n7m"
docker ps -a
```

---

# 11. View Logs

```bash id="p67xqz"
docker logs container_id
```

Live Logs:

```bash id="6shyfa"
docker logs -f container_id
```

---

# 12. Access Container Terminal

```bash id="qfshxa"
docker exec -it container_id bash
```

or

```bash id="1im7ga"
docker exec -it container_id sh
```

---

# 13. Push Image to Docker Hub

## Step 1: Login

```bash id="cb7oz6"
docker login
```

---

## Step 2: Create Repository on Docker Hub

Example Repository:

```text id="3txwfw"
username/insurance-api
```

---

## Step 3: Tag Image

Syntax:

```bash id="scltvr"
docker tag local_image username/repository:tag
```

Example:

```bash id="o44p4h"
docker tag insurance-api username/insurance-api:latest
```

---

## Step 4: Push Image

```bash id="5a5h7z"
docker push username/insurance-api:latest
```

---

## Step 5: Verify Upload

Repository now contains:

```text id="2mq4a6"
username/insurance-api:latest
```

---

# 14. Pull Image Anywhere

Download Image:

```bash id="qgpp0u"
docker pull username/insurance-api:latest
```

Run:

```bash id="vq68ll"
docker run -p 8000:8000 username/insurance-api:latest
```

---

# 15. Docker Volumes

Problem:

```text id="61a5vg"
Container Deleted
       ↓
Data Lost
```

Solution:

Volume

Create:

```bash id="zv5vd6"
docker volume create myvolume
```

Use:

```bash id="c0v4ht"
docker run -v myvolume:/data insurance-api
```

---

# 16. Docker Networks

Create Network:

```bash id="is7mnf"
docker network create mynetwork
```

Run Containers:

```bash id="jg8n9j"
docker run --network=mynetwork app1
```

```bash id="g5k9ng"
docker run --network=mynetwork app2
```

Containers can communicate.

---

# 17. Docker Compose

Used when multiple containers are required.

Example:

```yaml id="a0l7ti"
services:
  app:
    build: .
    ports:
      - "8000:8000"

  database:
    image: postgres
```

Start:

```bash id="4y4gm0"
docker compose up
```

Background Mode:

```bash id="8wew67"
docker compose up -d
```

Stop:

```bash id="55dbw9"
docker compose down
```

---

# 18. Image Versioning

Instead of using only latest:

```bash id="p3ytv5"
docker tag insurance-api username/insurance-api:v1.0
```

Push:

```bash id="3hyk53"
docker push username/insurance-api:v1.0
```

Future Versions:

```text id="s4j1x5"
v1.0
v1.1
v2.0
latest
```

---

# 19. Complete Real-World Docker Workflow

```text id="ik5pgn"
Write Application
        │
        ▼
Create Dockerfile
        │
        ▼
Create .dockerignore
        │
        ▼
docker build
        │
        ▼
Docker Image
        │
        ▼
docker run
        │
        ▼
Test Application
        │
        ▼
docker login
        │
        ▼
docker tag
        │
        ▼
docker push
        │
        ▼
Docker Hub
        │
        ▼
docker pull
        │
        ▼
Run Anywhere
        │
        ▼
Deployment
```

---

# 20. Most Important Docker Commands

```bash id="kjlwmq"
docker --version

docker images

docker ps

docker ps -a

docker build -t app .

docker run app

docker run -p 8000:8000 app

docker stop container_id

docker rm container_id

docker rmi image_name

docker logs container_id

docker exec -it container_id bash

docker login

docker logout

docker tag image username/image:latest

docker push username/image:latest

docker pull username/image:latest

docker volume ls

docker network ls

docker compose up

docker compose down
```

# Learning Order

1. Docker Definition
2. Docker Architecture
3. Images vs Containers
4. Dockerfile
5. .dockerignore
6. Build Image
7. Run Container
8. Check Logs
9. Docker Hub (Login → Tag → Push → Pull)
10. Volumes
11. Networks
12. Docker Compose
13. Deployment
14. CI/CD with Docker
