
# Minikube Kubernetes Project

This project sets up a Kubernetes (K8s) cluster locally using **Minikube** for deploying a WordPress and MySQL setup. Docker is used to build the images, and the deployment is managed via Kubernetes manifests.

## Prerequisites

### 1. **Install Minikube**:
Follow the installation steps for Linux:
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

### 2. **Install Docker**:
Docker is needed for building container images. To install Docker on Linux, use the following commands:
```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

### 3. **Install kubectl**:
Install kubectl on Linux:
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

## Files in the Project

1. **wordpress-service.yml**: Defines the service for the WordPress application.
2. **wordpress-ingress.yml**: Configures ingress for external access to the WordPress application.
3. **wordpress-deployment.yml**: Manages the deployment of the WordPress containerized application.
4. **mysql-statefulset.yaml**: Configures a StatefulSet for deploying MySQL with persistent storage.
5. **docker-compose.yml**: Docker Compose file to build the necessary Docker images.
6. **mysql-service.yaml**: Defines the service for the MySQL database.

## Setup Instructions

### 1. Start Minikube:

```bash
minikube start
```

### 2. Build Docker Images:

Ensure that Docker is set to use Minikube's environment for building images:

```bash
eval $(minikube docker-env)
docker-compose build
```

### 3. Deploy MySQL:

Apply the MySQL StatefulSet and Service:

```bash
kubectl apply -f mysql-statefulset.yaml
kubectl apply -f mysql-service.yaml
```

### 4. Deploy WordPress:

Apply the WordPress Deployment and Service:

```bash
kubectl apply -f wordpress-deployment.yml
kubectl apply -f wordpress-service.yml
```

### 5. Configure Ingress:

To allow external access to WordPress, apply the ingress configuration:

```bash
kubectl apply -f wordpress-ingress.yml
```

### 6. Verify the Deployment:

Check if the MySQL and WordPress pods and services are running:

```bash
kubectl get pods
kubectl get svc
```

### 7. Access the Application:

If you're using Minikube's built-in service tunneling, you can access the WordPress application by exposing it:

```bash
minikube service wordpress-service
```

### 8. Minikube Dashboard:

You can also use Minikube's dashboard to manage and monitor the cluster:

```bash
minikube dashboard
```

## Clean Up

To stop and delete the Minikube cluster and remove all resources:

```bash
minikube stop
minikube delete
```

