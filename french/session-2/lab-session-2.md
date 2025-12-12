# 🚀 LABS — SESSION 2 (Architecture, Pods, Deployments, Services…)

## **🧪 LAB 1 — Inspecter l’Architecture de votre Cluster**

🎯 Objectif : Découvrir les composants du Control Plane & Worker Nodes

### Étapes :

1. Lister les nœuds :

```bash
kubectl get nodes -o wide
```

2. Voir les composants du Control Plane :

```bash
kubectl get pods -n kube-system
```

3. Inspecter un Pod système :

```bash
kubectl describe pod <nom-du-pod> -n kube-system
```

4. Entrer dans un node (Kind) :

```bash
docker exec -it <nom-du-cluster>-control-plane bash
```

👉 **Résultat attendu :** Comprendre comment Kubernetes organise ses composants internes.

---

## **🧪 LAB 2 — Créer et analyser un Pod manuellement**

🎯 Objectif : Comprendre la structure d’un Pod et son caractère éphémère

### 1. Créer un Pod

Crée un fichier `pod.yaml` :

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mycontainer
    image: nginx
    ports:
    - containerPort: 80
```

```bash
kubectl apply -f pod.yaml
```

### 2. Observer le Pod

```bash
kubectl get pod mypod -o wide
kubectl describe pod mypod
```

### 3. Supprimer et observer la disparition

```bash
kubectl delete pod mypod
kubectl get pods
```

👉 **Résultat :** Tu vois que le Pod est supprimé sans recréation → éphémère.

---

## **🧪 LAB 3 — Créer un Deployment + ReplicaSet**

🎯 Objectif : Comprendre l’auto-réparation et le scaling automatique des RS.

### 1. Déployer Nginx

```bash
kubectl create deployment web --image=nginx
```

### 2. Vérifier ReplicaSet & Pods

```bash
kubectl get deployments
kubectl get rs
kubectl get pods
```

### 3. Supprimer un Pod manuellement

```bash
kubectl delete pod <nom-du-pod>
```

Observez :

```bash
kubectl get pods
```

👉 Résultat : Kubernetes recrée automatiquement le Pod (self-healing).

---

## **🧪 LAB 4 — Scaling horizontal**

🎯 Objectif : Modifier dynamiquement le nombre de replicas.

### Augmenter :

```bash
kubectl scale deployment web --replicas=5
kubectl get pods
```

### Réduire :

```bash
kubectl scale deployment web --replicas=2
kubectl get pods
```

👉 Résultat : Le ReplicaSet adapte instantanément le nombre de Pods.

---

## **🧪 LAB 5 — Rolling Update + Rollback**

🎯 Objectif : Maitriser les mises à jour sans interruption.

### 1. Mettre à jour l’image

```bash
kubectl set image deployment/web nginx=nginx:1.25
```

### 2. Vérifier le rollout

```bash
kubectl rollout status deployment/web
```

### 3. Faire un rollback

```bash
kubectl rollout undo deployment/web
```

👉 Résultat : Compréhension des mises à jour progressives.

---

## **🧪 LAB 6 — Créer un Service (NodePort)**

🎯 Objectif : Exposer une application et comprendre le Service Mesh interne.

### 1. Exposer :

```bash
kubectl expose deployment web --type=NodePort --port=80
kubectl get svc web
```

### 2. Tester l’accès (avec Kind)

Récupérer le port :

```bash
kubectl get svc web
```

Puis utiliser l’IP de ton host ou du node Kind :

```bash
curl http://localhost:<NODE_PORT>
```

👉 Résultat : Comprendre la stabilité réseau grâce aux Services.

---

## **🧪 LAB 7 — ConfigMaps & Injection dans Pods**

🎯 Objectif : Séparer le code de la configuration.

### 1. Créer un ConfigMap

```bash
kubectl create configmap app-config --from-literal=color=blue
```

### 2. Le monter dans un Pod

Créer `configmap-pod.yaml` :

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: cm-pod
spec:
  containers:
  - name: test
    image: busybox
    command: ["sh", "-c", "echo La couleur est $COLOR && sleep 3600"]
    env:
    - name: COLOR
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: color
```

Appliquer :

```bash
kubectl apply -f configmap-pod.yaml
kubectl logs cm-pod
```

👉 Résultat : Comprendre le fonctionnement pratique de ConfigMap.

---

## **🧪 LAB 8 — Secrets & Variables d’environnement**

🎯 Objectif : Manipuler des informations sensibles.

### 1. Créer un Secret

```bash
kubectl create secret generic db-secret \
  --from-literal=password=MySecretPass123
```

### 2. Vérifier

```bash
kubectl get secrets
kubectl describe secret db-secret
```

### 3. L’injecter dans un Pod

```yaml
env:
- name: DB_PASSWORD
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: password
```

👉 Résultat : Comprendre l’usage sécurisé des Secrets.

---

# 🎁 BONUS — LAB CHALLENGE (Mini-Projet)

🎯 Objectif : Combiner **Pods + Deployments + Services + ConfigMaps + Secrets**

Crée une stack simple :

* Un backend Nginx déployé en Deployment
* ConfigMap pour la configuration Nginx
* Secret pour un mot de passe admin
* Service NodePort
* Rolling update d’une nouvelle version
* Scaling à 5 replicas

Ce petit projet consolide toute la session.

Tu veux quoi ?
