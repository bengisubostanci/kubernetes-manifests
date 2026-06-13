1. Install kubectl
Install the official Kubernetes command-line tool to interact with the cluster:

# Download the stable kubectl binary
curl -LO "[https://dl.k8s.io/release/$](https://dl.k8s.io/release/$)(curl -L -s [https://dl.k8s.io/release/stable.txt](https://dl.k8s.io/release/stable.txt))/bin/linux/amd64/kubectl"

# Install with root ownership and explicit permissions
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Verify the client version
kubectl version --client --output=yaml

Official Documentation: Install and Set Up kubectl on Linux

Start Cluster
Initiate Minikube specifying Docker as the container orchestration driver alongside optimized resource limits:
minikube start --driver='docker' --cpus='2' --memory='4096'

Discovering Runtime Sizing Options
To inspect configuration parameters for allocations:
minikube start --help | grep -E 'cpu|memory'

--cpus='2': Core count allocated to the control plane. Set to "max" to leverage all host threads.

--memory='4096': RAM footprint allocated (supported formats: b, k, m, g). Set to "max" for full host allocation.

Stop Cluster
Gracefully halt the running local Kubernetes control plane without destroying states:

minikube stop

Delete Cluster
Purge the local cluster context, state files, and container resources:
minikube delete
🔍 Diagnostics, Management & Access
Web Dashboard
Expose and connect directly to the native Kubernetes dashboard user interface:
minikube dashboard
Note: This automatically initiates a proxy channel. Use Ctrl + C within the terminal stream to terminate the process safely.

SSH Context Access
Directly tunnel into the underlying Minikube node engine container:
# Connect via SSH
minikube ssh

# Terminate session
docker@minikube:~$ exit

📊 Cluster Architecture Inspection
Cluster Details & Health
Query fundamental routing parameters and control-plane endpoint data:
kubectl cluster-info
To fetch lower-level logs and dump operational cluster context for troubleshooting:
kubectl cluster-info dump
Node Inventories
List active worker/master instances running within the current cluster boundaries:
kubectl get nodes -o wide

🏗️ Core System Components (kube-system)
1. Kubernetes Proxy (kube-proxy)
Manages network routing rules and handles internal network request load balancing across Services. Runs as a DaemonSet ensuring a footprint on every cluster node.
kubectl get daemonsets -n kube-system kube-proxy
2. Cluster DNS Engine (coredns)
Handles name resolution and service discovery namespaces across running Pod workloads.
# Verify the CoreDNS Deployment state
kubectl get deployments -n kube-system coredns

# Inspect the CoreDNS internal LoadBalancer Service mappings
kubectl get service -n kube-system kube-dns

Write content to file
file_name = "README.md"
with open(file_name, "w", encoding="utf-8") as f:
f.write(markdown_content)

print(f"File created successfully: {file_name}")
