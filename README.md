# Movie Picture Pipeline

This project implements a complete CI/CD pipeline for a full-stack movie application with a React frontend and Python Flask backend, deployed to AWS EKS using GitHub Actions.

## Project Structure

```
.
├── .github/
│   └── workflows/
│       ├── frontend-ci.yaml     # Frontend CI pipeline
│       ├── backend-ci.yaml      # Backend CI pipeline
│       ├── frontend-cd.yaml     # Frontend CD pipeline
│       └── backend-cd.yaml      # Backend CD pipeline
├── frontend/                     # React application
│   ├── src/
│   ├── k8s/                     # Kubernetes manifests
│   ├── Dockerfile
│   └── package.json
├── backend/                      # Flask application
│   ├── movies/
│   ├── k8s/                     # Kubernetes manifests
│   ├── Dockerfile
│   └── Pipfile
└── README.md
```

## CI/CD Workflows

### 1. Frontend CI (frontend-ci.yaml)
- **Triggers**: Pull requests to main, manual dispatch
- **Jobs**:
  - **Lint**: Runs ESLint on the codebase
  - **Test**: Runs unit tests with coverage
  - **Build**: Builds Docker image (runs after lint and test pass)

### 2. Backend CI (backend-ci.yaml)
- **Triggers**: Pull requests to main, manual dispatch
- **Jobs**:
  - **Lint**: Runs flake8 linting
  - **Test**: Runs pytest tests
  - **Build**: Builds Docker image (runs after lint and test pass)

### 3. Frontend CD (frontend-cd.yaml)
- **Triggers**: Push to main branch, manual dispatch
- **Jobs**:
  - **Lint**: Runs ESLint
  - **Test**: Runs unit tests
  - **Build**: 
    - Builds Docker image with REACT_APP_MOVIE_API_URL environment variable
    - Pushes to Amazon ECR
    - Deploys to EKS cluster using kubectl and kustomize

### 4. Backend CD (backend-cd.yaml)
- **Triggers**: Push to main branch, manual dispatch
- **Jobs**:
  - **Lint**: Runs flake8
  - **Test**: Runs pytest
  - **Build**: 
    - Builds Docker image
    - Pushes to Amazon ECR
    - Deploys to EKS cluster using kubectl and kustomize

## Setup Instructions

### Prerequisites
1. AWS Account with EKS cluster named `cluster`
2. GitHub repository
3. ECR repositories created for `frontend` and `backend`

### Step 1: Set Up Infrastructure

If you haven't already, use the terraform scripts to set up your AWS infrastructure:

```bash
cd setup/terraform
terraform init
terraform apply
```

This will create:
- EKS cluster
- ECR repositories
- Required IAM roles and policies

### Step 2: Configure GitHub Secrets

Go to your GitHub repository → Settings → Secrets and variables → Actions

Add the following secrets:

| Secret Name | Description | Example Value |
|------------|-------------|---------------|
| `AWS_ACCESS_KEY_ID` | AWS Access Key ID | ASIAZJ6QOWJK... |
| `AWS_SECRET_ACCESS_KEY` | AWS Secret Access Key | c9Xv/SzWAFbT... |
| `AWS_SESSION_TOKEN` | AWS Session Token (if using temporary credentials) | IQoJb3JpZ2lu... |
| `BACKEND_URL` | Backend service URL | a1234567890.us-east-1.elb.amazonaws.com |

**⚠️ IMPORTANT**: 
- NEVER commit AWS credentials to your repository
- Credentials shown in this README are examples only
- Use GitHub Secrets to store all sensitive information

### Step 3: Get Backend URL

After deploying the backend for the first time (or manually deploying it once), get the LoadBalancer URL:

```bash
kubectl get svc -n default
```

Look for the backend service's EXTERNAL-IP and add it as the `BACKEND_URL` secret in GitHub.

### Step 4: Create ECR Repositories

Create two ECR repositories in AWS:

```bash
aws ecr create-repository --repository-name frontend --region us-east-1
aws ecr create-repository --repository-name backend --region us-east-1
```

### Step 5: Push Code to GitHub

```bash
git add .
git commit -m "Initial commit with CI/CD pipelines"
git push origin main
```

### Step 6: Create a Pull Request

To test the CI pipelines:

1. Create a new branch:
   ```bash
   git checkout -b feature/test-ci
   ```

2. Make a small change (e.g., update README)

3. Push and create a pull request:
   ```bash
   git push origin feature/test-ci
   ```

4. Go to GitHub and create a PR to main branch

5. Watch the CI workflows run automatically

### Step 7: Merge to Deploy

Once the CI checks pass:
1. Merge the pull request
2. The CD workflows will automatically deploy to EKS

## Verification

### Check Workflows
Go to your GitHub repository → Actions to see all workflow runs.

### Check Services
```bash
# Get all services
kubectl get svc -A

# Check deployments
kubectl get deployments -n default

# Check pods
kubectl get pods -n default
```

### Access Applications

