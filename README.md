Kubernetes (K8s) – Clean & Simple Notes
1. What is Kubernetes?
Kubernetes is a container orchestration platform used to deploy, manage, scale, and heal containerized applications automatically.
Why Kubernetes?
1. Container Orchestration – Manages many containers across many machines
2. Scalability – Increase or decrease application instances easily
3. Self-Healing – Restarts failed containers automatically
4. Load Balancing – Distributes traffic across multiple pods

2. Kubernetes Architecture
Cluster Architecture
* Cluster = Group of servers (nodes)
* Two main parts:
    1. Control Plane (Master Node)
    2. Worker Nodes

3. Control Plane (Master Node)
The control plane manages the entire cluster.
Main Components
1. API Server
* Entry point to Kubernetes
* Receives requests from:
    * kubectl
    * Web dashboard
* Validates and processes requests
2. etcd
* Distributed key-value store
* Stores:
    * Cluster state
    * Pod info
    * Configurations
3. Scheduler
* Checks pending pods in etcd
* Decides which worker node should run the pod
* Uses node info like:
    * CPU
    * Memory
    * Health
4. Controller Manager
* Ensures desired state = actual state
* Examples:
    * If a pod dies → create a new one
    * If replicas = 3 but only 2 running → create 1 more

4. Worker Nodes
Worker nodes do the actual work.
Components in Worker Node
1. Pod
* Smallest deployable unit
* Can contain:
    * One container
    * Multiple containers
* Multiple pods can run on one worker node
2. Docker (Container Runtime)
* Runs containers inside pods
3. Kubelet
* Agent running on each worker node
* Communicates with Control Plane
* Responsibilities:
    * Start pods
    * Monitor pod health
    * Report status to API Server
4. Kube-Proxy
* Handles networking
* Enables:
    * Pod-to-pod communication
    * Service networking

5. kubectl
Command-line tool to interact with Kubernetes.
Ways to Communicate with Cluster
1. kubectl (CLI) – most commonly used
2. Web Dashboard
Example:
kubectl get nodes

6. Kubernetes Flow (High Level)
1. User sends request via kubectl
2. API Server receives the request
3. Data stored in etcd
4. Scheduler selects a worker node
5. Kubelet runs the pod
6. Kube-proxy manages networking

7. Cluster Setup Options
1. Self-Managed Cluster
a. Minikube
* Single machine
* For learning and practice
* Control plane + worker on same system
b. kubeadm
* Multi-node cluster
* Manual setup
* Downtime risk if master fails

2. Managed Kubernetes Cluster
Cloud provider manages control plane.
Cloud	Service
AWS	EKS (Elastic Kubernetes Service)
Azure	AKS
GCP	GKE
IBM	IKE
8. AWS EKS
* Managed Kubernetes on AWS
* Control plane managed by AWS
* High availability & auto scaling
📘 Reference: GitHub EKS Setup Guide (as provided)

9. Deploy Your First App (Minikube)
Step 1: Install Tools
* Install kubectl
* Install minikube
Step 2: Start Cluster
minikube start
With memory:
minikube start --memory=4096 --driver=virtualbox
Step 3: Verify Cluster
kubectl get nodes

10. Create a Pod
sample.yml
apiVersion: v1
kind: Pod
metadata:
  name: samplepod
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
Create Pod
kubectl apply -f sample.yml
Check Pod
kubectl get pods
kubectl get pods -o wide
Access Pod
* Get Pod IP
* Use curl:
curl <pod-ip>
Login to Minikube
minikube ssh

11. Debugging Pods
Pod Details
kubectl describe pod nginx
Pod Logs
kubectl logs nginx

12. Container vs Pod vs Deployment
Container
* Created using Docker
docker run -it imageName -p 8080:80
Pod
* Wraps one or more containers
* Defined using YAML
* No auto-healing or scaling
Deployment
* Wrapper over Pods
* Provides:
    * Auto-healing
    * Auto-scaling
* Uses ReplicaSet
* ReplicaSet creates Pods

13. Create a Deployment
* Create deployment YAML
* Change replicas
* Delete pod and observe auto-healing
Example:
kubectl delete pod <pod-name>
kubectl get pods

14. Kubernetes Service
Why Service?
* Pod IPs change when pods restart
* Service provides stable IP & DNS
What Service Does
* Load balances traffic
* Routes requests using labels
* Connects users to pods

15. Service Flow
1. Deployment creates pods
2. Pods have labels (e.g., payment-service)
3. Service selects pods using labels
4. User connects to Service IP

16. Service Types
1. ClusterIP
* Default
* Accessible only inside cluster
2. NodePort
* Exposes service on node IP + port
* Accessible inside organization network
3. LoadBalancer
* Creates cloud load balancer
* Public access

17. Practical Service Implementation
Step 1: Build Docker Image
docker build -t imagename:v1 .
Step 2: Create deployment.yml
* Define:
    * App name
    * Container name
    * Container port
Step 3: Deploy
kubectl apply -f deployment.yml -v=7
Step 4: Verify
kubectl get deploy -v=7
kubectl get pods -v=7
kubectl get pods -o wide -v=7

18. Key Interview Lines (Short & Powerful)
* Kubernetes automates deployment, scaling, and management of containers.
* Pod is the smallest unit in Kubernetes.
* Deployment provides auto-healing and scaling.
* Service provides stable networking and load balancing.
* Control Plane manages cluster state.
* Worker Nodes run application workloads.

 

Spring boot Concepts: