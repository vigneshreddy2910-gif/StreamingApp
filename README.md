# StreamingApp — Container Orchestration on Kubernetes

A complete end-to-end deployment of a **5-service streaming platform** using **Docker, Amazon EKS, Kubernetes, Helm, Jenkins CI/CD, Amazon ECR, MongoDB, and AWS Application Load Balancer**.

This project was completed as part of the **Container Orchestration Assignment (DevOps Track)** and demonstrates packaging, deploying, exposing, scaling, and verifying a microservices-based application on Kubernetes.

---

## Project Overview

StreamingApp consists of five independent services and one shared MongoDB database.

| Service           |  Port | Purpose                         |
| ----------------- | ----: | ------------------------------- |
| Frontend          |    80 | React SPA served through Nginx  |
| Auth Service      |  3001 | Registration, Login & JWT       |
| Streaming Service |  3002 | Video catalogue & playback      |
| Admin Service     |  3003 | Video upload & asset management |
| Chat Service      |  3004 | WebSocket live chat             |
| MongoDB           | 27017 | Persistent application database |

The application is deployed on **Amazon Elastic Kubernetes Service (EKS)** using **Helm**, while **Jenkins** automatically builds Docker images, pushes them to **Amazon ECR**, and deploys new versions to the cluster.

---

# Assignment Objectives Achieved

* Containerized all five services using Docker
* Built and versioned images in Amazon ECR
* Created Kubernetes Deployments and Services
* Configured ConfigMaps and Secrets
* Deployed MongoDB using StatefulSet with Persistent Volume
* Packaged the platform as a Helm chart
* Exposed the application through AWS ALB Ingress
* Implemented rolling updates and replica scaling
* Verified login, upload, playback, live chat, and self-healing
* Automated CI/CD using Jenkins

---

# System Architecture

> Insert architecture screenshot here.

**Suggested file**

`docs/architecture/architecture.png`

The platform follows a microservice architecture where all client traffic enters through a single AWS Application Load Balancer and is routed to individual Kubernetes services.

---

# Technology Stack

| Category         | Technology                                 |
| ---------------- | ------------------------------------------ |
| Cloud            | Amazon Web Services                        |
| Containerization | Docker                                     |
| Registry         | Amazon ECR                                 |
| Orchestration    | Kubernetes (Amazon EKS)                    |
| Packaging        | Helm 3                                     |
| CI/CD            | Jenkins                                    |
| Database         | MongoDB StatefulSet                        |
| Frontend         | React + Nginx                              |
| Backend          | Node.js                                    |
| Networking       | AWS Load Balancer Controller + ALB Ingress |

---

# Repository Structure

```text
StreamingApp/
├── backend/
│   ├── authService/
│   ├── streamingService/
│   ├── adminService/
│   └── chatService/
│
├── frontend/
│
├── kubernetes/
│
├── helm/
│   └── streamingapp/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│
├── docs/
│
└── README.md
```

---

# Docker Containerization

Each microservice was containerized independently.

| Image                  | Version |
| ---------------------- | ------- |
| streamingapp-auth      | 1.0.20  |
| streamingapp-streaming | 1.0.20  |
| streamingapp-admin     | 1.0.20  |
| streamingapp-chat      | 1.0.20  |
| streamingapp-frontend  | 1.0.20  |

> Insert ECR/Jenkins build screenshot.

---

# Kubernetes Resources

The application was translated from Docker Compose into Kubernetes resources.

| Resource              | Purpose                           |
| --------------------- | --------------------------------- |
| Deployment            | Runs each application service     |
| Service               | Internal ClusterIP networking     |
| ConfigMap             | Environment configuration         |
| Secret                | Sensitive application credentials |
| StatefulSet           | MongoDB deployment                |
| PersistentVolumeClaim | Persistent database storage       |
| Ingress               | External routing through ALB      |

> Insert Kubernetes resources screenshot.

