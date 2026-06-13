🗂️ Namespaces
Namespaces provide isolated virtual clusters within a single physical Kubernetes cluster, acting like logical directories holding your resource objects.

By default, kubectl executes all operations within the default namespace.

To target an object in a specific namespace, use the --namespace or -n flag.

# List resources within a specific namespace
kubectl get pods -n kube-system

# Delete a resource from a dedicated namespace
kubectl delete pod nginx-pod -n mystuff

🌐 Contexts & Configuration (kubeconfig)
Contexts allow you to define parameters for switching easily between different clusters, users, and targeted namespaces.

Inspecting Local Configuration
To view your active cluster configurations, client certificates, endpoints, and active context details:

cat ~/.kube/config

Creating and Switching Custom Contexts
If you frequently work in a non-default namespace, you can create a dedicated context to save your target namespace permanently:
# 1. Create a custom context bound to a target namespace
kubectl config set-context my-context --namespace mystuff

# 2. Switch current active context to the newly created one
kubectl config use-context my-context

# Verify configuration changes inside the file
kubectl config current-context

🔎 Inspecting & Diagnosing Resources
List Resources (get)
Fetch a high-level overview or state listing of active resources:
# Get all pods in the current namespace
kubectl get pods

# Get basic info with extended runtime details (-o wide)
kubectl get nodes -o wide
Describe Resource Details (describe)
Examine low-level operational details, cluster events, controller states, and configuration parameters for a specific resource:
kubectl describe pod nginx


🏗️ Managing Object Lifecycles (Declarative vs. Imperative)
1. Declarative Manifest Management (apply)
The industry standard for infrastructure-as-code management. Apply and update resource declarations via YAML manifests.

nginx-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80

Apply Manifest:
kubectl apply -f nginx-pod.yaml

2. Imperative Resource Deployment (create & expose)
Quickly provision workloads directly via command-line arguments.

# Create an imperative deployment from an image registry
kubectl create deployment hello-minikube --image=gcr.io/kubernetes-e2e-test-images/echoserver:2.2

# Expose the deployment as a NodePort Service to make it accessible outside the cluster
kubectl expose deployment hello-minikube --type=NodePort --port=8080
# Create an imperative deployment from an image registry
kubectl create deployment hello-minikube --image=gcr.io/kubernetes-e2e-test-images/echoserver:2.2

# Expose the deployment as a NodePort Service to make it accessible outside the cluster
kubectl expose deployment hello-minikube --type=NodePort --port=8080

📊 Comprehensive Cluster Overview
To inspect all micro-components running inside your target namespace boundaries simultaneously:
kubectl get all
# Discover the cluster IP and mapped target node port
# Look for: service/hello-minikube  NodePort  10.100.180.143  <none>  8080:31870/TCP

To hit the active endpoint, direct your browser or terminal environment using the node network mapping:
curl http://<minikube-cluster-ip>:<service-port>

# Example implementation structure:
curl [http://172.17.0.3:31870/](http://172.17.0.3:31870/)

CLIENT VALUES:
client_address=172.18.0.1
command=GET
real path=/
...
SERVER VALUES:
server_version=nginx: 1.10.0 - lua: 10001

