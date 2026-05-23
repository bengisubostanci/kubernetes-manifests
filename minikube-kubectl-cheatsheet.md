Minikube & Kubectl Quick Reference Guide
A curated cheat sheet for local Kubernetes development using Minikube and Kubectl.

1. Minikube Basics
Minikube requires a container or virtual machine manager. If you are using Docker as the driver, ensure the Docker service is running first.

Service Management

# Start Docker daemon (if stopped)
sudo systemctl start docker

# Start Minikube cluster
minikube start

# Start Minikube with custom resources
minikube start --cpus="2" --memory="4g"

# Stop the local cluster
minikube stop

# Delete the cluster and recover disk space
minikube delete

Cluster Interaction
# Open the Kubernetes Web Dashboard automatically
minikube dashboard

# SSH into the Minikube control plane node
minikube ssh

2. Kubectl Basics
The core syntax for interacting with resources:
kubectl <action> <object_type> <object_name>

Diagnostics & Cluster Status
# Check control plane and core services status
kubectl cluster-info

# Dump cluster state for deep debugging
kubectl cluster-info dump

# List all physical/virtual nodes in the cluster
kubectl get nodes


Pod Management

# Create and run a specific standalone Pod
kubectl run firstpod --image=nginx:1.14 --restart=Never

# Inspect detailed configuration and status of a Pod
kubectl describe pod firstpod

# Interact with a running container inside a Pod
kubectl exec -it firstpod -- bash

Deployments & Services
# Create a managed Deployment tracking a replica set
kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4

# Expose the deployment target port via a NodePort Service
kubectl expose deployment hello-minikube --type=NodePort --port=8080

# List all active resources (Pods, Services, Deployments, ReplicaSets)
kubectl get all

💡 Accessing NodePort Apps: To easily get the external URL of an exposed service in Minikube without hunting down cluster IPs manually, you can run:

minikube service hello-minikube --url
