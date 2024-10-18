<div align="center">
  <br />
      <img src="https://zoomclone-images.s3.amazonaws.com/0_Ej9hveRz7ou4nYLS.webp" alt="Argo and K8s Banner">
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

---

## <a name="deploy">🚀 Application Deployment</a>

**Prerequisites**

- An AWS account
- AWS CLI installed 
- kubectl installed on your machine
- Docker installed
- DockerHub account
- Domain
- Make sure you created the infrastructer in [Infrastructure as Code (IaC) repository](https://github.com/shadyosama9/Zoom-Clone-Infra)

<br>

**Steps**
1. **DockerHub Repo**
   - Create a docker hub repository for the image

2. **Build The Docker Image**
   - Build the docker image from - [Application code repository](https://github.com/shadyosama9/Zoom-Clone-App.git)
     ```bash
     docker build -t <your-dockerhub-repo-name>:V1 <path-to-dockerfile>
     ```

3. **Upload The Image**
    - Push the image to your dockerhub repository
      ```bash
      docker push <your-image-name>

      ```
4. **Route 53 SetUp**
    - Go to your AWS account
    - search for route53 service
    - create hosted zones for the app and argocd 

5. **Creating NS Records**
    - Go to your domain register (eg. godaddy)
    - Create NS records from your route 53 hosed zones

6. **Certificate Claming**
    - To enable HTTPS with your app you can issue a certificate from AWS Certificate Manager
        - Go to your AWS acoount
        - Search for certificate manager
        - Issue a certificate for your domain with the format `*.<your-domain>`
        - Go to your domain register
        - Create CNAME record for the certificate 
        - For the name copy the name provided from AWS certificate manager and remove the domain part
        - For the value copy the one provided by AWS Certificate Manager and remove the last `.`

6. **Configure AWS CLI**
   - Set up your AWS credentials and default region:
     ```bash
     aws configure
     ```

7. **Clone The Repository**
   - Clone the required repository to your local machine:
     ```bash
     git clone <repository-url>
     ```

8. **Update Kubeconfig File**
   - Get your eks cluster kubeconfig file
     ```bash
     aws eks update-kubeconfig --region <your-region> --name <your eks cluster name>
     ```

9. **Deploy The App**
   - To deploy the app go to `kuberentes/` 
   - Update the image name in `zoom-deploy.yml` with your image name
   - Update the domain name in `zoom-ingress.yml` with your hosted zone name
   - Update the certificate arn in `zoom-ingress.yml` with your cerificate arn
   - Run 
   ```bash
   kubectl apply -f .
   ```
5. **Deploy Argocd App**
   - First go to `kuberentes/`
    - Update the domain name in `argo-ingress.yml` with your hosted zone name
    - Update the certificate in `argo-ingress.yml` with your certificate arn
   - To create an argocd app go back to the root and run 
   ```bash
   kubctl apply -f application.yml
   ```


---

## <a name="access">🔑 Accessing The Application </a>

Once the app has been deployed and the load blancer has been provisioned:

- Go to route 53 on aws
- Create a record in the app hosted zone
- Select alias and choose `Alias to Application and Classic Load Balancer` in endpoint
- Select your region
- Choose your Load Balancer

Now you can access the app from the domain sepcified in `zoom-ingress.yml`

<div align="center">
  <br />
      <img src="https://zoomclone-images.s3.amazonaws.com/Zoom.webp" alt="Zoom Clone Page">
  <br />
</div>

---

## <a name="access">⚙️ Accessing ArgoCD </a>

For ArgoCD it's the same as the app but:

- Create the record in argo hosted zone if you've seprated the hosted zones 

You Access argocd from the domain specified in 

<div align="center">
  <br />
      <img src="https://zoomclone-images.s3.amazonaws.com/Argo-Login.webp" alt="Argo Application Page">
  <br />
</div>

<br>

- The username for argo is "admin"
- To get the passwords run:
```bash
kubectl get secret/argocd-initial-admin-secret -n argocd -o yaml
```
- Decode the password by running:
```bash
echo <your-password> | base64 --decode
```

<div align="center">
  <br />
      <img src="https://zoomclone-images.s3.amazonaws.com/argo-app.webp" alt="Argo Login Page">
  <br />
</div>
