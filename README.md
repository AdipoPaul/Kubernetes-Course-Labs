# Widgetario: Kubernetes Application Deployment & Observability

## Overview

Widgetario is a sample multi-component application designed to demonstrate key concepts of Kubernetes including deployment, configuration, networking, storage, observability, and CI/CD. The application simulates a product management system with a frontend, APIs, and databases. This repository documents each part of the deployment pipeline and observability stack, providing insights into how to operationalize a cloud-native application.

---

## Repository Structure

```
widgetario/
├── part1-core-apps/          # Deployments for databases and APIs
├── part2-configure-apps/     # Secrets, ConfigMaps, probes, and environment variables
├── part3-storage/            # Persistent Volume and VolumeClaim setups
├── part4-ingress/            # Ingress setup for local DNS access
├── part5-production/         # Liveness/Readiness probes, resource limits, secrets, limits
├── part6-monitoring/         # Grafana, Prometheus setup
├── part7-efk-logging/        # Elasticsearch, Fluent Bit, Kibana stack
├── dashboards/               # Grafana, Prometheus an Kibana dashboards
├── tests/                    # Testing scripts and test cases
├── README.md                 # Project overview and documentation
└── deploy.sh                 # Optional deployment script
```

---

## Part 1: Core App Deployment

Deploys the following components:

* `products-db`: A PostgreSQL database
* `products-api`: Backend API to manage product data
* `stock-api`: Backend API for stock management
* `web`: A Node.js frontend served via Nginx

### Commands:

```bash
kubectl apply -f part1-core-apps/products-db \
  -f hackathon/solution-part-1/products-api \
  -f hackathon/solution-part-1/stock-api \
  -f hackathon/solution-part-1/web
```

---

## Part 2: Configure Apps

In this phase, we:

* Use `ConfigMaps` for environment variables
* Use `Secrets` for sensitive data
* Configure liveness and readiness probes
* Inject configuration using the Kubernetes Downward API

### Commands:

```bash
kubectl apply -f part2-configure-apps/
```

---

## Part 3: Storage

This section provisions persistent storage for `products-db` using:

* `PersistentVolume`
* `PersistentVolumeClaim`
* `StatefulSet` for `products-db`

### Commands:

```bash
kubectl apply -f part3-storage/
```

---

## Part 4: Ingress

Enables access via `http://widgetario.local` instead of `NodePort`. Ingress controller setup includes:

* Deploying NGINX Ingress Controller
* Creating an Ingress resource for the frontend

### Commands:

```bash
kubectl apply -f part4-ingress/
```

Update local `/etc/hosts` with:

```
127.0.0.1 widgetario.local
```

---

## Part 5: Productionizing

This phase configures:

* Liveness and readiness probes
* CPU and memory resource requests and limits
* Secrets for DB passwords and API keys

### Commands:

```bash
kubectl apply -f part5-production/
```

---

## Part 6: Monitoring (Prometheus + Grafana)

Deploy monitoring tools under the `monitoring` namespace:

* Prometheus: Metric collection and scraping
* Grafana: Visual dashboards using imported JSON templates

### Access:

```bash
minikube service grafana-np -n monitoring --url
```

Login:

* **User**: `admin`
* **Password**: `labs` (set via `grafana-creds` secret)

### Commands:

```bash
kubectl apply -f part6-monitoring/
```

Import dashboards via UI or `dashboards/` folder.

---

## Part 7: Logging (EFK Stack)

Install Elasticsearch, Fluent Bit, and Kibana under the `logging` namespace:

* **Elasticsearch**: Log storage
* **Fluent Bit**: Log collector
* **Kibana**: Visualization tool

### Access Kibana:

```bash
minikube service kibana-np -n logging --url
```

### Commands:

```bash
kubectl apply -f part7-efk-logging/
```

---

## Installation & Deployment

Ensure Minikube is installed and started.

```bash
minikube start
```

Apply each part sequentially:

```bash
kubectl apply -f part1-core-apps/
kubectl apply -f part2-configure-apps/
...
kubectl apply -f part7-efk-logging/
```

Optionally, use `deploy.sh` to automate the sequence.

---

## Testing

Tests are stored under `tests/` and include curl commands or automation scripts:

```bash
curl http://widgetario.local/api/products
curl http://widgetario.local/api/stock
```

To run unit tests for services:

```bash
cd tests/
npm test  # or appropriate framework
```

---

## Dependencies

* Minikube
* kubectl
* Node.js / PostgreSQL
* Prometheus, Grafana
* Elasticsearch, Fluent Bit, Kibana

---

## Contribution Guidelines

Feel free to fork and open a pull request. This repo is structured for educational and hackathon demonstration purposes.

---
