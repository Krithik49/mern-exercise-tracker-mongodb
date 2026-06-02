# MERN Exercise Tracker — Production Deployment on AKS

A production-grade 3-tier MERN application deployed on Azure Kubernetes Service (AKS) using a complete GitOps pipeline.

## Architecture

- **Frontend** — React.js
- **Backend** — Node.js + Express
- **Database** — MongoDB

## DevOps Stack

| Tool | Purpose |
|------|---------|
| Terraform | Provision AKS cluster and ACR on Azure |
| Docker | Containerize frontend and backend |
| GitHub Actions | CI pipeline — build and push images to ACR |
| Helm | Package and deploy app to Kubernetes |
| ArgoCD | GitOps CD — auto deploy on every push |
| Prometheus | Collect metrics from pods |
| Grafana | Dashboards and alerting |
| HPA | Auto scale backend pods based on CPU |

## Pipeline Flow
Code push to GitHub
↓
GitHub Actions builds Docker images
↓
Images pushed to Azure Container Registry (ACR)
↓
ArgoCD detects change in Helm chart
↓
Auto deploys to AKS cluster
↓
HPA scales pods based on traffic
↓
Prometheus + Grafana monitors everything

## Infrastructure

- AKS Cluster with 2 worker nodes
- Azure Container Registry (ACR)
- Resource Group — all managed by Terraform

## How to Deploy

**Step 1 — Provision infrastructure:**
```bash
cd terraform
terraform init
terraform apply
```

**Step 2 — Connect kubectl:**
```bash
az aks get-credentials --resource-group mern-project-rg --name mern-aks-cluster
```

**Step 3 — Install ArgoCD:**
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl apply -f argocd/application.yaml
```

**Step 4 — App is live!**
```bash
kubectl get svc frontend-service
```

## Monitoring

- Prometheus scrapes metrics every 15 seconds
- Grafana dashboards show CPU, memory, network per pod
- Alert fires when pod restarts more than 3 times

## Auto Scaling

HPA configured on backend deployment:
- Min replicas: 2
- Max replicas: 5
- Scale trigger: CPU above 50%
