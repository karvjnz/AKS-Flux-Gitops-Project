# AKS-Flux-Gitops-Project
Automated Deployment Pipeline using Docker, Azure Kubernetes Service (AKS), and Flux GitOps

## End-to-End DevOps Project: Docker + Azure AKS + Flux GitOps
## Project Title
Automated Deployment Pipeline using Docker, Azure Kubernetes Service (AKS), and Flux GitOps

## Project Summary
Designed and implemented a cloud-native, end-to-end deployment pipeline using Docker, Azure Container Registry (ACR), Azure Kubernetes Service (AKS), and Flux GitOps. 
The system enables automated, version-controlled deployments where any change pushed to GitHub is automatically reflected in the Kubernetes cluster.

## Objectives
1. Containerize an application using Docker
2. Deploy containers to Azure Kubernetes Service (AKS)
3. Implement GitOps using Flux for automated deployments
4. Achieve continuous delivery with zero manual intervention

## Architecture Overview
Developer → Docker → Azure Container Registry → GitHub → Flux → AKS → End Users

## 🧰 Tech Stack:
1. Docker
2. Azure Container Registry (ACR)
3. Azure Kubernetes Service (AKS)
4. Kubernetes (K8s)
5. Flux CD (GitOps)
6. GitHub
7. Python (Flask App)

## 📂 Project Structure

project-root/
│
├── app/                       # Application source code
│   ├── app.py                 # Main Flask application
│   ├── requirements.txt       # Python dependencies
│   └── Dockerfile             # Docker build configuration
│
├── k8s/                       # Kubernetes manifests (manual deployment)
│   ├── deployment.yaml        # Deployment configuration
│   └── service.yaml           # Service configuration (LoadBalancer)
│
└── gitops-repo/               # GitOps repository for Flux CD
    └── clusters/
        └── my-cluster/
            ├── deployment.yaml  # Deployment managed by Flux
            └── service.yaml     # Service managed by Flux

## 🐳 Step 1: Docker Setup
1. Dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY . .
RUN pip install flask
CMD ["python", "app.py"]

2. Build & Run
docker build -t myapp:v1 .
docker run -p 5000:5000 myapp:v1

## ☁️ Step 2: Push to Azure Container Registry
az acr login --name <acr-name>
docker tag myapp:v1 <acr-name>.azurecr.io/myapp:v1
docker push <acr-name>.azurecr.io/myapp:v1


## ☸️ Step 3: Deploy to AKS
1. deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: <acr-name>.azurecr.io/myapp:v1
        ports:
        - containerPort: 5000

2. service.yaml

apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
    - port: 80
      targetPort: 5000

3. Apply
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

## Step 4: GitOps with Flux
Install Flux
flux bootstrap github \
  --owner=<github-username> \
  --repo=aks-gitops \
  --branch=main \
  --path=./clusters/my-cluster \
  --personal

## Continuous Deployment Flow

1. Developer updates code
2. GitHub Actions builds Docker image
3. Image pushed to ACR
4. Update image tag in GitOps repo
5. Flux detects change
6. AKS deploys automatically

## Key Features Implemented
1. Containerized application deployment
2. Kubernetes orchestration
3. GitOps-based continuous delivery
4. Automated rolling updates
5. Scalable microservice architecture


echo "# AKS-Flux-Gitops-Project" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/karvjnz/AKS-Flux-Gitops-Project.git
git push -u origin main
