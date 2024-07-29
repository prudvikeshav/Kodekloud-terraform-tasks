# **Welcome to the terraform challenge series 1**

In this challenge we will deploy several Kubernetes resources using terraform.

- Utilize /root/terraform_challenge directory to store your Terraform configuration files.

1. Terraform version: 1.1.5 installed on controlplane?

2. Configure terraform and provider settings within provider.tf file with following specifications:
  
   - Configure terraform to use hashicorp/kubernetes provider.

   - Specify the provider's local name: kubernetes

   - Provider version: 2.11.0

   - Configure kubernetes provider with path to your kubeconfig file: /root/.kube/config

3. Create a terraform resource webapp-service for kubernetes service with following specs:

   - Service name: webapp-service

   - Service Type: NodePort

   - Port: 8080

   - NodePort: 30080

4. Create a terraform resource frontend for kubernetes deployment with following specs:

   - Deployment Name: frontend

   - Deployment Labels = name: frontend

   - Replicas: 4

   - Pod Labels = name: webapp

   - Image: kodekloud/webapp-color:v1

   - Container name: simple-webapp

   - Container port: 8080
