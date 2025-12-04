# 🧑‍🍳🔥 Session 1 — Fondamentaux de Docker & Concept de Conteneurs

**Kubernetes Griller Bootcamp — Notes Étudiant (1h30)**

---

# 📌 1. Qu’est-ce qu’un conteneur ?

Les conteneurs sont des **environnements légers et isolés** qui exécutent des applications avec toutes leurs dépendances incluses.

Avant les conteneurs, déployer une application sur différents serveurs entraînait souvent :

* Des problèmes “Ça marche sur ma machine”
* Des conflits de dépendances
* Un provisionnement lent des environnements
* Des machines virtuelles lourdes consommant trop de ressources

### ✔ Les conteneurs résolvent tout cela.

---

## 🆚 Machines Virtuelles vs Conteneurs

| Machines Virtuelles             | Conteneurs                   |
| ------------------------------- | ---------------------------- |
| Lourdes (en Go)                 | Légers (en Mo)               |
| Démarrage en minutes            | Démarrage en millisecondes   |
| Chaque VM possède un OS complet | Partagent le noyau de l’hôte |
| Déploiement lent                | Très rapide                  |
| Peu portables                   | Portables partout            |

### 👉 Kubernetes n’exécute **PAS** de machines virtuelles.

Il orchestre des **conteneurs**, donc maîtriser Docker est la base de Kubernetes.

---

# 📌 2. Pourquoi Docker ?

Docker est devenu le standard de l’industrie car il apporte :

### ✔ **Portabilité**

La même image fonctionne partout (PC, serveur, cloud, cluster).

### ✔ **Cohérence**

Application + dépendances packagées ensemble.

### ✔ **Rapidité**

Démarrage et déploiement en quelques secondes.

### ✔ **Scalabilité**

Parfait pour les microservices et les architectures cloud-native.

### ✔ **Productivité développeur**

Simple à construire, tester et partager.

---

# 📌 3. Architecture Docker (Vue générale)

```
+-----------------------+
|      Docker CLI       |   → Vous lancez : docker build / run / push
+-----------------------+
             |
             v
+-----------------------+
|   Docker Daemon       |   → Construit les images, exécute les conteneurs
+-----------------------+
             |
             v
+-----------------------+
|     Docker Registry   |   → Stocke et distribue les images
+-----------------------+
```

**Composants principaux :**

* **Docker Client (CLI) :** là où vous tapez les commandes
* **Docker Daemon :** exécute les actions
* **Docker Images :** modèles en lecture seule
* **Docker Containers :** instances actives des images
* **Registries :** Docker Hub, GHCR, ECR, GCR, etc.

---

# 📌 4. Images, Couches & Registries

## 🏗️ Une image Docker = Un blueprint en couches

Chaque instruction d’un Dockerfile crée une **couche immuable**.

Exemple :

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json .
RUN npm install --production
COPY . .
CMD ["node", "app.js"]
```

Couches (du haut vers le bas) :

1. Image de base (node:18-alpine)
2. Création du dossier
3. Copie des fichiers package.json
4. Installation des dépendances
5. Copie du code applicatif
6. Commande de démarrage

### Pourquoi les couches sont importantes ?

* Builds plus rapides
* Push/pull d’images optimisés
* Cache améliorant les temps de compilation

---

# 📌 5. Rédiger de bons Dockerfiles

Pour créer des conteneurs prêts pour la production :

### ✔ Utiliser des images de base légères

```
FROM python:3.10-slim
FROM node:18-alpine
FROM golang:1.22-alpine
```

### ✔ Réduire le nombre de couches

Fusionner les instructions RUN si possible.

### ✔ Copier uniquement ce dont vous avez besoin

```
COPY package*.json .
```

### ✔ Éviter de tourner en root

```
USER node
```

### ✔ Utiliser des builds multi-étapes (optionnel, avancé)

* Étape build
* Étape runtime

---

# 📌 6. Construire & Pousser une Image Docker

## 👉 Construire une image

```bash
docker build -t codegrill/app:1.0 .
```

## 👉 Exécuter le conteneur

```bash
docker run -p 8080:8080 codegrill/app:1.0
```

## 👉 Tagger l'image pour Docker Hub

```bash
docker tag codegrill/app:1.0 <username>/app:1.0
```

## 👉 Pousser l'image

```bash
docker push <username>/app:1.0
```

---

# 📌 7. Bases du Réseau Docker

Docker crée des réseaux pour permettre aux conteneurs de communiquer.

### Réseaux par défaut :

* `bridge`
* `host`
* `none`

### Créer votre propre réseau :

```bash
docker network create app-net
```

Exécuter dedans :

```bash
docker run -d --network app-net --name api codegrill/api
docker run -d --network app-net --name db mysql
```

Communication via noms DNS :

```
api → http://db
db → http://api
```

---

# 📌 8. Volumes Docker (Persistance)

Les conteneurs sont temporaires : en cas de redémarrage, les données disparaissent.

Les volumes règlent ce problème.

### Créer un volume :

```bash
docker volume create app-data
```

### Attacher un volume :

```bash
docker run -d \
  -v app-data:/var/lib/mysql \
  mysql
```

---

# 📌 9. Labs Pratiques

---

## 🧪 LAB 1 — Construire une petite app Node.js

Créer un dossier :

```bash
mkdir docker-lab
cd docker-lab
```

### Créer `app.js` :

```js
const express = require('express');
const app = express();

app.get("/", (req, res) => res.send("🔥 CodeGrill Docker Lab!"));
app.listen(3000, () => console.log("Server running on port 3000"));
```

### Créer `package.json` :

```json
{
  "name": "docker-lab",
  "dependencies": {
    "express": "4.18.2"
  }
}
```

---

## 🧪 LAB 2 — Créer le Dockerfile

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

## 🧪 LAB 3 — Builder l’image

```bash
docker build -t codegrill/docker-lab:1.0 .
```

Exécuter :

```bash
docker run -p 3000:3000 codegrill/docker-lab:1.0
```

Ouvrir :

```
http://localhost:3000
```

---

## 🧪 LAB 4 — Pousser l’image sur Docker Hub

Connexion :

```bash
docker login
```

Tag :

```bash
docker tag codegrill/docker-lab:1.0 <username>/docker-lab:1.0
```

Push :

```bash
docker push <username>/docker-lab:1.0
```

---

## 🧪 LAB 5 — Docker Compose (App multi-conteneurs)

Créer `docker-compose.yml` :

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

Lancer :

```bash
docker compose up -d
```

Lister :

```bash
docker ps
```

Arrêter :

```bash
docker compose down
```

---

# 📌 10. Résumé de la Session

À la fin de cette session, vous devez comprendre :

✔ Ce que sont les conteneurs et leur utilité
✔ L’architecture Docker
✔ Le fonctionnement des images et des couches
✔ Comment écrire un Dockerfile correct
✔ Comment construire, exécuter, tagger et pousser une image
✔ Comment fonctionne le réseau Docker
✔ Comment persister les données avec les volumes
✔ Comment exécuter une application multi-conteneurs

Vous êtes maintenant prêt pour **Session 2 — Concepts de Base Kubernetes**.
