# Multi-Cloud-Kubernetes-Federation-Deployment

# Multi-Cloud Kubernetes Federation Deployment

This project provisions Kubernetes clusters in **Azure (AKS)** and **AWS (EKS)**, configures **Consul federation with mesh gateways**, and deploys microservices across clusters using **Terraform and Helm**.

## Prerequisites
Ensure the following are installed and configured:  
- **Terraform 0.14+**  
- **AWS CLI** with credentials configured  
- **Azure CLI**  
- **kubectl**  

## Deployment Steps

### 1. Clone the Repository
```bash
git clone https://github.com/Multi-Cloud-Kubernetes-Federation-Deployment.git
cd Multi-Cloud-Kubernetes-Federation-Deployment
'''

### 2. Provision an EKS Cluster (AWS)
'''bash
cd eks
terraform init
terraform apply
'''
#### Deploys an EKS cluster with worker nodes and VPC networking.
#### Outputs authentication details for Kubernetes access.

### 3. Provision an AKS Cluster (Azure)
'''bash
cd ../aks
az login
'''
#### Create an Active Directory service principal for authentication.
#### Rename and update terraform.tfvars with credentials.

### Initialize and apply Terraform:
'''bash
terraform init
terraform apply
'''

### 4. Deploy Consul Federation
'''bash
cd ../consul
terraform init
terraform apply
'''

#### Uses terraform_remote_state to retrieve AKS & EKS cluster details.
#### Configures Consul mesh gateways for federated networking.

### 5. Configure kubectl
'''bash
aws eks update-kubeconfig --name <EKS_CLUSTER_NAME>
az aks get-credentials --name <AKS_CLUSTER_NAME> --resource-group <RESOURCE_GROUP>
'''

### 6. Deploy Consul & Proxy Defaults
'''bash
kubectl apply -f <consul-config.yaml>
'''
#### Deploys primary Consul datacenter on EKS.
####Loads federation secret into AKS.
####Deploys secondary Consul cluster.

### 7. Verify Cluster Federation
'''bash
kubectl get pods --all-namespaces
Ensures federated connectivity across clusters.
'''
### 8. Deploy Microservices Application
'''bash
cd ../counting-service
kubectl apply -f <app-deployment.yaml>
'''
#### Deploys a multi-cluster microservice across EKS and AKS.

## Conclusion
#### This project demonstrates multi-cloud Kubernetes federation, enabling cross-cluster communication and high availability.

#### For further improvements, consider Istio for service mesh and CI/CD automation.

