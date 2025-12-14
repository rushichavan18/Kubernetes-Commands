# Kubernetes-Commands

Below is a **clean, structured README.md** created from your command history.
I’ve **grouped commands by topic**, removed noise/typos, and explained **why each set of commands is used**, so when you read it later you’ll **understand your journey clearly**.

You can **copy–paste this directly into GitHub** ✅

---

# 📘 Kubernetes, Docker & DevOps Practice – Command Reference

This document explains the commands used during hands-on practice with **Linux, Docker, Kubernetes (Kind), HPA, Storage, and Helm**.
Each section explains **what was done and why**.

---

## 🖥️ 1. Linux & Shell Configuration

### Clear terminal & basic navigation

```bash
clear
ls
ll
pwd
cd ..
```

**Purpose:**
Used to navigate directories and keep the terminal clean.

---

### Edit shell configuration

```bash
vim ~/.bashrc
vim ~/.bash_profile
source ~/.bashrc
reset
```

**Purpose:**

* Customize terminal prompt (PS1)
* Increase history size
* Apply shell changes without logout

---

### Custom prompt (PS1)

```bash
PS1='\s-\v\$ '
```

**Purpose:**
Customize how the shell prompt looks (shell name, version, `$`).

---

## 🧠 2. Command History Management

### View & save history

```bash
history
history > history.txt
touch history.txt
```

**Purpose:**

* View previously executed commands
* Save history into a file for learning, auditing, or documentation

---

## 🐳 3. Docker Installation & Usage

### Install Docker

```bash
sudo apt-get update
sudo apt-get install docker.io
```

### Allow non-root Docker usage

```bash
sudo usermod -aG docker $USER
newgrp docker
```

**Purpose:**
Run Docker commands without `sudo`.

---

### Docker basics

```bash
docker ps
docker ps -a
docker images
docker build -t online-shop-app .
docker run -d -p 5173:5173 online-shop-app
docker exec -it <container-id> bash
docker stop <container-id>
```

**Purpose:**

* Build images
* Run containers
* Debug inside containers

---

## ☸️ 4. Kubernetes Tooling (kubectl & kind)

### Install Kind

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.30.0/kind-linux-amd64
chmod +x kind
sudo mv kind /usr/local/bin/
kind --version
```

---

### Install kubectl

```bash
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version
```

---

### Create Kubernetes cluster using Kind

```bash
kind create cluster --config cluster.yml --name tws-cluster
kubectl get nodes
kubectl cluster-info
```

**Purpose:**
Create a local Kubernetes cluster using Docker.

---

## 📦 5. Kubernetes Core Operations

### Namespaces

```bash
kubectl get ns
kubectl apply -f namespace.yml
```

**Purpose:**
Logical isolation of workloads.

---

### Pods & Deployments

```bash
kubectl apply -f pod.yml
kubectl apply -f deployment.yml
kubectl get pods
kubectl get pods -n <namespace>
kubectl describe pod <pod-name> -n <namespace>
kubectl logs <pod-name> -n <namespace>
kubectl exec -it <pod-name> -n <namespace> -- bash
```

**Purpose:**
Deploy and debug applications.

---

### Scaling

```bash
kubectl scale deployment nginx-deployment -n nginx-ns --replicas=5
```

**Purpose:**
Manually scale pods.

---

## 🌐 6. Services & Networking

### Services

```bash
kubectl apply -f service.yml
kubectl get svc -n <namespace>
```

### Port forwarding

```bash
kubectl port-forward svc/<service-name> 5173:5173 --address=0.0.0.0
```

**Purpose:**
Expose Kubernetes services locally without LoadBalancer.

---

## 🚪 7. Ingress (NGINX)

### Install ingress controller (Kind)

```bash
kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml
kubectl get pods -n ingress-nginx
kubectl get svc -n ingress-nginx
```

### Ingress configuration

```bash
vim ingress.yml
kubectl apply -f ingress.yml
```

---

## 💾 8. Storage (PV & PVC – MySQL)

### Persistent Volume

```bash
kubectl apply -f persistentVolume.yml
kubectl get pv
```

### Persistent Volume Claim

```bash
kubectl apply -f persistentVolumeClaim.yml
kubectl get pvc -n mysql
```

**Purpose:**
Ensure MySQL data persists even if pods restart.

---

## 🗄️ 9. MySQL on Kubernetes

### Secrets & ConfigMaps

```bash
echo -n "Admin123" | base64
kubectl apply -f secrets.yml
kubectl apply -f configmap.yml
kubectl get secrets -n mysql
```

### Deployment

```bash
kubectl apply -f deployment.yml
kubectl get pods -n mysql
kubectl logs <mysql-pod> -n mysql
```

---

## 📊 10. Metrics Server & HPA

### Install Metrics Server

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl -n kube-system rollout restart deployment metrics-server
```

### Verify metrics

```bash
kubectl top nodes
kubectl top pods -n online-shop
```

---

### Horizontal Pod Autoscaler

```bash
vim hpa.yml
kubectl apply -f hpa.yml
kubectl get hpa -n online-shop
```

**Purpose:**
Automatically scale pods based on CPU usage.

---

## 📦 11. Helm & Jenkins

### Install Helm

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4
chmod 700 get_helm.sh
./get_helm.sh
helm version
```

---

### Jenkins using Helm

```bash
helm repo add jenkins https://charts.jenkins.io
helm repo update
helm install jenkins-demo jenkins/jenkins --create-namespace
```

### Access Jenkins

```bash
kubectl port-forward svc/jenkins-demo 8080:8080 --address=0.0.0.0
```

### Get admin password

```bash
kubectl get secret jenkins-demo -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode
```

---

## 📌 Summary

This README covers:

* Linux basics
* Docker lifecycle
* Kubernetes cluster creation (Kind)
* Core K8s objects (Pod, Deployment, Service)
* Storage (PV/PVC)
* Autoscaling (HPA)
* Helm & Jenkins



Just tell me 👍
