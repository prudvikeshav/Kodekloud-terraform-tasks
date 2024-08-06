
## **Terraform Challenge Series 1: Kubernetes Resource Deployment**

In this challenge, you will use Terraform to define and deploy Kubernetes resources. Follow the detailed instructions below to complete the task.

### **1. Verify Terraform Installation**

Confirm that Terraform version 1.1.5 is installed on the control plane.

### **2. Configure Provider Settings**

Set up Terraform to interact with your Kubernetes cluster by creating a configuration file named `provider.tf` in the `/root/terraform_challenge` directory. This file should:

- Specify the use of the `hashicorp/kubernetes` provider.
- Assign the local name `kubernetes` to this provider.
- Set the provider version to `2.11.0`.
- Configure the provider to use the kubeconfig file located at `/root/.kube/config` for connecting to your Kubernetes cluster.

### **3. Define the Kubernetes Service**

Create a Terraform resource to define a Kubernetes service within a file named `service.tf`. This service will be of type `NodePort`, making it accessible externally on a specific port. The service should have the following specifications:

- **Service Name:** `webapp-service`
- **Service Type:** `NodePort`
- **Port:** `8080` (the port exposed by the service)
- **NodePort:** `30080` (the port on each node that routes traffic to the service)

### **4. Define the Kubernetes Deployment**

Create another Terraform resource for a Kubernetes deployment in a file named `deployment.tf`. This deployment will manage the lifecycle of your application pods. Configure it with the following details:

- **Deployment Name:** `frontend`
- **Deployment Labels:** `name: frontend`
- **Replicas:** `4` (number of pod replicas to be maintained)
- **Pod Labels:** `name: webapp`
- **Container Image:** `kodekloud/webapp-color:v1`
- **Container Name:** `simple-webapp`
- **Container Port:** `8080` (the port on which the container listens)

Ensure that your configurations reflect these specifications to correctly deploy and manage the resources in your Kubernetes cluster.

## **Solution**

### **1. Verify Terraform Installation**

To start, we needed to verify whether Terraform was installed on the control plane. We initially checked for the presence of Terraform by running the command:

```bash
terraform
bash:terraform: command not found
```

The output indicated that Terraform was not installed. To resolve this, we proceeded with the following steps:

1. **Update the Package List:** We updated the package list to ensure we had the latest information on available packages:

   ```bash
   apt update
   ```

2. **Install Unzip:** Since Terraform is distributed as a ZIP archive, we installed the `unzip` utility to extract the Terraform binary:

   ```bash
   apt install unzip -y
   ```

3. **Download Terraform:** We downloaded the specific version of Terraform (1.1.5) from the HashiCorp release page using `curl`:

   ```bash
   curl -L -o /tmp/terraform_1.1.5_linux_amd64.zip https://releases.hashicorp.com/terraform/1.1.5/terraform_1.1.5_linux_amd64.zip
   ```

4. **Extract and Install Terraform:** We unzipped the downloaded file and moved the Terraform binary to `/usr/local/bin`, making it available system-wide:

   ```bash
   unzip -d /usr/local/bin /tmp/terraform_1.1.5_linux_amd64.zip
   ```

5. **Verify Installation:** Finally, we confirmed the installation by checking the Terraform version:

   ```bash
   terraform -version
   ```

   Output:

   ```
   Terraform v1.1.5
   on linux_amd64
   ```

   This confirmed that Terraform v1.1.5 was successfully installed.

### **2. Configure Provider Settings**

To enable Terraform to interact with our Kubernetes cluster, we created a `provider.tf` file in the `/root/terraform_challenge` directory with the following configuration:

```hcl
terraform {
  required_providers {
    kubernetes = {
      source  = "hashicorp/kubernetes"
      version = "2.11.0"
    }
  }
}

provider "kubernetes" {
  config_path = "/root/.kube/config"
}
```

This configuration:

- **Specifies the Provider:** We used the `hashicorp/kubernetes` provider with version `2.11.0`.
- **Configures the Provider:** We set the `config_path` to point to the kubeconfig file located at `/root/.kube/config`, which Terraform uses to connect to the Kubernetes cluster.

