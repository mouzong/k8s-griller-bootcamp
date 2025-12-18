# 🧪 Kubernetes Homelab (kinD) — Common Training Resources

This folder is a **shared/common directory inside the main project repository**.

It contains **infrastructure and tooling resources that are common to both English and French trainees**, avoiding duplication while keeping training environments consistent.

This kinD-based Kubernetes homelab is used across:

* 🇬🇧 **English training tracks**
* 🇫🇷 **French training tracks**

---

## 🎯 Purpose of this Folder

The goal of this folder is to provide:

* A **single, standardized Kubernetes local environment**
* A **reproducible setup** for all trainees
* A **macOS-friendly NodePort configuration**
* Shared tooling used by multiple training paths

This folder is **not a standalone repository** and is meant to be referenced by training materials located elsewhere in the project.

---

## 📦 Prerequisites

Before using this homelab, make sure the following tools are installed:

* **Docker** (Docker Desktop recommended)
* **kubectl**
* **kind**
* **make**

Verification:

```bash
docker --version
kubectl version --client
kind version
make --version
```

---

## 📁 Folder Structure

```text
common/
├── Makefile              # kinD cluster automation
├── cluster-config.yaml   # Auto-generated (do not edit manually)
└── README.md             # This document
```

⚠️ `cluster-config.yaml` is **generated automatically** and should not be committed or manually modified.

---

## ⚙️ Configuration (Makefile)

The Makefile exposes the following variables, which apply to **all trainees regardless of language track**:

```makefile
KIND_CLUSTER_NAME ?= kind
NODEPORT_START    ?= 30000
NODEPORT_END      ?= 30010
KIND_CONFIG       ?= cluster-config.yaml
```

### 🔧 Variable Explanation

* **KIND_CLUSTER_NAME**: Name of the kinD cluster
* **NODEPORT_START / NODEPORT_END**: NodePort range exposed to the host
* **KIND_CONFIG**: Generated kinD configuration file

💡 This explicit NodePort mapping is **critical on macOS**, where Docker runs inside a VM.

---

## 🚀 Create the Shared Kubernetes Cluster

From the `common/` folder, run:

```bash
make kind-up
```

This will:

1. Generate `cluster-config.yaml`
2. Map NodePorts `30000–30010` from container → host
3. Recreate the kinD cluster if it already exists

This cluster is then reused by:

* English labs
* French labs

---

## 🔍 Validate the Cluster

```bash
kubectl cluster-info
kubectl get nodes
```

Expected:

* One control-plane node
* Status: `Ready`

---

## 🌐 NodePort Usage (Training Labs)

All labs that expose services via NodePort **must use ports within the configured range**:

```yaml
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 8080
      nodePort: 30001
```

Access from host:

```text
http://localhost:30001
```

✅ Works consistently for all trainees on macOS and Linux.

---

## 🧹 Tear Down the Cluster

When finished with labs:

```bash
make kind-down
```

This will:

* Delete the shared kinD cluster
* Remove generated configuration files

---

## 🧠 Training Philosophy

This setup ensures:

* ✅ One environment for all trainees
* ✅ No divergence between EN / FR tracks
* ✅ Less setup friction, more learning

Happy learning & teaching 🚀
