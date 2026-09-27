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


---

# Scaling & Rolling Updates

Replica scaling and rolling updates were implemented without downtime.

### Replica Scaling

Frontend replicas were increased successfully inside the Kubernetes cluster.

### Rolling Update Strategy

* `maxUnavailable = 0`
* `maxSurge = 1`

The rollout completed successfully using Kubernetes Deployment strategy.


---

# Application Verification

## User Authentication

A new user account was registered and authenticated successfully through the Auth Service.

> Insert login screenshot.

---

## Video Upload

The Admin Service uploads videos and thumbnails to Amazon S3 while storing metadata in MongoDB.

---

## Video Playback

Uploaded videos are available in the Streaming Service catalogue and stream successfully through the frontend.

---

## Live Chat

The WebSocket Chat Service was verified using two browser tabs.

Evidence included:

* Two-tab chat demonstration
* Live broadcast of messages
* Recorded verification video

Video:


---

# Self-Healing Verification

A running application pod was deleted manually.

Kubernetes automatically recreated the pod, restoring the desired replica count without affecting application availability.


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

# Images

<img width="1408" height="258" alt="06-ecr-repositories" src="https://github.com/user-attachments/assets/8362533c-ce0d-4ae6-993b-bc1a098162d6" />

<img width="1917" height="938" alt="07-jenkins-auto-trigger" src="https://github.com/user-attachments/assets/2d094a15-d867-4b98-bb8c-1b050e36ed80" />

<img width="1757" height="931" alt="08-eks-cluster-created" src="https://github.com/user-attachments/assets/fd13338c-487a-4405-91ab-5a41037fcf02" />
<img width="1767" height="725" alt="Screenshot 2026-09-26 220141" src="https://github.com/user-attachments/assets/2ae46afd-759e-459b-a360-c6e9a5902a35" />

<img width="1047" height="310" alt="Screenshot 2026-09-26 221209" src="https://github.com/user-attachments/assets/63105d3f-4615-4bac-88d6-3200dd40e3d7" />

<img width="1355" height="603" alt="Screenshot 2026-09-26 221956" src="https://github.com/user-attachments/assets/eefdebf9-e088-4159-995e-b8f317ef330a" />

<img width="1442" height="603" alt="Screenshot 2026-09-27 101856" src="https://github.com/user-attachments/assets/60c821ce-dfab-43ac-9d25-c45240683a28" />

<img width="1393" height="418" alt="Screenshot 2026-09-27 110519" src="https://github.com/user-attachments/assets/b9fe1f3a-93ca-4a85-8cdd-720286b281d5" />

<img width="1392" height="360" alt="Screenshot 2026-09-27 111019" src="https://github.com/user-attachments/assets/9432a9fb-63de-422e-8366-d4b7041bed8f" />
<img width="1297" height="550" alt="Screenshot 2026-09-27 111034" src="https://github.com/user-attachments/assets/84471a26-705d-4250-a47a-579d38f4335e" />
<img width="1180" height="311" alt="Screenshot 2026-09-27 111311" src="https://github.com/user-attachments/assets/95fbf8a4-44ec-4a86-ab9e-4d787dba482c" />

<img width="962" height="665" alt="Screenshot 2026-09-27 130328" src="https://github.com/user-attachments/assets/f2276227-9f69-4aae-9dd4-b639f0482e3e" />

<img width="1917" height="1031" alt="Screenshot 2026-09-27 143253" src="https://github.com/user-attachments/assets/4fbe1466-e86c-489d-8fcf-82344c07992e" />

<img width="1467" height="242" alt="Screenshot 2026-09-27 155616" src="https://github.com/user-attachments/assets/6151256d-30a9-4c48-a520-f3a66c00c756" />
<img width="812" height="726" alt="Screenshot 2026-09-27 155836" src="https://github.com/user-attachments/assets/acfe2c85-63f1-48dc-af89-5883019a12ea" />
<img width="812" height="726" alt="Screenshot 2026-09-27 155836" src="https://github.com/user-attachments/assets/594a375d-2725-4fcd-8f6a-1bfd473f49d0" />

<img width="1520" height="850" alt="Screenshot 2026-09-27 165539" src="https://github.com/user-attachments/assets/1ceb7486-1da6-4543-9e45-a6920628867c" />
<img width="1470" height="197" alt="Screenshot 2026-09-27 170244" src="https://github.com/user-attachments/assets/407e2040-d8f1-474d-a2f1-781a3f6e0e8e" />

<img width="1901" height="970" alt="Screenshot 2026-09-27 181143" src="https://github.com/user-attachments/assets/6f29b6fb-200b-41e0-8a60-ea023aa3c0db" />
<img width="1900" height="972" alt="Screenshot 2026-09-27 181157" src="https://github.com/user-attachments/assets/249a6975-e61e-4bdb-9399-44850cb211e6" />
<img width="1902" height="897" alt="Screenshot 2026-09-27 181218" src="https://github.com/user-attachments/assets/079c63df-86a4-446e-b16a-6c0adc381bc7" />


<img width="1102" height="827" alt="Screenshot 2026-09-27 193518" src="https://github.com/user-attachments/assets/a3c61292-8814-4ce7-8f47-f08ef0adada8" />