### **3. Define the Kubernetes Service**

Next, we defined a Kubernetes service in the `service.tf` file:

```hcl
resource "kubernetes_service" "webapp-service" {
  metadata {
    name = "webapp-service"
  }

  spec {
    selector = {
      name = kubernetes_deployment.frontend.metadata[0].labels.name
    }
    port {
      port      = 8080
      node_port = 30080
    }
    type = "NodePort"
  }
}
```

In this configuration:

- **Service Name:** The service is named `webapp-service`.
- **Service Type:** We set the service type to `NodePort`, which allows the service to be accessed externally on a specific port.
- **Port Configuration:** The service listens on port `8080` and exposes it on port `30080` on each node.
- **Selector:** The service uses the label selector to match the pods created by the `frontend` deployment.

### **4. Define the Kubernetes Deployment**

We created a `deployment.tf` file to define the Kubernetes deployment:

```hcl
resource "kubernetes_deployment" "frontend" {
  metadata {
    name = "frontend"
    labels = {
      name = "webapp"
      name = "frontend"
    }
  }

  spec {
    replicas = 4

    selector {
      match_labels = {
        name = "webapp"
      }
    }

    template {
      metadata {
        labels = {
          name = "webapp"
        }
      }

      spec {
        container {
          image = "kodekloud/webapp-color:v1"
          name  = "simple-webapp"
          port {
            container_port = 8080
          }
        }
      }
    }
  }
}
```

In this configuration:

- **Deployment Name:** The deployment is named `frontend`.
- **Labels:** The deployment uses labels to categorize its resources.
- **Replicas:** We set the deployment to manage 4 replicas to ensure high availability.
- **Pod Template:** Each pod created by this deployment will use the `kodekloud/webapp-color:v1` image and listen on port `8080`.

### **5. Initialize, Validate, and Apply the Configuration**

With the configurations in place, we executed the following Terraform commands:

1. **Initialize Terraform:** We initialized the Terraform working directory, which downloaded the necessary provider plugins and set up the environment:

   ```bash
   terraform init
   ```

2. **Validate Configuration:** We validated the configuration files to ensure they were syntactically correct and free of errors:

   ```bash
   terraform validate
   ```

3. **Plan the Deployment:** We generated an execution plan to review the proposed changes before applying them:

   ```bash
   terraform plan
   ```

4. **Apply the Configuration:** We applied the configuration to create the Kubernetes deployment and service:

   ```bash
   terraform apply
   ```

   We confirmed the deployment by entering `yes` when prompted to proceed with the actions.

5. **Verify the Deployment:** After applying the configuration, we used `kubectl` commands to verify the resources:

   - **List Deployments:** We confirmed that the `frontend` deployment was created with 4 replicas:

     ```bash
     kubectl get deployments.apps
     ```

     Expected Output:

     ```
     NAME       READY   UP-TO-DATE   AVAILABLE   AGE
     frontend   4/4     4            4           3m28s
     ```

   - **List Services:** We verified that the `webapp-service` was created as a `NodePort` service and was mapped correctly:

     ```bash
     kubectl get service
     ```

     Expected Output:

      ```
      NAME             TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
      kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP          35m
      webapp-service   NodePort    10.101.182.159   <none>        8080:30080/TCP   2m54s
      ```

    - **List Pods:** We checked the status of the pods to ensure they were running as expected:

      ```bash
      kubectl get pods
      ```

      Expected Output:

      ```
      NAME                        READY   STATUS    RESTARTS   AGE
      frontend-6ff676678c-45pth   1/1     Running   0          4m17s
      frontend-6ff676678c-4sl92   1/1     Running   0          4m17s
      frontend-6ff676678c-h8rzr   1/1     Running   0          4m17s
      frontend-6ff676678c-m2k94   1/1     Running   0          4m17s
      ```

This comprehensive deployment process confirms that Terraform was successfully used to manage and deploy Kubernetes resources, ensuring that the `frontend` deployment and `webapp-service` were created and configured correctly.
