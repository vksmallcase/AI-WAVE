# Online Boutique (Microservices Demo) – Installation Guide

A quick guide for installing and running the **Google Online Boutique** microservices demo locally on Kubernetes.

---

## Prerequisites

- A local Kubernetes cluster (Minikube, Kind, or Docker Desktop)
- [`kubectl`](https://kubernetes.io/docs/tasks/tools/) CLI installed
- [`git`](https://git-scm.com/downloads) installed
- Recommended: At least **4 CPUs** and **8 GB RAM**

---

## Installation Steps

### 1. Start a Local Kubernetes Cluster

<details>
<summary><strong>Using Minikube</strong></summary>

```bash
minikube start --cpus=4 --memory=8192
```
</details>

<details>
<summary><strong>Using Kind</strong></summary>

```bash
cat > kind-config.yaml <<'EOF'
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
EOF

kind create cluster --name boutique --config kind-config.yaml
```
</details>

---

### 2. Clone the Demo Repository

```bash
git clone https://github.com/GoogleCloudPlatform/microservices-demo.git
cd microservices-demo
```

---

### 3. Deploy the Application

```bash
kubectl apply -f ./release/kubernetes-manifests.yaml
```

Wait for all pods to be running:

```bash
kubectl get pods -w
```

---

### 4. Access the Frontend

Forward the frontend service:

```bash
kubectl port-forward svc/frontend 8080:80
```

Open your browser and navigate to:

```
http://localhost:8080
```

---

## Cleanup

To remove the app:

```bash
kubectl delete -f ./release/kubernetes-manifests.yaml
```

To delete the cluster:

```bash
kind delete cluster --name boutique
# or
minikube delete
```

---
