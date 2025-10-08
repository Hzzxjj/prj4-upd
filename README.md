# Movie Picture Pipeline - CI/CD Project

[![Frontend CI](https://github.com/Hzzxjj/prj4-upd/actions/workflows/frontend-ci.yaml/badge.svg)](https://github.com/Hzzxjj/prj4-upd/actions/workflows/frontend-ci.yaml)
[![Backend CI](https://github.com/Hzzxjj/prj4-upd/actions/workflows/backend-ci.yaml/badge.svg)](https://github.com/Hzzxjj/prj4-upd/actions/workflows/backend-ci.yaml)
[![Frontend CD](https://github.com/Hzzxjj/prj4-upd/actions/workflows/frontend-cd.yaml/badge.svg)](https://github.com/Hzzxjj/prj4-upd/actions/workflows/frontend-cd.yaml)
[![Backend CD](https://github.com/Hzzxjj/prj4-upd/actions/workflows/backend-cd.yaml/badge.svg)](https://github.com/Hzzxjj/prj4-upd/actions/workflows/backend-cd.yaml)

A complete CI/CD pipeline implementation for a full-stack movie application with React frontend and Flask backend, deployed to AWS EKS using GitHub Actions.

## 🚀 Live Application

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

## 🔄 CI/CD Workflows

### 1. Frontend Continuous Integration
**Trigger:** Pull requests to main branch  
**Workflow:** [frontend-ci.yaml](.github/workflows/frontend-ci.yaml)  
**View Runs:** [Frontend CI Actions](https://github.com/Hzzxjj/prj4-upd/actions/workflows/frontend-ci.yaml)

**Jobs:**
- **Lint** - Runs ESLint to check code quality
- **Test** - Runs Jest tests with coverage
- **Build** - Builds Docker image to verify no errors

Jobs run in parallel: `lint` and `test` → then `build`

### 2. Backend Continuous Integration
**Trigger:** Pull requests to main branch  
**Workflow:** [backend-ci.yaml](.github/workflows/backend-ci.yaml)  
**View Runs:** [Backend CI Actions](https://github.com/Hzzxjj/prj4-upd/actions/workflows/backend-ci.yaml)

**Jobs:**
- **Lint** - Runs Flake8 to check code quality
- **Test** - Runs Pytest tests
- **Build** - Builds Docker image to verify no errors

Jobs run in parallel: `lint` and `test` → then `build`

### 3. Frontend Continuous Deployment
**Trigger:** Push to main branch (after merge)  
**Workflow:** [frontend-cd.yaml](.github/workflows/frontend-cd.yaml)  
**View Runs:** [Frontend CD Actions](https://github.com/Hzzxjj/prj4-upd/actions/workflows/frontend-cd.yaml)

**Jobs:**
- **Lint** - Code quality check
- **Test** - Run all tests
- **Build & Deploy:**
  - Build Docker image with `REACT_APP_MOVIE_API_URL` environment variable
  - Push to ECR: `639850426965.dkr.ecr.us-east-1.amazonaws.com/frontend`
  - Deploy to EKS using kubectl and kustomize

### 4. Backend Continuous Deployment
**Trigger:** Push to main branch (after merge)  
**Workflow:** [backend-cd.yaml](.github/workflows/backend-cd.yaml)  
**View Runs:** [Backend CD Actions](https://github.com/Hzzxjj/prj4-upd/actions/workflows/backend-cd.yaml)

**Jobs:**
- **Lint** - Code quality check
- **Test** - Run all tests
- **Build & Deploy:**
  - Build Docker image
  - Push to ECR: `639850426965.dkr.ecr.us-east-1.amazonaws.com/backend`
  - Deploy to EKS using kubectl and kustomize

## 🔗 Important Links

### GitHub
- **Repository**: https://github.com/Hzzxjj/prj4-upd
- **Actions Dashboard**: https://github.com/Hzzxjj/prj4-upd/actions
- **Pull Requests**: https://github.com/Hzzxjj/prj4-upd/pulls
- **Settings**: https://github.com/Hzzxjj/prj4-upd/settings

### AWS Resources
- **ECR Frontend Repository**: https://console.aws.amazon.com/ecr/repositories/private/639850426965/frontend
- **ECR Backend Repository**: https://console.aws.amazon.com/ecr/repositories/private/639850426965/backend
- **EKS Cluster**: `my-cluster` in `us-east-1`
- **Region**: us-east-1 (N. Virginia)

### Application URLs
- **Frontend**: http://aab205cf6bcf442019b39ed063809cc3-1168313768.us-east-1.elb.amazonaws.com
- **Backend API (Movies)**: http://aa9aa9f85052c445481fb7413c8b62a4-338532416.us-east-1.elb.amazonaws.com/movies

## 🛠️ Local Development

### Frontend
```bash
cd frontend
npm install
npm start           # Development server
npm test            # Run tests
npm run lint        # Check code quality
npm run build       # Production build
```

### Backend
```bash
cd backend
pip install pipenv
pipenv install --dev
pipenv run serve    # Development server
pipenv run test     # Run tests
pipenv run lint     # Check code quality
```

## 🐳 Docker

### Build Images Locally

**Frontend:**
```bash
cd frontend
docker build --build-arg REACT_APP_MOVIE_API_URL=http://aa9aa9f85052c445481fb7413c8b62a4-338532416.us-east-1.elb.amazonaws.com -t frontend:latest .
docker run -p 3000:3000 frontend:latest
```

**Backend:**
```bash
cd backend
docker build -t backend:latest .
docker run -p 5000:5000 backend:latest
```

## ☸️ Kubernetes Deployment

### View Deployments
```bash
kubectl get deployments -n default
kubectl get pods -n default
kubectl get svc -n default
```

### Check Service Status
```bash
kubectl get svc -A
```

Output:
```
NAMESPACE     NAME       TYPE           EXTERNAL-IP
default       backend    LoadBalancer   aa9aa9f85052c445481fb7413c8b62a4-338532416.us-east-1.elb.amazonaws.com
default       frontend   LoadBalancer   aab205cf6bcf442019b39ed063809cc3-1168313768.us-east-1.elb.amazonaws.com
```

### Manual Deployment (Not Recommended - Use CI/CD!)
```bash
# Frontend
cd frontend/k8s
kustomize build | kubectl apply -f -

# Backend
cd backend/k8s
kustomize build | kubectl apply -f -
```

## 🔐 GitHub Secrets Configuration

The following secrets are configured in the repository:

| Secret Name | Description |
|------------|-------------|
| `AWS_ACCESS_KEY_ID` | AWS access key for ECR and EKS |
| `AWS_SECRET_ACCESS_KEY` | AWS secret access key |
| `AWS_SESSION_TOKEN` | AWS session token (temporary credentials) |
| `BACKEND_URL` | Backend LoadBalancer URL (for frontend build) |

**Configure at:** https://github.com/Hzzxjj/prj4-upd/settings/secrets/actions

## 📊 Monitoring & Verification

### Check Workflow Status
```bash
# View all workflows
https://github.com/Hzzxjj/prj4-upd/actions

# Individual workflow pages
https://github.com/Hzzxjj/prj4-upd/actions/workflows/frontend-ci.yaml
https://github.com/Hzzxjj/prj4-upd/actions/workflows/backend-ci.yaml
https://github.com/Hzzxjj/prj4-upd/actions/workflows/frontend-cd.yaml
https://github.com/Hzzxjj/prj4-upd/actions/workflows/backend-cd.yaml
```

### Check Application Health
```bash
# Frontend health
curl http://aab205cf6bcf442019b39ed063809cc3-1168313768.us-east-1.elb.amazonaws.com

# Backend API
curl http://aa9aa9f85052c445481fb7413c8b62a4-338532416.us-east-1.elb.amazonaws.com/movies
```

### Check ECR Images
```bash
# List frontend images
aws ecr list-images --repository-name frontend --region us-east-1

# List backend images
aws ecr list-images --repository-name backend --region us-east-1
```

## 🚀 Deployment Process

### Typical Workflow

1. **Developer creates feature branch**
   ```bash
   git checkout -b feature/new-feature
   # Make changes
   git add .
   git commit -m "feat: Add new feature"
   git push origin feature/new-feature
   ```

2. **Create Pull Request**
   - Go to https://github.com/Hzzxjj/prj4-upd/pulls
   - Create PR from feature branch to main
   - **CI workflows automatically run**
   - Lint, test, and build jobs must pass

3. **Review & Merge**
   - Wait for all CI checks to pass ✅
   - Review code changes
   - Merge pull request

4. **Automatic Deployment**
   - **CD workflows automatically trigger**
   - Build Docker images
   - Push to ECR
   - Deploy to EKS
   - Application updates automatically!

## 🎯 Key Features

✅ **Automated CI/CD** - No manual deployments needed  
✅ **Parallel Testing** - Lint and test jobs run simultaneously  
✅ **Security** - AWS credentials stored as GitHub Secrets  
✅ **Docker Optimization** - Multi-stage builds for smaller images  
✅ **Environment Variables** - Backend URL injected at build time  
✅ **Kustomize** - Clean Kubernetes deployments  
✅ **LoadBalancer** - External access to applications  
✅ **Zero Downtime** - Rolling updates in Kubernetes  
✅ **Caching** - npm and pipenv dependencies cached for faster builds  
✅ **Manual Triggers** - All workflows can be manually triggered  

## 📚 Additional Documentation

- **Quick Start Guide**: [QUICK_START.md](QUICK_START.md)
- **Screenshot Guide**: [SCREENSHOT_GUIDE.md](SCREENSHOT_GUIDE.md)
- **Project Summary**: [PROJECT_SUMMARY.md](PROJECT_SUMMARY.md)

## 🔧 Troubleshooting

### Workflow Failures

**Check logs:**
1. Go to https://github.com/Hzzxjj/prj4-upd/actions
2. Click on the failed workflow
3. Click on the failed job
4. Review error messages

**Common issues:**
- AWS credentials expired → Update GitHub Secrets
- Tests failing → Fix code and push again
- Docker build errors → Check Dockerfile syntax
- kubectl errors → Verify EKS cluster access

### Application Not Loading

**Check services:**
```bash
kubectl get svc -n default
```

**Check pods:**
```bash
kubectl get pods -n default
kubectl logs <pod-name>
```

**Check deployments:**
```bash
kubectl describe deployment frontend
kubectl describe deployment backend
```

## 🏆 Success Metrics

All criteria met:
- ✅ 4 workflow files in `.github/workflows/`
- ✅ All workflows passing with green checkmarks
- ✅ CI runs on pull requests automatically
- ✅ CD runs on push to main automatically
- ✅ Lint and test jobs run in parallel
- ✅ Build job runs after lint/test complete
- ✅ Docker images pushed to ECR
- ✅ Applications deployed to EKS
- ✅ No AWS credentials in code
- ✅ Frontend can fetch data from backend
- ✅ Applications publicly accessible

## 📝 License

MIT License

## 👤 Author

**Hzzxjj**
- GitHub: [@Hzzxjj](https://github.com/Hzzxjj)
- Repository: [prj4-upd](https://github.com/Hzzxjj/prj4-upd)

---

**Last Updated:** October 8, 2025  
**Status:** ✅ All systems operational  
**Cluster:** my-cluster (us-east-1)  
**Registry:** 639850426965.dkr.ecr.us-east-1.amazonaws.com
