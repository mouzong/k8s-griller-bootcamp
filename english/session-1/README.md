# 🧑‍🍳🔥 Session 1 — Docker Fundamentals & Containers Mindset

**Kubernetes Griller Bootcamp — Student Notes (1h30)**

---

# 📌 1. What Are Containers?

Containers are **lightweight, isolated environments** that run applications with all their required dependencies.

Before containers, deploying apps on different servers often caused issues like:

* “Works on my machine” problems
* Dependency conflicts
* Slow provisioning of environments
* Heavy virtual machines that waste resources

### ✔ Containers solve all of this.

---

## 🆚 Virtual Machines vs Containers

| Virtual Machines                  | Containers            |
| --------------------------------- | --------------------- |
| Heavy (GBs)                       | Lightweight (MBs)     |
| Boot in minutes                   | Start in milliseconds |
| Each VM has full OS               | Share host OS kernel  |
| Slow to deploy                    | Super fast            |
| Hard to move between environments | Portable everywhere   |

### 👉 Kubernetes does **NOT** run VMs.

It orchestrates **containers**, so mastering Docker is the foundation of Kubernetes.

---

# 📌 2. Why Docker?

Docker became the industry standard for containers because it provides:

### ✔ **Portability**

The same image works on any machine, cloud, or cluster.

### ✔ **Consistency**

The application + dependencies are packaged together.

### ✔ **Speed**

Start and deploy in seconds.

### ✔ **Scalability**

Perfect for microservices and cloud-native apps.

### ✔ **Developer productivity**

Easy to build, test, and share reproducibly.

---

# 📌 3. Docker Architecture (High-Level)

```
+-----------------------+
|      Docker CLI       |   → You run: docker build / run / push
+-----------------------+
             |
             v
+-----------------------+
|   Docker Daemon       |   → Builds images, runs containers
+-----------------------+
             |
             v
+-----------------------+
|     Docker Registry   |   → Stores and distributes images
+-----------------------+
```

Main components:

* **Docker Client (CLI):** You type commands
* **Docker Daemon:** Executes the commands
* **Docker Images:** Read-only templates
* **Docker Containers:** Running instances of images
* **Registries:** Docker Hub, GHCR, ECR, GCR, etc.

---

# 📌 4. Images, Layers & Registries

## 🏗️ Docker Image = Layered Blueprint

Each instruction in a Dockerfile creates one **immutable layer**.

Example:

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json .
RUN npm install --production
COPY . .
CMD ["node", "app.js"]
```

Layers (top to bottom):

1. Base image (node:18-alpine)
2. Create directory
3. Copy package.json
4. Install dependencies
5. Copy app code
6. Define startup command

### Why layers matter?

* Rebuilds are faster
* Pushing/pulling images is efficient
* Caches improve build times

---

# 📌 5. Writing Good Dockerfiles

To build production-ready containers:

### ✔ Use lightweight base images

```
FROM python:3.10-slim
FROM node:18-alpine
FROM golang:1.22-alpine
```

### ✔ Reduce layers

Combine RUN statements when possible.

### ✔ Copy only required files

```
COPY package*.json .
```

### ✔ Avoid running as root

```
USER node
```

### ✔ Use multi-stage builds (optional advanced)

* Build stage
* Runtime stage

---

# 📌 6. Build & Push Docker Images

## 👉 Build an image

```bash
docker build -t codegrill/app:1.0 .
```

## 👉 Run the container

```bash
docker run -p 8080:8080 codegrill/app:1.0
```

## 👉 Tag the image for Docker Hub

```bash
docker tag codegrill/app:1.0 <username>/app:1.0
```

## 👉 Push the image

```bash
docker push <username>/app:1.0
```

---

# 📌 7. Docker Networking Basics

Docker creates networks so containers can talk to each other.

### Default networks:

* `bridge`
* `host`
* `none`

### Creating your own network:

```bash
docker network create app-net
```

Run containers inside it:

```bash
docker run -d --network app-net --name api codegrill/api
docker run -d --network app-net --name db mysql
```

Now they reach each other using DNS-like names:

```
api → http://db
db → http://api
```

---

# 📌 8. Docker Volumes (Persistence)

Containers are temporary. When they restart, data disappears.

Volumes solve that.

### Create volume:

```bash
docker volume create app-data
```

### Attach volume:

```bash
docker run -d \
  -v app-data:/var/lib/mysql \
  mysql
```

---

# 📌 9. Hands-On Labs

---

## 🧪 LAB 1 — Build a Simple Node.js App

### Lab Prerequisites:
- docker
- vscode
- git 

Create project folder and open it in vscode editor:

```bash
# Open GitBash or any other terminal emulator

# Create the lab folder
mkdir docker-lab

# Navigate into the lab folder
cd docker-lab

# open the lab folder in Visual Studio Code (vscode)
code .
```

### Create `app.js`:

```js
const express = require('express');
const app = express();

app.get("/", (req, res) => res.send("🔥 CodeGrill Docker Lab!"));
app.listen(3000, () => console.log("Server running on port 3000"));
```

### Create `package.json`:

```json
{
  "name": "docker-lab",
  "dependencies": {
    "express": "4.18.2"
  }
}
```

---

## 🧪 LAB 2 — Create Dockerfile

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json .
RUN npm install --production
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]
```

---

## 🧪 LAB 3 — Build the image

```bash
docker build -t codegrill/docker-lab:1.0 .
```

Run it:

```bash
docker run -p 3000:3000 codegrill/docker-lab:1.0
```

Open in browser:

```
http://localhost:3000
```

---

## 🧪 LAB 4 — Push Image to Docker Hub

Login:

```bash
docker login
```

Tag:

```bash
docker tag codegrill/docker-lab:1.0 <username>/docker-lab:1.0
```

Push:

```bash
docker push <username>/docker-lab:1.0
```

---

## 🧪 LAB 5 — Using Docker Compose (Multi-Container App)

Create `docker-compose.yml`:

```yaml
version: "3.9"
services:
  api:
    image: <username>/docker-lab:1.0
    ports:
      - "3000:3000"
    networks:
      - app-net

  redis:
    image: redis:alpine
    networks:
      - app-net

networks:
  app-net:
```

Run:

```bash
docker compose up -d
```

See containers:

```bash
docker ps
```

Stop:

```bash
docker compose down
```

---

# 📌 10. End of Session Summary

After this session, you should understand:

✔ What containers are & why they matter
✔ Docker architecture
✔ How images & layers work
✔ How to write Dockerfiles
✔ How to build, tag, run, and push images
✔ How networks allow containers to communicate
✔ How volumes persist data
✔ How to run multi-container applications

You are now ready for **Session 2 — Kubernetes Core Concepts**.
