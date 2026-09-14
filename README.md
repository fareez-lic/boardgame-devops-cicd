# 🎲 End-to-End DevOps CI/CD Pipeline for BoardGame Application

## 📌 Project Overview

This project demonstrates the implementation of a complete end-to-end DevOps CI/CD pipeline for a Java-based BoardGame web application.

The goal of this project is to automate the software delivery lifecycle — from source code management and application build to code-quality analysis, security scanning, artifact management, containerization, and deployment to a Kubernetes cluster running on AWS.

The project demonstrates practical experience with:

- AWS EC2
- Git & GitHub
- Jenkins
- Maven
- SonarQube
- Trivy
- Nexus Repository
- Docker
- Kubernetes
- kubeadm
- containerd
- Flannel CNI
- Prometheus
- Grafana

---

## 👨‍💻 My DevOps Implementation

My work in this project focuses on designing, configuring, and implementing the DevOps infrastructure and CI/CD pipeline around the application.

The DevOps workflow includes:

1. Source code management using Git and GitHub
2. Automated CI/CD pipeline using Jenkins
3. Java application build and testing using Maven
4. Static code-quality analysis using SonarQube
5. Filesystem and container vulnerability scanning using Trivy
6. Artifact management using Nexus Repository
7. Containerization using Docker
8. Kubernetes cluster creation using kubeadm
9. Container runtime configuration using containerd
10. Kubernetes networking using Flannel CNI
11. Application deployment to Kubernetes
12. Monitoring using Prometheus and Grafana

---

## 🏗️ CI/CD Architecture

```text
Developer
    |
    v
GitHub
    |
    v
Jenkins
    |
    +--------------------+
    |                    |
    v                    v
Maven Build          Trivy Scan
    |
    v
SonarQube
Code Quality Analysis
    |
    v
Maven Package
    |
    v
Nexus Repository
Artifact Storage
    |
    v
Docker Build
    |
    v
Trivy Image Scan
    |
    v
Container Registry
    |
    v
Kubernetes Cluster
    |
    +---------------------------+
    |                           |
    v                           v
Control Plane                Worker Node
                                |
                                v
                         BoardGame Application
                                |
                                v
                     Prometheus + Grafana
                          Monitoring
```

---

## ☁️ AWS Infrastructure

The DevOps environment is hosted on AWS EC2.

| Server | Purpose |
|---|---|
| Jenkins Server | CI/CD automation |
| SonarQube Server | Static code-quality analysis |
| Nexus Server | Artifact repository |
| Kubernetes Control Plane | Kubernetes cluster management |
| Kubernetes Worker | Runs application workloads |

The Kubernetes environment consists of a two-node cluster:

```text
Kubernetes Cluster

Control Plane
     |
     |
     +-------- Worker Node
```

The cluster was created manually using **kubeadm** rather than using a managed Kubernetes service.

---

## ☸️ Kubernetes Configuration

The Kubernetes cluster uses:

- Kubernetes v1.35
- kubeadm
- kubelet
- kubectl
- containerd
- Flannel CNI

Cluster architecture:

```text
AWS VPC
│
├── Kubernetes Control Plane
│      ├── kube-apiserver
│      ├── scheduler
│      ├── controller-manager
│      └── etcd
│
└── Kubernetes Worker
       ├── kubelet
       ├── containerd
       └── Application Pods
```

Cluster status can be verified using:

```bash
kubectl get nodes
```

Example:

```text
NAME               STATUS   ROLES           VERSION
control-plane      Ready    control-plane   v1.35.x
worker-node        Ready    <none>          v1.35.x
```

---

## 🔄 CI/CD Pipeline Workflow

### 1. Source Code

Developers push application code to GitHub.

```text
Developer → GitHub
```

Jenkins retrieves the latest source code from the repository.

---

### 2. Maven Build

Maven compiles the Java application and executes tests.

```bash
mvn clean compile
mvn test
```

---

### 3. Trivy Filesystem Scan

Trivy scans the project filesystem and dependencies for known vulnerabilities.

```text
Source Code
     |
     v
Trivy Filesystem Scan
```

---

### 4. SonarQube Analysis

SonarQube performs static code analysis to identify:

- Bugs
- Vulnerabilities
- Code smells
- Maintainability issues
- Reliability issues

```text
Jenkins
   |
   v
SonarQube
   |
   v
Quality Analysis
```

---

### 5. Maven Package

After successful analysis, Maven packages the application.

```bash
mvn package
```

The resulting artifact is generated in the Maven target directory.

---

### 6. Nexus Artifact Repository

The packaged application artifact is published to Nexus Repository.

```text
Jenkins
   |
   v
Maven Artifact
   |
   v
Nexus Repository
```

Nexus provides centralized artifact storage and version management.

---

### 7. Docker Image Build

Jenkins builds a Docker image for the BoardGame application.

