# 🔥 SESSION 3 (1h30) — Réseau Kubernetes & Ingress

⏱ **Durée :** 1h30
🎯 **Objectif :** Aider les apprenants à comprendre comment les applications communiquent **à l’intérieur** et **à l’extérieur** d’un cluster Kubernetes, et comment le trafic est exposé à l’aide des **Services** et des **Ingress**.

---

## 🎯 Objectifs pédagogiques

À la fin de cette session, les apprenants seront capables de :

* Comprendre comment fonctionne le **réseau au sein d’un cluster Kubernetes**
* Expliquer et utiliser les différents **types de Services**
* Comprendre la **résolution DNS** dans Kubernetes
* Exposer des applications à l’aide d’un **Ingress Controller**
* Configurer un **routage multi-chemins** vers plusieurs services

---

## 📘 Partie 1 — Bases du réseau Kubernetes (15 min)

### 🔹 Concepts clés

* Chaque **Pod possède sa propre adresse IP**
* Les Pods peuvent communiquer **directement entre eux**
* Les IP des Pods sont **éphémères** (elles changent lorsque les Pods redémarrent)
* Les **Services** fournissent une **identité réseau stable**

👉 C’est pour cette raison que les Services sont essentiels dans Kubernetes.

---

## 📘 Partie 2 — Les Services Kubernetes (30 min)

### 🔹 Qu’est-ce qu’un Service ?

Un **Service** expose un ensemble de Pods à l’aide de :

* Une **adresse IP stable**
* Un **nom DNS**
* Un **équilibrage de charge** entre les Pods

---

### 🔹 Les types de Services expliqués

#### 1️⃣ ClusterIP (par défaut)

* Expose l’application **uniquement à l’intérieur du cluster**
* Utilisé pour la **communication interne** (backend → base de données, API → API)

```bash
kubectl expose deployment my-app \
  --type=ClusterIP \
  --port=80
```

📌 **Cas d’usage :**

* Une API backend qui appelle un autre service interne

---

#### 2️⃣ NodePort

* Expose l’application sur **l’IP de chaque nœud**
* Accessible via :
  `NodeIP:NodePort`

```bash
kubectl expose deployment my-app \
  --type=NodePort \
  --port=80
```

📌 **Cas d’usage :**

* Développement local
* Tests rapides sans Ingress

⚠️ **Limitations :**

* Plage de ports : `30000–32767`
* Non recommandé pour la production

---

#### 3️⃣ LoadBalancer

* Expose l’application à l’aide d’un **load balancer du fournisseur cloud**
* Attribue automatiquement une IP externe

```bash
kubectl expose deployment my-app \
  --type=LoadBalancer \
  --port=80
```

📌 **Cas d’usage :**

* Environnements de production sur AWS / GCP / Azure

---

## 📘 Partie 3 — Le DNS dans Kubernetes (10 min)

### 🔹 Fonctionnement du DNS

Kubernetes utilise **CoreDNS** pour résoudre les noms de services.

Format DNS d’un service :

```text
service-name.namespace.svc.cluster.local
```

Exemple :

```text
backend.default.svc.cluster.local
```

✔ Les Pods peuvent appeler les services en utilisant **uniquement le nom du service** :

```bash
curl http://backend
```

📌 **Avantages :**

* Pas d’IP codées en dur
* Découverte automatique des services

---

## 📘 Partie 4 — Ingress & Ingress Controllers (20 min)

### 🔹 Qu’est-ce qu’un Ingress ?

Un **Ingress** gère le trafic **HTTP/HTTPS** provenant de l’extérieur du cluster vers les services internes.

Au lieu d’exposer plusieurs services individuellement, on expose **un point d’entrée unique**.

---

### 🔹 Ingress Controller

Les règles Ingress nécessitent un contrôleur pour fonctionner.

Contrôleurs courants :

* **NGINX Ingress Controller**
* **Gateway API** (approche de nouvelle génération)

---

### 🔹 Flux Ingress

```text
Client → Ingress Controller → Service → Pod
```

---

## 📘 Partie 5 — Travaux pratiques (15 min)

### 🧪 TP 1 — Déployer un Ingress Controller (NGINX)

```bash
kubectl apply -f \
https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

Vérification :

```bash
kubectl get pods -n ingress-nginx
```

---

### 🧪 TP 2 — Déployer deux applications

Exemples de services :

* `app1-service`
* `app2-service`

Chaque service expose le port 80.

---

### 🧪 TP 3 — Routage multi-chemins avec Ingress

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

Application :

```bash
kubectl apply -f ingress.yaml
```

Test :

```bash
curl http://demo.local/app1
curl http://demo.local/app2
```

📌 *(Ajouter `demo.local` dans `/etc/hosts` si nécessaire)*

---

## ✅ Points clés à retenir

* Les **Services** fournissent un réseau stable dans Kubernetes
* **ClusterIP** → communication interne
* **NodePort** → exposition externe simple
* **LoadBalancer** → exposition native cloud
* **Ingress** centralise le routage HTTP
* Le DNS facilite une communication simple et scalable entre services
