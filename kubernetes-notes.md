# ☸️ Kubernetes — Architecture & Core Concepts

> My personal study notes — including interview-friendly one-liners.

---

## 🏗️ Architecture

Kubernetes uses a **client–server architecture** consisting of:

- **Master node (Control Plane)** — manages the cluster
- **Worker nodes** — run your actual applications

The master is typically installed on one Linux system; worker nodes can scale across many.

### Master node components
- **API Server** — entry point for the cluster
- **Scheduler** — places pods on nodes
- **Controller Manager** — keeps state consistent
- **etcd** — distributed key-value database for cluster state

### Worker node components
- **kubelet** — talks to the master
- **kube-proxy** — handles pod networking
- **Container Runtime** (Docker / containerd / CRI-O) — actually runs containers

---

## 🧩 Components in Detail

Kubernetes components fall into two categories:

| Category | Role |
|---|---|
| **Control Plane** | Controls every worker node and every pod inside them |
| **Nodes (Workers)** | Run the containers where your application lives |

---

## 1️⃣ Master Node (Control Plane)

Responsible for **managing the cluster**, **not** running user applications.

### 🔹 API Server
- The main entry point for all Kubernetes commands (`kubectl`)
- All communication happens through the API Server
> 💬 *Interview line:* "API Server is the front door of Kubernetes."

### 🔹 Scheduler
- Decides which node will run which pod
- Considers CPU, RAM, taints, tolerations, affinities
> 💬 *Interview line:* "Scheduler places pods on the best-suited worker node."

### 🔹 Controller Manager
- Ensures **desired state = actual state**
- Includes many controllers: Node, Deployment, ReplicaSet, Endpoint
- Example: if you want 3 pods but 1 dies → Controller Manager starts a new one

### 🔹 etcd
- A distributed key-value store
- Stores the **entire cluster state**
- Highly available, strongly consistent
> 💬 *Interview line:* "etcd is the source of truth of Kubernetes."

---

## 2️⃣ Worker Node

The machine where Kubernetes actually runs your containers.
Contains **kubelet**, **kube-proxy**, and a **container runtime**, and executes workloads assigned by the master.

This is where your applications run **inside Pods**.

### 🔹 Kubelet
- A small agent that runs on every worker node
- Communicates with the Control Plane
- Makes sure containers inside Pods are running
- If a container crashes → kubelet restarts it

### 🔹 Kube-Proxy
- Handles networking on the node
- Assigns IPs to each pod (dynamic IP)
- Implements service load-balancing and routing
- Ensures Pods can communicate with each other and the outside world

### 🔹 Container Runtime
- Actually runs the containers
- Examples: **Docker**, **containerd**, **CRI-O**
- Pulls images, starts/stops containers

---

## 📦 Pod

The smallest unit in Kubernetes.

- A Pod is a group of **one or more containers** deployed together on the same host
- A **cluster** is a group of nodes
- A cluster has at least one worker node and one master node
- A Pod runs on a node, which is controlled by the master

### 🚀 Multi-Container Pod

A Pod that contains more than one container running together inside the same Pod.

➡️ All containers inside the Pod share:
- Same **Network** (IP, port space)
- Same **Storage** (volumes)
- Same **Lifecycle**
- Same **Node**

Used when containers must work together closely.

---

## ⚙️ Tools

### ✅ `kubectl` — Kubernetes Command Line Tool
Used to interact with your Kubernetes cluster.

With kubectl you can:
- Deploy applications
- Check pods, deployments, services
- Scale your app
- Delete resources
- View logs

Example commands:

```bash
kubectl get pods
kubectl apply -f deployment.yaml
kubectl describe pod my-pod
kubectl logs my-pod
```

> ⚠️ `kubectl` does **not** create a cluster — it only controls an existing one.

---

### ✅ Minikube — Local Cluster for Learning

- Runs a small Kubernetes cluster on your laptop / PC
- Good for developers, practice, learning
- **Not** used in production

```bash
minikube start
minikube status
minikube dashboard
```

---

### ✅ kind — Kubernetes in Docker

- Creates a Kubernetes cluster **inside Docker containers**
- Very lightweight, faster than Minikube
- Used for: local testing, CI/CD pipelines, learning Kubernetes

```bash
kind create cluster
kind get clusters
kind delete cluster
```

---

📌 *See [README.md](./README.md) for the overview and roadmap.*
