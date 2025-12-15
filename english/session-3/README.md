# 🔥 SESSION 3 (1h30) — Kubernetes Networking & Ingress

⏱ **Duration:** 1h30
🎯 **Goal:** Help trainees understand how applications communicate **inside** and **outside** a Kubernetes cluster, and how traffic is exposed using **Services** and **Ingress**.

---

## 🎯 Learning Objectives

By the end of this session, trainees will be able to:

* Understand how **networking works inside a Kubernetes cluster**
* Explain and use different **Service types**
* Understand **DNS resolution** inside Kubernetes
* Expose applications using an **Ingress Controller**
* Configure **multi-path routing** to multiple services

---

## 📘 Part 1 — Kubernetes Networking Basics (15 min)

### 🔹 Key Concepts

* Every **Pod gets its own IP address**
* Pods can communicate **directly with each other**
* Pod IPs are **ephemeral** (change when Pods restart)
* **Services** provide a **stable network identity**

👉 This is why Services are essential in Kubernetes.

---

## 📘 Part 2 — Kubernetes Services (30 min)

### 🔹 What is a Service?

A **Service** exposes a set of Pods using:

* A **stable IP**
* A **DNS name**
* **Load balancing** between Pods

---

### 🔹 Service Types Explained

#### 1️⃣ ClusterIP (Default)

* Exposes the application **inside the cluster only**
* Used for **internal communication** (backend → database, API → API)

```bash
kubectl expose deployment my-app \
  --type=ClusterIP \
  --port=80
```

📌 Example use case:

* Backend API calling another internal service

---

#### 2️⃣ NodePort

* Exposes the application on **each node’s IP**
* Accessible via:
  `NodeIP:NodePort`

```bash
kubectl expose deployment my-app \
  --type=NodePort \
  --port=80
```

📌 Example use case:

* Local development
* Quick testing without Ingress

⚠️ Limitations:

* Port range: `30000–32767`
* Not recommended for production

---

#### 3️⃣ LoadBalancer

* Exposes the application using a **cloud provider load balancer**
* Automatically assigns an external IP

```bash
kubectl expose deployment my-app \
  --type=LoadBalancer \
  --port=80
```

📌 Example use case:

* Production environments on AWS / GCP / Azure

---

## 📘 Part 3 — DNS in Kubernetes (10 min)

### 🔹 How DNS Works

Kubernetes uses **CoreDNS** to resolve service names.

Service DNS format:

```text
service-name.namespace.svc.cluster.local
```

Example:

```text
backend.default.svc.cluster.local
```

✔ Pods can call services using **service name only**:

```bash
curl http://backend
```

📌 Benefits:

* No hardcoded IPs
* Automatic service discovery

---

## 📘 Part 4 — Ingress & Ingress Controllers (20 min)

### 🔹 What is Ingress?

An **Ingress** manages **HTTP/HTTPS traffic** from outside the cluster to internal services.

Instead of exposing many services, you expose **one entry point**.

---

### 🔹 Ingress Controller

Ingress rules require a controller to work.

Common controllers:

* **NGINX Ingress Controller**
* **Gateway API** (next-generation approach)

---

### 🔹 Ingress Flow

```text
Client → Ingress Controller → Service → Pod
```

---

## 📘 Part 5 — Hands-on Lab (15 min)

### 🧪 Lab 1 — Deploy an Ingress Controller (NGINX)

```bash
kubectl apply -f \
https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

Verify:

```bash
kubectl get pods -n ingress-nginx
```

---

### 🧪 Lab 2 — Deploy Two Applications

Example services:

* `app1-service`
* `app2-service`

Each service exposes port 80.

---

### 🧪 Lab 3 — Multi-Path Routing Ingress

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: demo-ingress
spec:
  rules:
  - host: demo.local
    http:
      paths:
      - path: /app1
        pathType: Prefix
        backend:
          service:
            name: app1-service
            port:
              number: 80
      - path: /app2
        pathType: Prefix
        backend:
          service:
            name: app2-service
            port:
              number: 80
```

Apply:

```bash
kubectl apply -f ingress.yaml
```

Test:

```bash
curl http://demo.local/app1
curl http://demo.local/app2
```

📌 (Add `demo.local` to `/etc/hosts` if needed)

---

## ✅ Key Takeaways

* **Services** provide stable networking in Kubernetes
* **ClusterIP** → internal
* **NodePort** → simple external access
* **LoadBalancer** → cloud-native exposure
* **Ingress** centralizes HTTP routing
* DNS makes service communication easy and scalable
