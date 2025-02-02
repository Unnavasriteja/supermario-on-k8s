
# **Deploying Super Mario on Kubernetes (AWS EKS)**

Super Mario is a timeless classic game cherished by many. This guide demonstrates how to deploy the Super Mario game on Amazon Elastic Kubernetes Service (EKS) using Kubernetes. By leveraging Kubernetes, we ensure scalability, reliability, and easy management.

## **Table of Contents**
1. [Prerequisites](#prerequisites)
2. [Project Overview](#project-overview)
3. [Deployment Steps](#deployment-steps)
    - [Step 1: Launch Ubuntu Instance](#step-1-launch-ubuntu-instance)
    - [Step 2: Provision the EKS Cluster](#step-2-provision-the-eks-cluster)
    - [Step 3: Deploy Kubernetes Resources](#step-3-deploy-kubernetes-resources)
4. [Access the Super Mario Game](#access-the-super-mario-game)
5. [Cleanup](#cleanup)
6. [Notes and Best Practices](#notes-and-best-practices)

---

## **Prerequisites**
Before proceeding, ensure you have the following:
- **AWS Account** with sufficient privileges.
- **IAM Role** for your EC2 instance with `Administrator Access` permissions (for learning purposes only).
- A working knowledge of AWS, Terraform, Kubernetes, and Docker.
- **Ubuntu Server (t2.micro)** instance launched in AWS.

---

## **Project Overview**
This project deploys a Super Mario game application as a Kubernetes workload on AWS EKS. Terraform automates the provisioning of the EKS cluster, while Kubernetes manifests handle the deployment and service configuration.

### Key Tools:
- **Terraform**: For EKS cluster provisioning.
- **Kubernetes**: For workload orchestration.
- **Docker**: To fetch the Super Mario game image from Docker Hub (`nirmalnaveen/supermario`).

---

## **Deployment Steps**

### **Step 1: Launch Ubuntu Instance**
1. Launch an Ubuntu server (t2.micro) in AWS.
2. Assign an IAM role with **Administrator Access** to the instance (for learning purposes).
3. SSH into the Ubuntu server.

---

### **Step 2: Provision the EKS Cluster**
1. Clone the project repository:
   ```bash
   git clone https://github.com/Unnavasriteja/supermario-k8s.git
   cd supermario-k8s
   ```

2. Provide executable permissions to the setup script and execute it:
   ```bash
   sudo chmod +x script.sh
   ./script.sh
   ```

   The script installs **Terraform**, **AWS CLI**, **kubectl**, and **Docker**. Verify installations:
   ```bash
   docker --version
   aws --version
   kubectl version --client
   terraform --version
   ```

3. Navigate to the Terraform directory:
   ```bash
   cd EKS-TF
   ```

4. Update the `backend.tf` file with a unique S3 bucket name for the Terraform state.

5. Initialize Terraform:
   ```bash
   terraform init
   ```

6. Validate and plan the infrastructure:
   ```bash
   terraform validate
   terraform plan
   ```

7. Apply the configuration to create the EKS cluster:
   ```bash
   terraform apply --auto-approve
   ```

---

### **Step 3: Deploy Kubernetes Resources**
1. Update your Kubernetes configuration:
   ```bash
   aws eks update-kubeconfig --name EKS_CLOUD --region us-east-1
   ```

2. Deploy the application:
   - Apply the deployment configuration:
     ```bash
     kubectl apply -f deployment.yaml
     ```
   - Verify the deployment:
     ```bash
     kubectl get all
     ```

3. Expose the application via a service:
   ```bash
   kubectl apply -f service.yaml
   kubectl get all
   ```

4. Obtain the LoadBalancer Ingress URL:
   ```bash
   kubectl describe service mario-service
   ```
   Copy the `LoadBalancer Ingress` URL and paste it into your browser to play the game!

---

## **Access the Super Mario Game**
Once the LoadBalancer Ingress URL is available, access the game by visiting the URL in your browser. Enjoy the nostalgia as you play the iconic game!

---

## **Cleanup**
To avoid incurring unnecessary AWS charges, clean up resources after testing:

1. Delete the Kubernetes service and deployment:
   ```bash
   kubectl delete service mario-service
   kubectl delete deployment mario-deployment
   ```

2. Destroy the Terraform-managed infrastructure:
   ```bash
   terraform destroy --auto-approve
   ```

3. Stop or terminate any unused EC2 instances.

---

## **Notes and Best Practices**
- **IAM Role**: Avoid assigning excessive permissions to your EC2 instance in production environments.
- **Costs**: Monitor AWS resources to avoid unexpected charges.
- **Terraform Backend**: Use unique S3 bucket names for state files.
- **Security**: Avoid exposing your EKS cluster or Kubernetes services unnecessarily.

---

## **Contributions**
Contributions are welcome! Feel free to open an issue or submit a pull request if you’d like to improve this project.

---

## **License**
This project is open-source and available under the MIT License.


