# Widgetario Kubernetes Hackathon Project

Welcome to the **Widgetario** project, a Kubernetes-based microservices application built and deployed as part of a hands-on hackathon. This project spans multiple stages including configuration, storage, ingress, production hardening, observability, and CI/CD using popular cloud-native tools.

---

## 📦 Part 1: Core App Setup

### Objective

Deploy the core components of Widgetario:

* Products API
* Stock API
* PostgreSQL database

### Tools & Features

* Kubernetes YAML files
* StatefulSet for PostgreSQL
* Secrets for database credentials
* Deployments and Services for APIs

### Key Commands

```bash
kubectl apply -k k8s/core
```

### Result

All core services were successfully deployed in a dedicated namespace.

---

## 🌐 Part 2: Ingress Setup

### Objective

Expose the APIs and the Web frontend using Kubernetes Ingress.

### Tools Used

* NGINX Ingress Controller
* Custom Ingress Resources
* NodePort & LoadBalancer services for testing

### Steps

1. Install ingress-nginx using kubectl manifests
2. Configure DNS entries locally (`/etc/hosts`)
3. Access services via `http://widgetario.local`

---

## ⚙️ Part 3: Web Frontend Deployment

### Objective

Deploy the user-facing frontend for Widgetario.

### Features

* ConfigMap for feature flags
* Service and Deployment for the web UI
* Internal communication with APIs via services

### Steps

```bash
kubectl apply -k k8s/web
```

### Verification

Accessible via browser through Ingress (`widgetario.local`).

---

## 🔐 Part 4: Production Hardening

### Objective

Secure and stabilize the application stack.

### Features Implemented

* Readiness and liveness probes
* Resource limits (CPU/memory)
* HTTPS setup with secrets
* Password encryption for all APIs

### Result

Resilience, security, and stability improvements in all deployed services.

---

## 📊 Part 5: Monitoring with Grafana & Prometheus

### Objective

Enable observability using Prometheus and Grafana.

### Steps

1. Deploy Prometheus and Grafana using manifests
2. Create `grafana-creds` secret for login
3. Access Grafana: `http://<minikube-ip>:30003`

### Metrics Covered

* Cluster health
* Pod resource usage
* Service availability

---

## 📁 Part 6: Logging with EFK Stack (Elasticsearch, Fluent Bit, Kibana)

### Objective

Centralize logging for debugging and analytics.

### Components

* Elasticsearch for storage
* Fluent Bit for log shipping
* Kibana for visualization

### Access Kibana

```
http://<minikube-ip>:30005
```

### Logs Captured

* Application stdout/stderr
* Kubernetes events
* Container logs

---

## 🔄 Part 7: CI/CD with Jenkins and Gogs

### Objective

Automate builds and deployments using Jenkins and a Gogs Git server.

### Steps

1. Deploy Gogs:

```bash
kubectl apply -k k8s/ci/gogs
```

2. Setup Git user and repo in Gogs
3. Deploy Jenkins:

```bash
kubectl apply -k k8s/ci/jenkins
```

4. Configure Jenkins pipelines to pull code from Gogs
5. Auto-deploy on push via Git webhook

---

## 🚀 Conclusion

This project demonstrates the full lifecycle of building, deploying, monitoring, and automating a microservices-based application in Kubernetes. Each part is modular and can be scaled or adapted to real-world production needs.

---

## 🧾 Authors

* DevOps Team Widgetario

## 📜 License

MIT