```text
Application
    +
Dockerfile
    |
    v
Docker Image
```

---

### 8. Trivy Container Image Scan

Before deployment, the Docker image is scanned using Trivy.

```text
Docker Image
     |
     v
Trivy
     |
     v
Vulnerability Report
```

This helps detect known security vulnerabilities before deployment.

---

### 9. Container Registry

After successful scanning, the container image is pushed to a container registry.

```text
Jenkins
   |
   v
Docker Image
   |
   v
Container Registry
```

---

### 10. Kubernetes Deployment

Jenkins deploys the application using Kubernetes manifests.

```bash
kubectl apply -f deployment-service.yaml
```

The application runs as a Kubernetes workload on the worker node.

```text
Jenkins
   |
   v
Kubernetes API
   |
   v
Worker Node
   |
   v
Application Pod
```

---

## 📊 Monitoring

Prometheus and Grafana are used for monitoring the Kubernetes environment and application infrastructure.

### Prometheus

Prometheus collects metrics from the environment.

### Grafana

Grafana provides dashboards for visualizing collected metrics.

```text
Kubernetes
    |
    v
Prometheus
    |
    v
Grafana
    |
    v
Monitoring Dashboards
```

---

## 🔐 Security Practices

Several security practices are incorporated into the project:

- SSH key-based EC2 access
- AWS Security Groups with restricted inbound access
- Service-to-service Security Group rules
- No private SSH keys stored in Git
- Credentials managed separately from source code
- Trivy filesystem vulnerability scanning
- Trivy Docker image vulnerability scanning
- SonarQube code-quality analysis
- Kubernetes node communication restricted using AWS networking rules

Sensitive information such as the following should never be committed:

```text
Private SSH keys
AWS credentials
Jenkins passwords
SonarQube tokens
Nexus credentials
Kubernetes join tokens
API keys
```

---

## 📁 Repository Structure

```text
boardgame-devops-cicd/
│
├── src/
│
├── Dockerfile
│
├── Jenkinsfile
│
├── deployment-service.yaml
│
├── pom.xml
│
├── sonar-project.properties
│
├── mvnw
├── mvnw.cmd
│
└── README.md
```

---

## 🎲 BoardGame Application

The sample application is a Java/Spring Boot BoardGame listing web application.

Application technologies include:

- Java
- Spring Boot
- Spring MVC
- Spring Security
- Thymeleaf
- HTML
- CSS
- JavaScript
- JDBC
- H2 Database
- JUnit
- Maven

The application allows users to browse board games and reviews, while authenticated users can perform additional operations based on their assigned roles.

---

## 📸 Project Evidence

The project was implemented step by step on AWS.

Portfolio evidence includes:

```text
01 - Jenkins EC2 Server
02 - Jenkins SSH Connection
03 - Jenkins Service Running
04 - Jenkins Dashboard

05 - SonarQube EC2 Server
06 - Docker Running on SonarQube
07 - SonarQube Container Running
08 - SonarQube Dashboard

09 - Nexus EC2 Server
10 - Docker Running on Nexus
11 - Nexus Container Running
12 - Nexus Dashboard

13 - Kubernetes Control Plane EC2
14 - Kubernetes Control Plane Versions

15 - Kubernetes Worker EC2
16 - Kubernetes Worker Versions

17 - Kubernetes Cluster Nodes Ready
```

Additional CI/CD, deployment, and monitoring screenshots will be added as the project progresses.

---

## 🎯 Project Goals

This project demonstrates practical knowledge of:

```text
Continuous Integration
Continuous Delivery / Deployment
Infrastructure Configuration
Linux Administration
Cloud Infrastructure
Artifact Management
Containerization
Container Security
Kubernetes Administration
Application Deployment
Monitoring
DevSecOps Practices
```

---

## 🚀 Final Pipeline

```text
GitHub
   ↓
Jenkins
   ↓
Maven Build & Test
   ↓
Trivy Filesystem Scan
   ↓
SonarQube Analysis
   ↓
Quality Gate
   ↓
Maven Package
   ↓
Nexus Repository
   ↓
Docker Build
   ↓
Trivy Image Scan
   ↓
Container Registry
   ↓
Kubernetes Deployment
   ↓
Prometheus
   ↓
Grafana
```

---

## 📚 Application Attribution

The BoardGame application used as the workload for this DevOps project was originally developed by **Aditya Jaiswal** and is used here as a sample application for implementing and demonstrating the DevOps pipeline.

Original application repository:

`https://github.com/jaiswaladi246/Boardgame`

The DevOps infrastructure, CI/CD configuration, Kubernetes environment, integrations, security scanning, deployment workflow, documentation, and portfolio implementation in this repository are the focus of this project.

---

## 👤 Author

**Md Kamrul Chowdhury**

DevOps & Cloud Engineering Portfolio Project