**Frontend**: 
```bash
kubectl get svc frontend -n default
# Access the EXTERNAL-IP in your browser
```

**Backend API**:
```bash
kubectl get svc backend -n default
# Access http://EXTERNAL-IP/movies to see the movie list
```

## Required Screenshots for Submission

To successfully submit this project, you need to provide the following screenshots:

### 1. GitHub Actions - All Workflows Successful ✅
- Navigate to your GitHub repository → Actions
- Take a screenshot showing ALL 4 workflows with successful (green checkmark) runs:
  - Frontend Continuous Integration
  - Backend Continuous Integration
  - Frontend Continuous Deployment
  - Backend Continuous Deployment

**Example**: Show the Actions page with the workflow list

### 2. Frontend CI Workflow Details ✅
- Click on a successful "Frontend Continuous Integration" workflow run
- Take a screenshot showing all jobs (lint, test, build) passed
- Show the detailed steps with green checkmarks

### 3. Backend CI Workflow Details ✅
- Click on a successful "Backend Continuous Integration" workflow run
- Take a screenshot showing all jobs (lint, test, build) passed

### 4. Frontend CD Workflow Details ✅
- Click on a successful "Frontend Continuous Deployment" workflow run
- Take a screenshot showing:
  - Lint and test jobs passed
  - Build job with ECR push successful
  - Deployment to EKS successful

### 5. Backend CD Workflow Details ✅
- Click on a successful "Backend Continuous Deployment" workflow run
- Take a screenshot showing successful deployment

### 6. Pull Request with Checks Passed ✅
- Create a pull request
- Take a screenshot showing all CI checks passed before merge
- Example: "All checks have passed" with green checkmarks

### 7. ECR Repositories with Images ✅
- Go to AWS Console → ECR
- Take screenshots showing:
  - Frontend repository with pushed images (tagged with git sha and latest)
  - Backend repository with pushed images

### 8. EKS Services Running ✅
- Run: `kubectl get svc -A`
- Take a screenshot showing:
  - frontend service with LoadBalancer and EXTERNAL-IP
  - backend service with LoadBalancer and EXTERNAL-IP

### 9. Frontend Application Working ✅
- Open the frontend URL (from LoadBalancer)
- Take a screenshot showing:
  - The movie list displayed
  - The URL in the browser address bar matching the LoadBalancer URL

### 10. Backend API Working ✅
- Access http://BACKEND_URL/movies
- Take a screenshot showing:
  - The JSON response with movie list
  - The URL in the browser address bar

### 11. kubectl get svc -A Output ✅
- Run: `kubectl get svc -A`
- Take a screenshot or copy the output
- This verifies the URLs match between your screenshots and actual deployments

## Troubleshooting

### Workflow Failures

**If lint fails**:
- Run `npm run lint` (frontend) or `pipenv run lint` (backend) locally
- Fix all linting errors before pushing

**If tests fail**:
- Run tests locally: `npm test` (frontend) or `pipenv run test` (backend)
- Ensure all tests pass before creating PR

**If Docker build fails**:
- Build locally to debug: `docker build -t test .`
- Check Dockerfile syntax and dependencies

**If ECR push fails**:
- Verify AWS credentials in GitHub Secrets
- Check ECR repository exists
- Ensure session token is valid (if using temporary credentials)

**If kubectl deployment fails**:
- Verify EKS cluster is running: `aws eks describe-cluster --name cluster`
- Check kubectl configuration
- Verify kustomization.yaml is correctly formatted

### AWS Session Token Expiration

Temporary AWS credentials expire. If your workflows start failing with authentication errors:

1. Generate new credentials from AWS
2. Update GitHub Secrets with new values
3. Re-run the failed workflow

## Key Features

✅ **Parallel Job Execution**: Lint and test run simultaneously for faster CI
✅ **Dependency Caching**: npm and pipenv dependencies are cached for speed
✅ **Security**: AWS credentials stored in GitHub Secrets, never in code
✅ **Docker Multi-stage Builds**: Optimized images
✅ **Environment Variables**: Backend URL passed via build args
✅ **Kustomize**: Clean Kubernetes deployments with image tag management
✅ **Automated Deployment**: Push to main = automatic deployment
✅ **Manual Triggers**: All workflows can be manually triggered via workflow_dispatch

## Important Notes

1. **Never interact with kubectl manually** after setting up CI/CD - all deployments must go through GitHub Actions
2. **Always create pull requests** for changes - direct pushes to main will trigger CD
3. **AWS credentials expire** - update them in GitHub Secrets when needed
4. **Test locally first** - run lint and tests before pushing to avoid workflow failures
5. **Monitor costs** - EKS and LoadBalancers incur AWS charges

## Success Criteria

✅ 4 workflow files present in `.github/workflows/`
✅ All workflows have successful runs
✅ CI workflows run on pull requests
✅ CD workflows run on push to main
✅ Docker images pushed to ECR
✅ Applications deployed and accessible on EKS
✅ Frontend can fetch movies from backend
✅ No AWS credentials in code
✅ All tests passing

## License

MIT License
