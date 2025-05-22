# Node.js App CI/CD with Jenkins and Kubernetes 🚀

This project demonstrates a complete CI/CD pipeline for a Node.js application using **Jenkins**, **Docker**, and **Kubernetes (Minikube)**. It automates the process from source code checkout to Docker image creation, pushing to DockerHub, and deploying to Kubernetes.

---

## 📸 Jenkins Pipeline Status

![Jenkins Pipeline](https://github.com/kushagra-g9/my-jenkins_app/assets/your-image-hash-path)

> 🟢 All stages completed successfully including build, push, and deployment!

---

## 📂 Project Structure

```bash
.
├── Dockerfile
├── Jenkinsfile
├── deployment-and-service.yml
├── README.md
└── src/

## 🛠️ Technologies Used
Node.js - Backend application

Docker - Containerization

DockerHub - Container registry

Jenkins - CI/CD orchestration

Kubernetes (Minikube) - Container orchestration

kubectl - K8s CLI for deployment

## 🔧 Jenkins Pipeline Stages
Checkout SCM – Clone code from GitHub

Build Docker Image – Dockerize the app

Push to DockerHub – Upload image to DockerHub

Check kubectl – Ensure kubectl is available

Deploy to Kubernetes – Deploy updated image using kubectl

Verify Deployment – Validate pods and services

Post Actions – Cleanup


## 🌐 Access the App

After successful deployment, run:

```bash
minikube service my-node-service
