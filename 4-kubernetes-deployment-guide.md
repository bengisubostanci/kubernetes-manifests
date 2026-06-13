Applying the Manifest

# Create or update the deployment context
kubectl apply -f nginx-deployment.yaml

# Verify created pods and workload resources
kubectl get pods
kubectl get all

📈 Capacity Scaling Management
1. Manual Resource Scaling
To scale up or scale down the desired number of active Pod replicas immediately:
# Scale the target deployment capacity to 5 replicas
kubectl scale deployment nginx-deployment --replicas=5

# Inspect current ReplicaSet configuration limits
kubectl get rs

2. Horizontal Pod Autoscaling (HPA)
Automate scaling metrics based on resource utilization limits (e.g., target CPU thresholds):
# Autoscale deployment between 2 and 6 pods when average CPU exceeds 80%
kubectl autoscale deployment.v1.apps/nginx-deployment --min=2 --max=6 --cpu-percent=80

# Monitor HPA target metrics and operational tracking states
kubectl get hpa

🔄 Rolling Updates (Zero-Downtime Image Updates)
To update application container image versions without incurring downtime, Kubernetes triggers a progressive rollout by replacing older ReplicaSet Pods with new instances:

# Update container image to version 1.16.1
kubectl set image deployment.v1.apps/nginx-deployment nginx=nginx:1.16.1

# Monitor rolling update rollout progress and deployment status
kubectl get deploy
Inspecting Under-the-Hood Deployment History
During the update, a new ReplicaSet is dynamically created to provision the updated pods, while the old one scales down to 0:
kubectl get rs
🌐 Exposing the Workload Locally (NodePort)
To route external network traffic into your deployment's application pods, expose it using a NodePort Service mapping:
# Bind the target deployment port to a designated cluster node port
kubectl expose deployment nginx-deployment --type=NodePort --name=nginx-service --port=80 --target-port=80

# Retrieve network endpoints and port mapping definitions
kubectl get svc nginx-service

Fetching Cluster Endpoint In Minikube
Identify the target service port mapped to your active Minikube master IP instance:
# Formatted Output structure example: 
# NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
# nginx-service   NodePort    10.111.76.185   <none>        80:32656/TCP   112s

# Curl the endpoint target using the minikube engine profile IP
curl [http://192.168.49.2:32656/](http://192.168.49.2:32656/)
