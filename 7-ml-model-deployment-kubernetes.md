Apply Deployment:
# Apply the declarative manifest configuration
kubectl apply -f flask-iris-deployment.yaml

🔍 Step 2: Monitor Workload Status
Track the lifecycle initialization state of the cluster workload pods until all container images are successfully pulled and initialized.
# Initial validation (Container Engine provisioning states)
kubectl get pods

# Expected state output after a brief interval:
# NAME                                     READY   STATUS    RESTARTS   AGE
# flask-iris-deployment-5d5f7f68c4-kmckk   1/1     Running   0          4m49s
# flask-iris-deployment-5d5f7f68c4-rwbpx   1/1     Running   0          4m49s
# flask-iris-deployment-5d5f7f68c4-s5dg7   1/1     Running   0          4m49s

🌐 Step 3: Create a NodePort Service
To routing inbound network traffic straight into the container environment instances, provision an imperative NodePort mapping.

⚠️ Technical Note: Ensure that the flag declarations match the designated containerPort: 8080 specified inside the container definitions block.
# Imperatively expose the service mapping port flags
kubectl create service nodeport flask-iris --tcp=8080:8080

# Verify active cluster routing parameters
kubectl get services

Output Resource Parameters Mapping Example:

NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
flask-iris   NodePort    10.97.235.241   <none>        8080:31375/TCP   9s
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP          50m

The cluster internally forwards port 8080 to the Node engine mapping allocation at port 31375

🚀 Step 4: Access the Application UI & Run Inference
Discover the target master runtime engine IP interface (typically via running minikube ip or checking active system nodes mappings) to query the UI panel.

Direct your preferred local system web browser environment or execution query to the endpoint layout:
http://<minikube-cluster-ip>:<node-port>/

# Based on resource parameters example:
[http://172.17.0.3:31375/](http://172.17.0.3:31375/)
