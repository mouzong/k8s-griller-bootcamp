# 🔥 SESSION 2 — Kubernetes Core Concepts

⏱️ **Duration:** 1h30
🎯 **Objective:** Understand the core architecture & components of Kubernetes and deploy your first application.

---

# 1. 🔍 Introduction to Kubernetes Architecture

## 1.1 What is Kubernetes?

Kubernetes (K8s) is an open-source container orchestration platform that automates:

* Deployment of applications
* Scaling
* Load balancing
* Updating applications with zero downtime
* Self-healing

---

# 2. 🏗️ Kubernetes Cluster Architecture

A Kubernetes cluster has **two main layers**:

## 2.1 Control Plane (Brain of the cluster)

The Control Plane ensures the cluster runs as expected.




### **API Server**

* Central communication hub of Kubernetes
* Exposes the K8s API used by `kubectl`, dashboard, controllers
* Validates YAML manifests and stores them in etcd

### **Scheduler**

* Assigns Pods to nodes
* Decides **where** a pod should run based on resources and constraints

### **Controller Manager**

Runs background controllers that ensure the desired state:

* Node Controller
* Deployment Controller
* ReplicaSet Controller
* Job Controller
* Etc.

### **etcd**

* Distributed key-value database storing the **entire cluster state**
* Critical; must be backed up regularly

---

## 2.2 Worker Nodes (Muscles of the cluster)

Nodes run your actual workloads.

### **Kubelet**

* Agent running on each node
* Ensures containers are running as instructed by the API Server

### **Container Runtime**

Examples: Docker, containerd, CRI-O

* Responsible for running containers

### **Kube-Proxy**

* Handles network rules
* Ensures pods and services can communicate
* Implements service load balancing

---

# 3. 🧱 Kubernetes Core Objects

## 3.1 Pods

* The **smallest deployable unit** in Kubernetes
* A Pod contains **one or more tightly coupled containers**
* Containers inside a Pod share:

  * Network namespace
  * Storage volumes
  * Lifecycle

**Pods are ephemeral**—K8s may kill and recreate them.

---

## 3.2 ReplicaSets

* Ensure a desired number of **identical pods**
* If a pod dies → RS automatically recreates it
* Rarely used directly; commonly controlled by Deployments

---

## 3.3 Deployments

The main workhorse for app deployment.

Features:

* Rolling updates
* Rollbacks
* Scaling
* Strategy control
* Declarative model

---

## 3.4 Services

Pods have ephemeral IPs → Services provide **stable networking**.

Types:

* **ClusterIP** (default): internal access
* **NodePort**: expose on each node’s IP
* **LoadBalancer**: managed cloud LB (AWS, GCP, Azure)
* **Headless**: service without cluster IP (for stateful apps)

---

## 3.5 ConfigMaps & Secrets

### **ConfigMaps**

* Store **non-sensitive** configuration data
* Inject into pods as:

  * Environment variables
  * Volume files

### **Secrets**

* Store **sensitive** data:

  * Passwords
  * Tokens
  * Certificates
* Base64 encoded (not encrypted unless using KMS / Vault)

---

# 4. 🧪 Hands-On Exercises

## 4.1 Deploy Your First App

### **Deployment YAML**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

### Apply:

```bash
kubectl apply -f nginx-deployment.yaml
```

### Check:

```bash
kubectl get pods
kubectl get rs
kubectl get deployments
```

---

## 4.2 Expose the App Through a Service

```bash
kubectl expose deployment nginx-deployment \
  --port=80 \
  --type=NodePort
```

Check exposed port:

```bash
kubectl get svc
```

---

## 4.3 Scaling a Deployment

Increase replicas:

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

---

## 4.4 Rolling Update

Update image:

```bash
kubectl set image deployment/nginx-deployment \
  nginx=nginx:1.25
```

Check rollout status:

```bash
kubectl rollout status deployment/nginx-deployment
```

Rollback if needed:

```bash
kubectl rollout undo deployment/nginx-deployment
```

---

# 5. 🧠 Key Takeaways

* **Control Plane** = decision-making layer
* **Worker nodes** = execution layer
* **Pods** are ephemeral → managed by ReplicaSets
* **Deployments** handle lifecycle, scaling, updates
* **Services** provide stable networking
* **ConfigMaps** and **Secrets** separate config from code
* Kubernetes ensures **desired state, self-healing, zero-downtime updates**
