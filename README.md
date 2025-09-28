# NGINX Deployment on Kubernetes

This repository contains Kubernetes manifests to deploy an **NGINX application**.  
It includes namespace, pod, deployment, service, and cluster configuration files.

---
# Kubernetes Setup on Linux

Follow these steps to install Docker, Kind, and kubectl on Linux.

## 1. Install Docker
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
rm get-docker.sh
```

## 2. Install Kind
```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

## 3. Install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

## 📂 Files
- `namespace.yml` → Creates a dedicated namespace for the app.
- `pod.yml` → Defines a single NGINX pod.
- `deployment.yml` → Deployment for NGINX (manages replicas, rolling updates).
- `service.yml` → Exposes the NGINX deployment inside/outside cluster.
- `cluster.yml` → (Optional) Cluster configuration file.

---

## 🚀 Steps to Deploy

### 1. Create Namespace
```bash
kubectl apply -f namespace.yml
```

Verify:
```bash
kubectl get namespaces
```

---

### 2. Deploy a Pod (Optional)
```bash
kubectl apply -f pod.yml -n nginx-deployment
```

Check:
```bash
kubectl get pods -n nginx-deployment
```

---

### 3. Create Deployment
```bash
kubectl apply -f deployment.yml -n nginx-deployment
```

Check:
```bash
kubectl get deployments -n nginx-deployment
kubectl get pods -n nginx-deployment
```

---

### 4. Expose Deployment with Service
```bash
kubectl apply -f service.yml -n nginx-deployment
```

Check:
```bash
kubectl get svc -n nginx-deployment
```

If service type is **NodePort** or **LoadBalancer**, use the external IP/port to access NGINX.

---

### 5.  Apply Cluster Config
If you have a custom cluster config:
```bash
kubectl apply -f cluster.yml
```



---

## RUN 
kubectl port-forward svc/nginx-svc -n nginx-ns 82:82 --address=0.0.0.

---

---

## ✅ Verify Deployment
Open in browser:
```
http://<public-IP>:82
```

You should see the default **NGINX Welcome Page**.

---


---

## 📌 Notes
- Make sure `kubectl` is configured to point to your cluster.
- Change `replicas` in `deployment.yml` as needed.
- Update `service.yml` to `LoadBalancer` if running on cloud providers like AWS, GCP, or Azure.
