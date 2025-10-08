# Movie Picture Pipeline - CI/CD Project

[![Frontend CI](https://github.com/Hzzxjj/prj4-upd/actions/workflows/frontend-ci.yaml/badge.svg)](https://github.com/Hzzxjj/prj4-upd/actions/workflows/frontend-ci.yaml)
[![Backend CI](https://github.com/Hzzxjj/prj4-upd/actions/workflows/backend-ci.yaml/badge.svg)](https://github.com/Hzzxjj/prj4-upd/actions/workflows/backend-ci.yaml)
[![Frontend CD](https://github.com/Hzzxjj/prj4-upd/actions/workflows/frontend-cd.yaml/badge.svg)](https://github.com/Hzzxjj/prj4-upd/actions/workflows/frontend-cd.yaml)
[![Backend CD](https://github.com/Hzzxjj/prj4-upd/actions/workflows/backend-cd.yaml/badge.svg)](https://github.com/Hzzxjj/prj4-upd/actions/workflows/backend-cd.yaml)

A complete CI/CD pipeline implementation for a full-stack movie application with React frontend and Flask backend, deployed to AWS EKS using GitHub Actions.



- **Frontend Application**: http://aab205cf6bcf442019b39ed063809cc3-1168313768.us-east-1.elb.amazonaws.com
- **Backend API**: http://aa9aa9f85052c445481fb7413c8b62a4-338532416.us-east-1.elb.amazonaws.com/movies

## 📋 Project Overview

This project demonstrates a complete DevOps CI/CD pipeline with:
- ✅ Automated testing and linting on every pull request
- ✅ Docker containerization
- ✅ AWS ECR for container registry
- ✅ Kubernetes deployment on AWS EKS
- ✅ GitHub Actions for automation
- ✅ Zero-downtime deployments

## 🏗️ Architecture

```
GitHub Repository → GitHub Actions → Docker Build → AWS ECR → AWS EKS → LoadBalancer
```

### Technology Stack

**Frontend:**
- React 18.2
- Node.js 18.14
- ESLint for code quality
- Jest for testing
- Docker containerization

**Backend:**
- Python 3.10
- Flask web framework
- Pytest for testing
- Flake8 for linting
- Docker containerization

**Infrastructure:**
- AWS EKS (Kubernetes cluster: `my-cluster`)
- AWS ECR (Container Registry)
- AWS LoadBalancer
- GitHub Actions for CI/CD

## 📁 Project Structure

```
prj4-upd/
├── .github/
│   └── workflows/
│       ├── frontend-ci.yaml          # Frontend CI pipeline
│       ├── backend-ci.yaml           # Backend CI pipeline
│       ├── frontend-cd.yaml          # Frontend CD pipeline
│       └── backend-cd.yaml           # Backend CD pipeline
├── frontend/                          # React application
│   ├── src/
│   ├── k8s/                          # Kubernetes manifests
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── kustomization.yaml
│   ├── Dockerfile
│   └── package.json
├── backend/                           # Flask application
│   ├── movies/
│   ├── k8s/                          # Kubernetes manifests
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── kustomization.yaml
│   ├── Dockerfile
│   └── Pipfile
└── README.md
```
