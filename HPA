Here is a **clean, focused README.md only for HPA / Autoscaling**
You can **directly copy–paste this into GitHub** ✅
(No extra topics, only autoscaling)

---

# 📈 Kubernetes Autoscaling (HPA) – Complete Guide

This README explains **Horizontal Pod Autoscaler (HPA)** in Kubernetes with **clear concepts, commands, YAML structure, and real troubleshooting**, based on hands-on practice.

---

## 🔹 What is Autoscaling in Kubernetes?

Autoscaling means **automatically adjusting resources** based on load.

Kubernetes supports:

* **HPA** → Scales number of pods
* **VPA** → Scales CPU/Memory of pods
* **Cluster Autoscaler** → Scales nodes

👉 This README focuses on **HPA (most commonly used in production)**.

---

## 🔹 What is HPA (Horizontal Pod Autoscaler)?

**HPA automatically increases or decreases the number of pods** in a Deployment/StatefulSet based on metrics like:

* CPU usage
* Memory usage
* Custom metrics

### Example:

* Traffic increases → pods scale from 2 → 5
* Traffic decreases → pods scale back to 2

---

## 🔹 Prerequisites for HPA

### 1️⃣ Metrics Server (MANDATORY)

HPA **will not work** without Metrics Server.

Check:

```bash
kubectl top nodes
kubectl top pods
```

If not installed:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Restart:

```bash
kubectl -n kube-system rollout restart deployment metrics-server
```

---

### 2️⃣ Deployment with resource requests

HPA requires **CPU requests** in Deployment.

Example:

```yaml
resources:
  requests:
    cpu: "100m"
    memory: "128Mi"
```

---

## 🔹 HPA YAML (autoscaling/v2 – Recommended)

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: online-shop-hpa
  namespace: online-shop
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: online-shop-deployment

  minReplicas: 1
  maxReplicas: 5

  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

### 🔑 Explanation

* **scaleTargetRef** → which Deployment to scale
* **minReplicas** → minimum pods
* **maxReplicas** → maximum pods
* **averageUtilization** → CPU % threshold

---

## 🔹 Apply & Verify HPA

```bash
kubectl apply -f hpa.yml
kubectl get hpa -n online-shop
kubectl describe hpa online-shop-hpa -n online-shop
```

Check pod scaling:

```bash
kubectl get pods -n online-shop
```

---

## 🔹 autoscaling/v1 (Fallback – CPU only)

Use this if your cluster does **not support autoscaling/v2**.

```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: online-shop-hpa
  namespace: online-shop
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: online-shop-deployment
  minReplicas: 1
  maxReplicas: 5
  targetCPUUtilizationPercentage: 70
```

---

## 🔹 Common Errors & Fixes

### ❌ `no matches for kind HorizontalPodAutoscaler`

✔️ Kubernetes version does not support `autoscaling/v2`
👉 Use `autoscaling/v1`

---

### ❌ `unknown field spec.scaleTargetRef.maxReplicas`

✔️ Wrong YAML indentation
👉 `minReplicas`, `maxReplicas`, `metrics` must be **outside** `scaleTargetRef`

---

### ❌ `metrics not available`

✔️ Metrics Server not running
👉 Restart metrics-server

---

### ❌ HPA not scaling

✔️ CPU usage not crossing threshold
✔️ Resource requests missing in Deployment

---

## 🔹 Load Testing (to Trigger HPA)

```bash
kubectl exec -it <pod-name> -- sh
```

Inside pod:

```sh
while true; do wget -q -O- http://localhost; done
```

Watch scaling:

```bash
watch kubectl get pods -n online-shop
```

---

## 🔹 HPA vs VPA (Quick Comparison)

| Feature        | HPA            | VPA         |
| -------------- | -------------- | ----------- |
| Scales         | Pods           | CPU/Memory  |
| Pod restart    | ❌ No           | ✅ Yes       |
| Production use | ✅ Very common  | ⚠️ Careful  |
| Best for       | Web apps, APIs | Jenkins, DB |

👉 **HPA is preferred in most production workloads**

---

## 🔹 Best Practices (Production)

* Always define **CPU requests**
* Use **HPA for stateless apps**
* Avoid HPA + VPA on CPU together
* Monitor scaling using:

  ```bash
  kubectl describe hpa
  ```

---

## 🧠 Interview-Ready Answer

> **HPA automatically scales pods based on metrics like CPU usage. It is preferred in production because it scales without restarting pods and ensures high availability.**

---

## ✅ Summary

This README covered:

* What HPA is
* How it works
* Metrics Server
* Correct YAML structure
* Common errors & fixes
* Production best practices

---
