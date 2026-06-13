🏗️ Step 1: Deploy Backend Applications & Services
We will deploy two separate workloads (nginx and httpd) and expose them internally using NodePort Services.

1. Nginx Deployment & Service (nginx-deployment-services.yaml)

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: NodePort

2. Httpd Deployment & Service (httpd-deployment-services.yaml)
3. apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd
spec:
  replicas: 1
  selector:
    matchLabels:
      app: httpd
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
      - name: httpd
        image: httpd
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: httpd-service
spec:
  selector:
    app: httpd
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: NodePort

Apply Manifests & Verify Services:
# Apply both application definitions
kubectl apply -f nginx-deployment-services.yaml
kubectl apply -f httpd-deployment-services.yaml

# Inspect active services to verify node allocations
kubectl get svc
🌐 Step 2: Configure Ingress Routing
Now, we create an Ingress resource that maps paths (/nginx and /httpd) under a single domain host rule to their respective backend cluster services.

ingress-conf.yaml

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: multi-app-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: vbo.argocd.example.com
    http:
      paths:
      - path: /nginx
        pathType: Prefix
        backend:
          service:
            name: nginx-service
            port:
              number: 80
      - path: /httpd
        pathType: Prefix
        backend:
          service:
            name: httpd-service
            port:
              number: 80


Apply Ingress Manifest:
kubectl apply -f ingress-conf.yaml

📝 Step 3: Local DNS & Verification
To access the cluster via the configured host domain (vbo.argocd.example.com), map the Minikube IP address inside the local host file.

1. Update /etc/hosts
Fetch your Minikube IP instance via minikube ip (e.g., 192.168.49.2) and map it:
sudo vi /etc/hosts
Add the following line to the file:
192.168.49.2    vbo.argocd.example.com

   2. Operational Testing (CLI Verification)
Execute client URL requests to verify the path routing rules are working as expected:
# Verify access to the Httpd backend
curl [http://vbo.argocd.example.com/httpd](http://vbo.argocd.example.com/httpd)

# Verify access to the Nginx backend
curl [http://vbo.argocd.example.com/nginx](http://vbo.argocd.example.com/nginx)


🖥️ Step 4: External Web Browser Access (Host-to-VM)
If running Minikube inside a VirtualBox Linux VM and wishing to access the endpoints directly from your main host machine's web browser:

1. Install and Configure Reverse Proxy on the VM
Install an external Nginx engine to listen on localhost and proxy requests to the internal Ingress domain:
# Install Nginx server package
sudo yum install nginx -y

# Configure SELinux to Permissive mode to allow proxy network connections
sudo setenforce permissive

# Persist SELinux changes across system reboots
# Update SELINUX=enforcing to SELINUX=permissive inside the file
sudo vi /etc/sysconfig/selinux
2. Configure Host Nginx Routing
Overwrite the default Nginx routing block to pass system traffic cleanly:
cat <<EOF | sudo tee /etc/nginx/nginx.conf
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    server {
        listen 80;
        server_name 127.0.0.1;

        location / {
            proxy_pass [http://vbo.argocd.example.com](http://vbo.argocd.example.com);
            proxy_set_header Host \$host;
            proxy_set_header X-Real-IP \$remote_addr;
        }
    }
}
EOF


 Restart Proxy Servic
 sudo systemctl restart nginx
sudo systemctl status nginx

🔌 Step 5: Port Forwarding & Browser Testing
Open VirtualBox Manager Settings for your target Linux virtual machine.

Navigate to Network -> Advanced -> Port Forwarding.

Create a new port routing rule:

Host Port: 1080

Guest Port: 80

Open your host workstation web browser and target the local pipeline endpoint:

[http://127.0.0.1:1080/nginx](http://127.0.0.1:1080/nginx)
[http://127.0.0.1:1080/httpd](http://127.0.0.1:1080/httpd)
