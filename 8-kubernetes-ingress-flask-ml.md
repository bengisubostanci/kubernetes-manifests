Expected Output Layout:
NAME                                       READY   STATUS      RESTARTS   AGE
ingress-nginx-admission-create-nvmxs        0/1     Completed   0          4m54s
ingress-nginx-admission-patch-p7f42         0/1     Completed   1          4m54s
ingress-nginx-controller-7c6974c4d8-kzm4c   1/1     Running     0          4m55s

Ensure the ingress-nginx-controller pod reaches a steady Running state before moving forward.

🏗️ Step 2: Define and Apply the Ingress Manifest
Create an Ingress rule structure that intercepts traffic targeting a specific domain host and routes it to the underlying Flask service endpoint.

ingress-flask-iris.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: flask-iris-ingress
spec:
  rules:
    - host: flask.iris.classification.vbo.local
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: flask-iris
                port:
                  number: 8080

Apply Manifest:
# Deploy the declarative Ingress routing infrastructure
kubectl apply -f ingress-flask-iris.yaml
🔍 Step 3: Inspect Ingress Resources
Query the cluster networking boundaries to verify the deployment state of the controller layer:
# Retrieve active Ingress route paths
kubectl get ingress
💡 API Versioning Note: In older Kubernetes cluster footprints, you might notice a deprecation warning regarding extensions/v1beta1. The production manifest provided above uses the fully compliant stable API version networking.k8s.io/v1.

📝 Step 4: Local DNS Resolution Configuration
To enable network mapping using custom domain names (flask.iris.classification.vbo.local), link your Minikube cluster node IP interface directly into the host operating system configuration layer.

1. Update /etc/hosts
Fetch your target Minikube node IP instance (via minikube ip or node inspection) and modify your local configuration:

sudo vi /etc/hosts
2. Append Network Parameter:
172.17.0.3  flask.iris.classification.vbo.local

(Replace 172.17.0.3 with your exact active Minikube master IP environment endpoint if different.)

🚀 Step 5: End-to-End Operational Validation
Open your preferred local workstation web browser or execute an inference client payload to target the live machine learning pipeline interface:

[http://flask.iris.classification.vbo.local/](http://flask.iris.classification.vbo.local/)
📊 Verification Flow
Connection Check: Ensure the browser reaches the interface successfully without routing failures.

ML Inference Panel: Input testing feature metrics into the Iris payload submission forms.

Execution Return: Confirm the server handles operational predictions back successfully across the Ingress network proxy boundary.
