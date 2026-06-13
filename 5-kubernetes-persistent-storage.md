🏗️ Step 1: Define the PersistentVolume (PV)
Create a static local cluster storage pool mapped directly to the local mounted machine path.

pv-volume.yaml

apiVersion: v1
kind: PersistentVolume
metadata:
  name: task-pv-volume
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/minikube-host/nginx_data"

Apply and Verify PV State:
kubectl apply -f pv-volume.yaml

# Inspect allocation state (Status should read: Available)
kubectl get pv task-pv-volume

📑 Step 2: Define the PersistentVolumeClaim (PVC)
Request a specific fraction of the initialized capacity pool using matching storage access patterns.

pv-claim.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: task-pv-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi


Apply PVC Manifest:
kubectl apply -f pv-claim.yaml

# Re-inspecting resources will show the PV status has changed to 'Bound'
kubectl get pvc task-pv-claim


🚀 Step 3: Deploy the Pod with Mounted Volume Claim
Bind the generated application namespace claim parameter into the target functional application pod definition.

pv-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: task-pv-pod
spec:
  volumes:
    - name: task-pv-storage
      persistentVolumeClaim:
        claimName: task-pv-claim
  containers:
    - name: task-pv-container
      image: nginx
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: task-pv-storage


Deploy workload:
kubectl apply -f pv-pod.yaml

🔎 Step 4: Verification & Operational Testing
Tunnel into the running container infrastructure to guarantee the cluster storage engine functions accurately:
# 1. Execute an interactive shell session inside the target Pod 
kubectl exec -it task-pv-pod -- /bin/bash

# 2. Run debugging diagnostics inside the active container shell environment
root@task-pv-pod:/# apt update && apt install curl -y

# 3. Test the web server index route mapping locally
root@task-pv-pod:/# curl http://localhost/

Expected Execution Output:
Hello from Kubernetes storage