---

# Helm Deployment

The entire platform is packaged into a reusable Helm chart.

## Chart Structure

```text
helm/streamingapp/
├── Chart.yaml
├── values.yaml
└── templates/
```

## Install

```bash
helm install streamingapp ./helm/streamingapp \
  --namespace streaming-app \
  --create-namespace
```

## Upgrade

```bash
helm upgrade streamingapp ./helm/streamingapp
```

> Insert Helm deployment screenshot.

---

# Ingress Routing

A single AWS Application Load Balancer exposes every service.

| Path             | Backend           |
| ---------------- | ----------------- |
| `/`              | Frontend          |
| `/api/auth`      | Auth Service      |
| `/api/streaming` | Streaming Service |
| `/api/admin`     | Admin Service     |
| `/api/chat`      | Chat Service      |

> Insert ALB / Ingress screenshot.

---

# Jenkins CI/CD Pipeline

The deployment pipeline performs the following automatically:

1. Checkout source from GitHub
2. Build five Docker images
3. Tag images with version `1.0.20`
4. Push images to Amazon ECR
5. Update Kubernetes manifests through Helm
6. Verify rollout status of every Deployment

Pipeline Result:

* Build: **SUCCESS**
* Images Built: **5**
* Registry: **Amazon ECR**
* Deployment: **Amazon EKS**
* Helm Release: **Revision 9**

> Insert Jenkins success screenshot.

---

# Scaling & Rolling Updates

Replica scaling and rolling updates were implemented without downtime.

### Replica Scaling

Frontend replicas were increased successfully inside the Kubernetes cluster.

### Rolling Update Strategy

* `maxUnavailable = 0`
* `maxSurge = 1`

The rollout completed successfully using Kubernetes Deployment strategy.

> Insert rollout/scaling screenshot.

---

# Application Verification

## User Authentication

A new user account was registered and authenticated successfully through the Auth Service.

> Insert login screenshot.

---

## Video Upload

The Admin Service uploads videos and thumbnails to Amazon S3 while storing metadata in MongoDB.

> Insert upload screenshot.

---

## Video Playback

Uploaded videos are available in the Streaming Service catalogue and stream successfully through the frontend.

> Insert playback screenshot.

---

## Live Chat

The WebSocket Chat Service was verified using two browser tabs.

Evidence included:

* Two-tab chat demonstration
* Live broadcast of messages
* Recorded verification video

Video:

`docs/verification/live-chat-demo.mp4`

> Insert chat screenshot.

---

# Self-Healing Verification

A running application pod was deleted manually.

Kubernetes automatically recreated the pod, restoring the desired replica count without affecting application availability.

> Insert self-healing screenshot.

---

# Final Cluster Verification

The final cluster contains:

* Running Pods
* ClusterIP Services
* AWS ALB Ingress
* MongoDB StatefulSet
* Helm Release

Verification command:

```bash
kubectl get pods,svc,ingress -A
```

> Insert final kubectl screenshot.

---

# How to Deploy

## Prerequisites

* Docker
* kubectl
* Helm 3
* AWS CLI
* Amazon EKS Cluster
* Jenkins (optional for CI/CD)

## Clone Repository

```bash
git clone https://github.com/vigneshreddy2910-gif/StreamingApp.git
cd StreamingApp
```

## Deploy with Helm

```bash
helm install streamingapp ./helm/streamingapp \
  --namespace streaming-app \
  --create-namespace
```

## Verify

```bash
kubectl get pods -n streaming-app
kubectl get svc -n streaming-app
kubectl get ingress -n streaming-app
```

---

# Project Outcome

The StreamingApp platform was successfully containerized, deployed, exposed, scaled, and verified on Amazon EKS using Kubernetes and Helm. A complete Jenkins CI/CD pipeline automates image delivery from GitHub to Amazon ECR and performs zero-downtime deployments into the Kubernetes cluster.
