# 🚀 KUBERNETES GRILLER BOOTCAMP – 12H PROGRAM


## 🔥SESSION 1 (1h30) — Docker Fundamentals & Containers Mindset

### 🎯 Objectives

* Understand containerization
* Build foundational Docker skills for Kubernetes

### 📘 Content

* What is a container? Why Docker
* Images, layers, registries
* Dockerfile: best practices
* Build & push images
* Container networking & volumes
* Hands-on:

  * Build a simple app
  * Push to Docker Hub / GHCR
  * Run multi-container apps with Docker Compose

---

# 🔥 SESSION 2 (1h30) — Kubernetes Core Concepts

### 🎯 Objectives

* Understand Kubernetes architecture & components

### 📘 Content

* Kubernetes cluster architecture

  * API Server, Scheduler, Kubelet, etc.
* Pods, ReplicaSets, Deployments, Services
* ConfigMaps & Secrets
* Hands-on:

  * Deploy first app
  * Scaling / Rollouts / Rollbacks

---

# 🔥 SESSION 3 (1h30) — Kubernetes Networking & Ingress

### 🎯 Objectives

* Learn how traffic flows into clusters
* Expose apps properly

### 📘 Content

* ClusterIP, NodePort, LoadBalancer
* Ingress Controllers (NGINX)
* DNS inside Kubernetes
* Hands-on:

  * Deploy an Ingress
  * Multi-path routing to 2 services

---

# 🔥 SESSION 4 (1h30) — Storage & Stateful Applications

### 🎯 Objectives

* Manage persistence on Kubernetes

### 📘 Content

* Persistent Volumes
* Persistent Volume Claims
* StorageClasses
* StatefulSets
* Hands-on:

  * Deploy a MySQL/Postgres DB
  * Bind PV/PVC
  * Backup strategy

---

# 🔥 SESSION 5 (1h30) — Helm & App Packaging

### 🎯 Objectives

* Learn how to package & reuse deployments

### 📘 Content

* Why Helm?
* Charts, templates, values
* Releasing & versioning
* Hands-on:

  * Create your own Helm chart
  * Deploy application with Helm
  * Use popular community charts

---

# 🔥 SESSION 6 (1h30) — CI/CD to Kubernetes

### 🎯 Objectives

* Automate container builds & Kubernetes deploys

### 📘 Content

* GitHub Actions or GitLab CI workflows
* Build → Test → Push → Deploy pipeline
* Kustomize basics
* Hands-on:

  * Create CI/CD pipeline
  * Auto-deploy app to cluster

---

# 🔥 SESSION 7 (1h30) — Observability with Prometheus

### 🎯 Objectives

* Monitor cluster and workloads

### 📘 Content

* Observability fundamentals
* Metrics: Pod, Node, Deployment
* Prometheus Operator
* Scrape configs & exporters
* Hands-on:

  * Install kube-prometheus-stack (Helm)
  * Explore Prometheus UI
  * Create custom metrics endpoint

---

# 🔥 SESSION 8 (1h30) — Grafana Dashboards & Alerting

### 🎯 Objectives

* Visualize metrics & create alerts

### 📘 Content

* Connect Grafana → Prometheus
* Building dashboards
* Alerts & notification channels (Slack, Email, Teams)
* Observability best practices
* Hands-on:

  * Build dashboard for a microservice
  * Add CPU/Memory/Latency panels
  * Create alert rules

---

# 🎉 FINAL PROJECT (Optional)

A mini-project the trainees to completed independently:

👉 Deploy a 3-tier microservice on Kubernetes with:

* Docker images
* Helm chart
* Ingress
* Persistent storage
* Full CI/CD
* Prometheus monitoring
* Grafana dashboard
