<div align="center">
  <br />
      <img src="https://miro.medium.com/v2/resize:fit:640/format:webp/0*Ej9hveRz7ou4nYLS.png" alt="Argo and K8s Banner">
  <br />

  <h3 align="center">Zoom Clone K8s and Argo</h3>
</div>

## <a name="overview">📋 Overview</a>

This repository is part of a collection of three repositories that follow GitOps principles to deploy a Zoom clone application to an AWS EKS cluster.

The other repositories in this collection are:
- [Application code repository](https://github.com/shadyosama9/Zoom-Clone-App.git)
- [Infrastructure as Code (IaC) repository](https://github.com/shadyosama9/Zoom-Clone-Infra)

This repository contains Kubernetes definitions for deploying application files along with the ArgoCD application file for creating an ArgoCD application.

### Included Files

- **zoom-deploy.yml**: Defines the deployment specifications for the application.
- **zoom-service.yml**: Configures the service settings to expose the application.
- **zoom-ingress.yml**: Sets up ingress rules for managing external access to the application.
- **argo-ingress.yml**: Contains the ingress configurations specifically for ArgoCD.
