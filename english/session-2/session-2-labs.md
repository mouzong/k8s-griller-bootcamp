# 🚀 LABS — SESSION 2 (Architecture, Pods, Deployments, Services…)

## **🧪 LAB 1 — Inspect Your Cluster Architecture**

🎯 Objective: Discover the components of the Control Plane & Worker Nodes

### Steps:

1. List the nodes:

```bash
kubectl get nodes -o wide
```

2. View the Control Plane components:

```bash
kubectl get pods -n kube-system
```

3. Inspect a system Pod:

```bash
kubectl describe pod <pod-name> -n kube-system
```

4. Enter a node (Kind):

```bash
docker exec -it <cluster-name>-control-plane bash
```

👉 **Expected result:** Understand how Kubernetes organizes its internal components.

---

## **🧪 LAB 2 — Manually Create and Analyze a Pod**

🎯 Objective: Understand the structure of a Pod and its ephemeral nature

### 1. Create a Pod

Create a file `pod.yaml`:

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

### 2. Observe the Pod

```bash
kubectl get pod mypod -o wide
kubectl describe pod mypod
```

### 3. Delete and observe its disappearance

```bash
kubectl delete pod mypod
kubectl get pods
```

👉 **Result:** You see that the Pod is deleted without being recreated → ephemeral.

---

## **🧪 LAB 3 — Create a Deployment + ReplicaSet**

🎯 Objective: Understand self-healing and automatic scaling of ReplicaSets.

### 1. Deploy Nginx

```bash
kubectl create deployment web --image=nginx
```

### 2. Check ReplicaSet & Pods

```bash
kubectl get deployments
kubectl get rs
kubectl get pods
```

### 3. Manually delete a Pod

```bash
kubectl delete pod <pod-name>
```

Observe:

```bash
kubectl get pods
```

👉 **Result:** Kubernetes automatically recreates the Pod (self-healing).

---

## **🧪 LAB 4 — Horizontal Scaling**

🎯 Objective: Dynamically change the number of replicas.

### Scale up:

```bash
kubectl scale deployment web --replicas=5
kubectl get pods
```

### Scale down:

```bash
kubectl scale deployment web --replicas=2
kubectl get pods
```

👉 **Result:** The ReplicaSet instantly adjusts the number of Pods.

---

## **🧪 LAB 5 — Rolling Update + Rollback**

🎯 Objective: Master zero-downtime updates.

### 1. Update the image

```bash
kubectl set image deployment/web nginx=nginx:1.25
```

### 2. Check the rollout

```bash
kubectl rollout status deployment/web
```

### 3. Perform a rollback

```bash
kubectl rollout undo deployment/web
```

👉 **Result:** Understand progressive (rolling) updates.

---

## **🧪 LAB 6 — Create a Service (NodePort)**

🎯 Objective: Expose an application and understand internal service networking.

### 1. Expose:

```bash
kubectl expose deployment web --type=NodePort --port=80
kubectl get svc web
```

### 2. Test access (with Kind)

Retrieve the port:

```bash
kubectl get svc web
```

Then use your host IP or the Kind node IP:

```bash
curl http://localhost:<NODE_PORT>
```

👉 **Result:** Understand network stability provided by Services.

---

## **🧪 LAB 7 — ConfigMaps & Injection into Pods**

🎯 Objective: Separate code from configuration.

### 1. Create a ConfigMap

```bash
kubectl create configmap app-config --from-literal=color=blue
```

### 2. Mount it into a Pod

Create `configmap-pod.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: cm-pod
spec:
  containers:
  - name: test
    image: busybox
    command: ["sh", "-c", "echo The color is $COLOR && sleep 3600"]
    env:
    - name: COLOR
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: color
```

Apply:

```bash
kubectl apply -f configmap-pod.yaml
kubectl logs cm-pod
```

👉 **Result:** Understand the practical use of ConfigMaps.

---

## **🧪 LAB 8 — Secrets & Environment Variables**

🎯 Objective: Handle sensitive information.

### 1. Create a Secret

```bash
kubectl create secret generic db-secret \
  --from-literal=password=MySecretPass123
```

### 2. Verify

```bash
kubectl get secrets
kubectl describe secret db-secret
```

### 3. Inject it into a Pod

```yaml
env:
- name: DB_PASSWORD
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: password
```

👉 **Result:** Understand secure usage of Secrets.

---

# 🎁 BONUS — LAB CHALLENGE (Mini-Project)

🎯 Objective: Combine **Pods + Deployments + Services + ConfigMaps + Secrets**

Create a simple stack:

* An Nginx backend deployed as a Deployment
* A ConfigMap for Nginx configuration
* A Secret for an admin password
* A NodePort Service
* A rolling update to a new version
* Scaling to 5 replicas

This mini-project consolidates the entire session.
