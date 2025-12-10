# 🔥 SESSION 2 — Concepts Fondamentaux de Kubernetes

- ⏱️ **Durée :** 1h30
- 🎯 **Objectif :** Comprendre l’architecture centrale et les composants de Kubernetes, puis déployer votre première application.

---

# 1. 🔍 Introduction à l’Architecture de Kubernetes

## 1.1 Qu’est-ce que Kubernetes ?

Kubernetes (K8s) est une plateforme open source d’orchestration de conteneurs qui automatise :

* Le déploiement des applications
* Le passage à l’échelle (scaling)
* Le load balancing
* Les mises à jour sans interruption
* L’auto-réparation (self-healing)

---

# 2. 🏗️ Architecture d’un Cluster Kubernetes

Un cluster Kubernetes possède **deux couches principales** :

## 2.1 Control Plane (le cerveau du cluster)

Le Control Plane garantit que le cluster fonctionne comme prévu.

### **API Server**

* Point central de communication de Kubernetes
* Expose l’API K8s utilisée par `kubectl`, le dashboard et les contrôleurs
* Valide les manifestes YAML et les stocke dans etcd

### **Scheduler**

* Assigne les Pods aux nœuds
* Décide **où** un pod doit s’exécuter en fonction des ressources et des contraintes

### **Controller Manager**

Exécute en arrière-plan les contrôleurs qui garantissent l’état désiré :

* Node Controller
* Deployment Controller
* ReplicaSet Controller
* Job Controller
* Etc.

### **etcd**

* Base de données distribuée clé-valeur stockant **tout l’état du cluster**
* Critique pour le cluster ; nécessite des sauvegardes régulières

---

## 2.2 Worker Nodes (les muscles du cluster)

Les nœuds exécutent réellement vos applications.

### **Kubelet**

* Agent présent sur chaque nœud
* S’assure que les conteneurs tournent comme demandé par l’API Server

### **Container Runtime**

Exemples : Docker, containerd, CRI-O

* Responsable de l’exécution des conteneurs

### **Kube-Proxy**

* Gère les règles réseau
* Assure la communication entre pods et services
* Implémente le load balancing des services

---

# 3. 🧱 Objets Fondamentaux de Kubernetes

## 3.1 Pods

* La **plus petite unité déployable** dans Kubernetes
* Un Pod contient **un ou plusieurs conteneurs étroitement couplés**
* Les conteneurs d’un Pod partagent :

  * L’espace réseau
  * Les volumes de stockage
  * Le cycle de vie

**Les Pods sont éphémères** — K8s peut les tuer et les recréer.

---

## 3.2 ReplicaSets

* Garantissent un nombre souhaité de **pods identiques**
* Si un pod meurt → le ReplicaSet le recrée automatiquement
* Rarement utilisés seuls ; généralement gérés par les Deployments

---

## 3.3 Deployments

Le composant principal pour déployer des applications.

Fonctionnalités :

* Rolling updates
* Rollbacks
* Scaling
* Contrôle de stratégie
* Modèle déclaratif

---

## 3.4 Services

Les Pods ont des IP éphémères → les Services fournissent un **accès réseau stable**.

Types :

* **ClusterIP** (par défaut) : accès interne
* **NodePort** : expose l’app sur chaque nœud
* **LoadBalancer** : load balancer cloud (AWS, GCP, Azure)
* **Headless** : service sans cluster IP (apps stateful)

---

## 3.5 ConfigMaps & Secrets

### **ConfigMaps**

* Stockent des données de configuration **non sensibles**
* Injectables dans les Pods en :

  * Variables d’environnement
  * Fichiers via volume

### **Secrets**

* Stockent des données **sensibles** :

  * Mots de passe
  * Tokens
  * Certificats

* Encodés en Base64 (non chiffrés sauf utilisation KMS/Vault)

---

# 4. 🧪 Exercices Pratiques

## 4.1 Déployer votre première application

### **Exemple de Deployment YAML**

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

### Appliquer le manifeste :

```bash
kubectl apply -f nginx-deployment.yaml
```

### Vérifier :

```bash
kubectl get pods
kubectl get rs
kubectl get deployments
```

---

## 4.2 Exposer l’application via un Service

```bash
kubectl expose deployment nginx-deployment \
  --port=80 \
  --type=NodePort
```

Voir le port exposé :

```bash
kubectl get svc
```

---

## 4.3 Faire du Scaling

Augmenter le nombre de réplicas :

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

---

## 4.4 Rolling Update

Mettre à jour l’image :

```bash
kubectl set image deployment/nginx-deployment \
  nginx=nginx:1.25
```

Vérifier le déploiement :

```bash
kubectl rollout status deployment/nginx-deployment
```

Rollback si nécessaire :

```bash
kubectl rollout undo deployment/nginx-deployment
```

---

# 5. 🧠 Points Clés à Retenir

* **Control Plane** = couche décisionnelle
* **Worker Nodes** = couche d’exécution
* **Pods** éphémères → gérés par ReplicaSets
* **Deployments** gèrent cycle de vie, scaling et mises à jour
* **Services** = réseau stable malgré la nature volatile des pods
* **ConfigMaps** & **Secrets** séparent configuration et code
* Kubernetes garantit **état désiré, auto-réparation, mises à jour sans interruption**
