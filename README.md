Flipkart Product Chatbot - LLMOps Deployment Guide 🚀
Welcome to the deployment repository for the Flipkart Product Chatbot. This project demonstrates an end-to-end LLMOps pipeline, deploying a Flask-based LLM application on a Kubernetes cluster (Minikube) hosted on a Google Cloud Platform (GCP) Virtual Machine. It also features a fully configured monitoring stack using Prometheus and Grafana.

🛠️ Tech Stack & Architecture
Application Framework: Flask, Python

AI/LLM Ecosystem: Groq API, Hugging Face, DataStax Astra DB

Containerization: Docker

Orchestration: Kubernetes (Minikube, kubectl)

Cloud Provider: Google Cloud Platform (GCP) Compute Engine (Ubuntu 24.04 LTS)

Observability & Monitoring: Prometheus, Grafana

📋 Prerequisites
Before you begin, ensure you have an active Google Cloud account to provision the necessary infrastructure. You will also need your respective API keys for Groq, Astra DB, and Hugging Face.

GCP VM Specifications Recommended:
Series: E2 Standard

Memory: 16 GB RAM

Boot Disk: 256 GB (Ubuntu 24.04 LTS)

Firewall: Allow HTTP and HTTPS traffic

🚀 Step 1: Environment Setup
SSH into your newly created GCP VM instance and run the following commands to set up your environment.

1. Clone the Repository
Bash
git clone https://github.com/d-hackmt/flipkart-product-chatbot.git
cd flipkart-product-chatbot
2. Install Docker
Bash
# Add Docker's official GPG key and repository, then install
# (Refer to official docs for the exact standard installation script)
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Post-installation (Run Docker without sudo)
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker

# Enable Docker on boot
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
3. Install Minikube & Kubectl
Bash
# Install Minikube (Linux x86 Binary)
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Start Minikube Cluster
minikube start

# Install kubectl via Snap
sudo snap install kubectl --classic
🏗️ Step 2: Application Build & Deployment
With the cluster running, point Docker to Minikube's internal environment and build the application image.

Bash
# Point Docker to Minikube
eval $(minikube docker-env)

# Build the Flask application image
docker build -t flask-app:latest .
Configure Secrets
Load your environment variables into the Kubernetes cluster. Make sure to replace the empty strings with your actual keys.

Bash
kubectl create secret generic llmops-secrets \
  --from-literal=GROQ_API_KEY="your_groq_api_key" \
  --from-literal=ASTRA_DB_APPLICATION_TOKEN="your_astra_token" \
  --from-literal=ASTRA_DB_KEYSPACE="default_keyspace" \
  --from-literal=ASTRA_DB_API_ENDPOINT="your_astra_endpoint" \
  --from-literal=HF_TOKEN="your_hf_token" \
  --from-literal=HUGGINGFACEHUB_API_TOKEN="your_hf_api_token"
Deploy the Application
Bash
# Apply the Kubernetes deployment configuration
kubectl apply -f flask-deployment.yaml

# Verify the pods are running
kubectl get pods

# Expose the application via Port-Forwarding
kubectl port-forward svc/flask-service 5000:80 --address 0.0.0.0
Your application is now accessible at http://<VM_EXTERNAL_IP>:5000.

📊 Step 3: Observability (Prometheus & Grafana)
To ensure the chatbot performs optimally, deploy the monitoring stack in a dedicated namespace.

1. Deploy the Monitoring Stack
Open a new terminal session in your VM and run:

Bash
# Create monitoring namespace
kubectl create namespace monitoring

# Deploy Prometheus
kubectl apply -f prometheus/prometheus-configmap.yaml
kubectl apply -f prometheus/prometheus-deployment.yaml

# Deploy Grafana
kubectl apply -f grafana/grafana-deployment.yaml

# Verify monitoring pods
kubectl get pods -n monitoring
2. Access the Dashboards
Use port-forwarding to access the monitoring interfaces externally:

Prometheus (Target Health & Metrics):

Bash
kubectl port-forward --address 0.0.0.0 svc/prometheus-service -n monitoring 9090:9090
Access at: http://<VM_EXTERNAL_IP>:9090

Grafana (Visualizations):

Bash
kubectl port-forward --address 0.0.0.0 svc/grafana-service -n monitoring 3000:3000
Access at: http://<VM_EXTERNAL_IP>:3000 (Default Login - Username: admin, Password: admin)

3. Connect Prometheus to Grafana
Navigate to your Grafana URL.

Go to Settings > Data Sources > Add Data Source.

Select Prometheus.

Set the URL to: [http://prometheus-service.monitoring.svc.cluster.local:9090](http://prometheus-service.monitoring.svc.cluster.local:9090)

Click Save & Test (Look for the green success message!).

You are now ready to build custom dashboards to track token usage, request latency, and application health.
