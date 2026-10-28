# Real-Time Data Pipeline & Analytic Dashboard (RTDP)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Docker](https://img.shields.io/badge/Docker-Ready-blue)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-Compatible-326CE5)](https://kubernetes.io/)
[![Red Hat OpenShift](https://img.shields.io/badge/OpenShift-Supported-EE0000)](https://www.redhat.com/en/technologies/cloud-computing/openshift)

An end-to-end cloud-native microservices architecture designed to simulate real-time event streaming, stream processing, data storage, and analytics visualization. 

This repository serves as a hands-on implementation project to demonstrate containerization with **Docker**, orchestration with **Kubernetes**, and enterprise deployment using **Red Hat OpenShift**.

---

## 📐 Architecture Overview

The system consists of four primary microservices communicating asynchronously:

```
[ Data Producer ] ──(JSON Events)──> [ Apache Kafka / Broker ]
                                              │
                                              ▼
[ Streamlit Dashboard ] <──(SQL)── [ Stream Consumer ]
```

1. **Producer Service (Python / FastAPI):** Generates and streams synthetic real-time event metrics (e.g., system telemetry, transactions, or log streams) to the message broker.
2. **Message Broker (Apache Kafka / RabbitMQ):** Handles high-throughput, asynchronous message queuing and decoupling between services.
3. **Consumer & Processing Engine (Python):** Ingests events from the broker, applies real-time transformations and aggregations, and persists state into a datastore.
4. **Analytics Dashboard (Streamlit / Python):** A lightweight web interface displaying real-time metrics, system health, and aggregated statistics.

---

## 🛠️ Tech Stack & Prerequisites

* **Languages:** Python 3.11+
* **Containerization:** Docker, Docker Compose
* **Orchestration:** Kubernetes (Minikube / Kind)
* **Cloud Platform:** Red Hat OpenShift (Developer Sandbox)
* **Message Broker:** Apache Kafka / RabbitMQ
* **Datastore:** PostgreSQL / SQLite

### Prerequisites
Before running the project locally, ensure you have the following installed:
* [Docker Desktop](https://www.docker.com/products/docker-desktop/) or `docker-engine` + `docker-compose`
* [kubectl](https://kubernetes.io/docs/tasks/tools/)
* [Minikube](https://minikube.sigs.k8s.io/docs/start/) or [Kind](https://kind.sigs.k8s.io/)
* [OpenShift CLI (`oc`)](https://docs.openshift.com/container-platform/latest/cli_reference/openshift_cli/getting-started-cli.html) *(Optional for Phase 4)*

---

## 📂 Repository Structure

```text
RTDP/
├── .github/              # CI/CD Workflows (GitHub Actions)
├── producer/             # Producer microservice (FastAPI/Python)
│   ├── app/
│   └── Dockerfile
├── consumer/             # Stream processing worker
│   ├── app/
│   └── Dockerfile
├── dashboard/            # Real-time UI (Streamlit)
│   ├── app/
│   └── Dockerfile
├── k8s/                  # Kubernetes manifests (Deployments, Services, ConfigMaps)
│   ├── producer/
│   ├── consumer/
│   ├── dashboard/
│   └── base/
├── openshift/            # OpenShift specific manifests (Routes, BuildConfigs, SCC)
├── docker-compose.yml    # Local multi-container orchestration
├── LICENSE
└── README.md
```

---

## 🚀 Getting Started

### 1. Running Locally with Docker Compose

To spin up the entire pipeline locally:

```bash
# Clone the repository
git clone https://github.com/davideippolito/RTDP.git
cd RTDP

# Build and start all services
docker compose up --build -d

# Check running containers
docker compose ps
```

The services will be available at:
* **Dashboard:** `http://localhost:8501`
* **Producer API:** `http://localhost:8000/docs`

---

## ☸️ Kubernetes Deployment

Deploying the microservices stack to a local Kubernetes cluster (Minikube/Kind):

```bash
# Start local cluster
minikube start

# Apply configurations and deployments
kubectl apply -f k8s/base/
kubectl apply -f k8s/producer/
kubectl apply -f k8s/consumer/
kubectl apply -f k8s/dashboard/

# Verify status
kubectl get pods -w
```

---

## 🔴 Red Hat OpenShift Porting

Key enterprise features applied during OpenShift deployment:
* **Routes:** Replacing standard Kubernetes Ingress objects for external service exposure with managed TLS.
* **Source-to-Image (S2I):** Utilizing OpenShift `BuildConfigs` and `ImageStreams` for automated container builds directly from Git.
* **Security Context Constraints (SCC):** Ensuring non-root container execution in accordance with OpenShift security policies.

To deploy via `oc` CLI:

```bash
oc login --token=<YOUR_TOKEN> --server=<YOUR_OPENSHIFT_SERVER>
oc new-project rtdp-pipeline
oc apply -f openshift/
```

---

## 📜 Roadmap

- [x] Initial project setup and architecture design
- [ ] **Phase 1:** Dockerfile optimization & Docker Compose setup
- [ ] **Phase 2:** Kubernetes manifests development (`Deployment`, `Service`, `ConfigMap`)
- [ ] **Phase 3:** Persistence setup (`PVC`) & Horizontal Pod Autoscaler (`HPA`)
- [ ] **Phase 4:** Red Hat OpenShift deployment & Route configuration

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.