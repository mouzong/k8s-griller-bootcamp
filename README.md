# 🔥 Kubernetes Griller Bootcamp (12h)

### **Learn. Build. Grill. Become Kubernetes-Ready.**

[![YouTube](https://img.shields.io/badge/YouTube-CodeGrill-red?logo=youtube)](https://www.youtube.com/@codegrill)
[![Follow LinkedIn](https://img.shields.io/badge/Follow-Andreas%20MOUZONG-blue?logo=linkedin)](https://www.linkedin.com/in/andreas-mouzong)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](#)

---

# 📚 Table of Contents

1. [📖 About the Bootcamp](#-about-the-bootcamp)
2. [🎯 Learning Goals](#-learning-goals)
3. [📂 Repository Structure](#-repository-structure)
4. [🔥 Program Sessions](#-program-sessions)
   * [Session 1 — Docker Fundamentals](#-session-1-1h30--docker-fundamentals--containers-mindset)
   * [Session 2 — Kubernetes Core Concepts](#-session-2-1h30--kubernetes-core-concepts)
   * [Session 3 — Networking & Ingress](#-session-3-1h30--kubernetes-networking--ingress)
   * [Session 4 — Storage & Stateful Apps](#-session-4-1h30--storage--stateful-applications)
   * [Session 5 — Helm](#-session-5-1h30--helm--app-packaging)
   * [Session 6 — CICD](#-session-6-1h30---cicd-to-kubernetes)
   * [Session 7 — Prometheus](#-session-7-1h30--observability-with-prometheus)
   * [Session 8 — Grafana](#-session-8-1h30--grafana-dashboards--alerting)
5. [🏆 Final Project](#-final-project-optional)
6. [🤝 Contributing](#-contributing)
7. [📜 License](#-license)
8. [🌍 Follow CodeGrill](#-follow-codegrill)

---

# 📖 About the Bootcamp

The **Kubernetes Griller Bootcamp** is a **12-hour intensive hands-on program** designed to take you from *beginner* to *production-ready* in Kubernetes.

You’ll learn Docker, Kubernetes primitives, Helm, CI/CD, Observability, and deploy production-grade workloads.

Cohorts are available **in English & French**, with exercises and labs synchronized across both.

---

# 🎯 Learning Goals

By the end of the bootcamp, you will:

✔ Build and push Docker images
✔ Deploy workloads on Kubernetes
✔ Configure networking, ingress, storage
✔ Package apps with Helm
✔ Deploy using CI/CD pipelines
✔ Monitor with Prometheus & Grafana
✔ Deliver a full GitOps-ready project

---

# 📂 Repository Structure

```
k8s-griller-bootcamp/
│
├── english/
│   ├── session-1/
│   ├── session-2/
│   ├── session-3/
│   ├── session-4/
│   ├── session-5/
│   ├── session-6/
│   ├── session-7/
│   ├── session-8/
│   └── final-project/
│
├── french/
│   ├── session-1/
│   ├── session-2/
│   ├── session-3/
│   ├── session-4/
│   ├── session-5/
│   ├── session-6/
│   ├── session-7/
│   ├── session-8/
│   └── final-project/
│
└── README.md
```

---

# 🔥 PROGRAM SESSIONS

## 🔥 SESSION 1 (1h30) — Docker Fundamentals & Containers Mindset

### 🎯 Objectives

* Understand containerization
* Build foundational Docker skills for Kubernetes

### 📘 Content

* What is a container? Why Docker
* Images, layers, registries
* Dockerfile: best practices
* Build & push images
* Container networking & volumes
* **Hands-on:**

  * Build a simple app
  * Push to Docker Hub / GHCR
  * Run multi-container apps with Docker Compose

---

## 🔥 SESSION 2 (1h30) — Kubernetes Core Concepts

### 🎯 Objectives

* Understand Kubernetes architecture & components

### 📘 Content

* Cluster architecture (API Server, Scheduler, Kubelet…)
* Pods, ReplicaSets, Deployments, Services
* ConfigMaps & Secrets
* **Hands-on:**

  * Deploy first app
  * Scaling / Rollouts / Rollbacks

---

## 🔥 SESSION 3 (1h30) — Kubernetes Networking & Ingress

### 🎯 Objectives

* Understand service exposure
* Learn traffic flow inside clusters

### 📘 Content

* ClusterIP, NodePort, LoadBalancer
* Ingress Controllers (NGINX)
* DNS inside Kubernetes
* **Hands-on:**

  * Deploy an Ingress
  * Multi-path routing (2 services)

---

## 🔥 SESSION 4 (1h30) — Storage & Stateful Applications

### 🎯 Objectives

* Manage persistent data in Kubernetes

### 📘 Content

* PersistentVolumes (PV)
* PersistentVolumeClaims (PVC)
* StorageClasses
* StatefulSets
* **Hands-on:**

  * Deploy MySQL/Postgres
  * Bind PV/PVC
  * Backup strategy

---

## 🔥 SESSION 5 (1h30) — Helm & App Packaging

### 🎯 Objectives

* Learn how to package & reuse deployments

### 📘 Content

* Why Helm?
* Charts, templates, values
* Releasing & versioning
* **Hands-on:**

  * Create your own chart
  * Deploy with Helm
  * Use community charts

---

## 🔥 SESSION 6 (1h30) — CI/CD to Kubernetes

### 🎯 Objectives

* Automate build → test → deploy

### 📘 Content

* GitHub Actions / GitLab CI pipelines
* Build → Test → Push → Deploy
* Kustomize basics
* **Hands-on:**

  * Create CI/CD pipeline
  * Auto-deploy app to cluster

---

## 🔥 SESSION 7 (1h30) — Observability with Prometheus

### 🎯 Objectives

* Monitor clusters and workloads

### 📘 Content

* Observability fundamentals
* Metrics (Pod, Node, Deployment)
* Prometheus Operator
* Exporters & scrape configs
* **Hands-on:**

  * Install kube-prometheus-stack
  * Explore Prometheus UI
  * Create custom metrics endpoint

---

## 🔥 SESSION 8 (1h30) — Grafana Dashboards & Alerting

### 🎯 Objectives

* Visualize metrics & create alerts

### 📘 Content

* Connect Grafana → Prometheus
* Build dashboards
* Alerting rules
* **Hands-on:**

  * Build dashboard for a microservice
  * CPU, Memory & Latency panels
  * Set up alerts

---

# 🏆 FINAL PROJECT (Optional)

Deploy a full **3-tier microservice** on Kubernetes:

✔ Docker images
✔ Helm chart
✔ Ingress
✔ Persistent storage
✔ CI/CD pipeline
✔ Prometheus monitoring
✔ Grafana dashboards

You will present your architecture and deployment strategy at the end.

---

# 🤝 Contributing

Pull requests are welcome!
You can contribute translations, labs, improvements, or examples.

---

# 📜 License

MIT License — feel free to use this material in your own learning or teaching.

---

# 🌍 Follow CodeGrill

- 🔥 YouTube: [https://www.youtube.com/@codegrill](https://www.youtube.com/@codegrill)
- 🔥 LinkedIn: [https://www.linkedin.com/in/bertrand-guegaba/](https://www.linkedin.com/in/andreas-mouzong/)
- 🔥 GitHub: [https://github.com/codegrill](https://github.com/mouzong)
