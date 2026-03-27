# CST8915 Lab 6 - Deploy Algonquin Pet Store to Azure Kubernetes Service (AKS)

## Demo Video
[Click here to watch the demo](https://youtu.be/BXfiSTgjkjM)

## Overview
In this lab, I deployed the Algonquin Pet Store application to Azure Kubernetes Service (AKS). 
The app consists of 4 microservices: store-front, order-service, product-service, and RabbitMQ.

## Steps Completed
1. Installed kubectl and Azure CLI
2. Created an AKS cluster on Azure Portal with 2 node pools
3. Connected to the cluster using kubectl
4. Deployed the app using the YAML file
5. Accessed the app via the external IP address

## Kubernetes Commands Used
```bash
# Connect to cluster
az aks get-credentials --resource-group AlgonquinPetStoreRG --name AlgonquinPetStoreCluster

# Deploy the app
kubectl apply -f algonquin-pet-store-all-in-one.yaml

# Check pods
kubectl get pods

# Check services
kubectl get services

# Check nodes
kubectl get nodes

# Clean up
kubectl delete -f algonquin-pet-store-all-in-one.yaml
```

## Live App URLs
- Store Front: http://20.125.71.229
- Products: http://20.125.71.229/products
- Orders: http://20.125.71.229/orders
- RabbitMQ Dashboard: http://20.125.71.229/rabbitmq/

## How Multi-Path Routing Works
All services are accessible via the same IP address using different URL paths.
This is made possible by **nginx** acting as a reverse proxy inside the store-front container.
The `nginx.conf` file routes:
- `/products` → product-service:3030
- `/orders` → order-service:3000
- `/rabbitmq/` → rabbitmq:15672

## Cleanup
After completing the lab, all Azure resources were deleted to avoid extra charges:
- Deleted AlgonquinPetStoreRG resource group
- Deleted MC_AlgonquinPetStoreRG_AlgonquinPetStoreCluster_canadacentral
- Deleted NetworkWatcherRG

## Notes
- Used a custom built store-front-l6 image with nginx reverse proxy configuration
- Two node pools were required: one system node and one worker node
- Student Azure subscription required smaller VM sizes (D2as_v4)
