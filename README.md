# FastAPI Azure Container Deployment with GitHub Actions CI/CD

[Read the full deployment guide here](https://dev.to/dhayv/how-to-deploy-a-fastapi-app-to-azure-with-docker-acr-and-github-actions-30bk)

![Azure](https://img.shields.io/badge/Built%20with-Azure-blue)
![Docker](https://img.shields.io/badge/Containerized-Docker-blue)
![GitHub Actions](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-blue)

## Overview

This repository contains the full configuration and deployment steps for containerizing a FastAPI application, pushing it to Azure Container Registry (ACR), deploying it on Azure App Service (Web App for Containers), and automating deployments using GitHub Actions CI/CD.

The project demonstrates cloud-native deployment practices focused on automation, security, and scalability, without relying on manual Docker Hub uploads or direct portal deployments.

---

## Prerequisites

- Azure CLI installed and configured
- GitHub account with a repository for your FastAPI app
- Basic understanding of Docker and container concepts
- Basic familiarity with Azure services (Container Registry, App Service)
- Git installed
- Docker installed

---

## Repository Structure

``bash
Repository Structure: 
Repository Structure:
├── .github/ │ └── workflows/ │ └── azure-webapp.yml # GitHub Actions CI/CD workflow
├── main.py                # FastAPI application
├── Dockerfile             # Docker configuration
└── requirements.txt       # Python dependencies

```

---

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/hayvid/azure-cicd.git
cd azure-fastapi-ci-cd
```

2. Configure Azure CLI

``` bash
az login
az account set --subscription "your-subscription-id"
3. Build and Push Docker Image to Azure Container Registry (ACR)
```

```bash
az acr login --name your-acr-name
docker build -t your-acr-name.azurecr.io/fastapi-demo:latest .
docker push your-acr-name.azurecr.io/fastapi-demo:latest
```

4. Deploy Web App for Containers
- Create an App Service Plan (Linux) through Azure Portal or CLI.
- Create a Web App linked to your ACR.
- Configure container settings to point to your pushed image.

5. Set Up GitHub Actions CI/CD
- Configure Deployment Center on your Web App to connect GitHub repo.
- Customize the GitHub Actions workflow (azure-webapp.yml) for secure ACR login and automated deployments.
- Add necessary GitHub Secrets (AZURE_CREDENTIALS, REGISTRY_LOGIN_SERVER, etc.)

## Technical Details

### Azure Container Registry (ACR)

- Stores Docker images securely in Azure.
- Managed through Azure CLI and GitHub Actions.

## Azure App Service (Web App for Containers)
- Hosts the FastAPI container.
- Linked directly to ACR for seamless deployment.
- Configured for Linux-based container hosting.

## GitHub Actions CI/CD Pipeline
- Triggers on pushes to the main branch.
- Builds and pushes Docker image to ACR.
- Deploys updated containers automatically to App Service.
- Secured with Service Principal authentication.

## Components

### FastAPI Application (app/main.py)
- RESTful API endpoints
- Dockerized configuration for container compatibility

### Docker Configuration (Dockerfile)
- Base Python image
- Installs FastAPI and Uvicorn server
- Copies app source files and sets entry point

### GitHub Actions Workflow (.github/workflows/azure-webapp.yml)
- Checks out code
- Builds Docker image
- Pushes image to ACR
- Deploys new image to Azure App Service

## Important Notes

- **Security**: ACR authentication is handled via Azure Service Principals and GitHub Secrets.
- **CI/CD**: GitHub Actions fully automates container builds and deployments.
- **Cost**: Azure App Service Free Tier (F1) used for demonstration purposes; production should use Standard or higher tiers.
- **Scaling**: App Service supports scaling out containers for production loads.
- **Versioning**: Explicit Docker tags are recommended instead of :latest for production setups.

## Resource Cleanup

To avoid unnecessary charges, clean up your resources when done:

```bash
# Delete Resource Group and all contained resources
az group delete --name your-resource-group-name --yes --no-wait
```
Or manually delete App Service, ACR, and related resources via the Azure Portal.
