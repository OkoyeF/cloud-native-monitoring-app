# Cloud Native Monitoring Application

<br>

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Docker](https://img.shields.io/badge/Docker-20.10%2B-blue)
![AWS](https://img.shields.io/badge/AWS-ECR%2FEKS-orange)
![Kubernetes](https://img.shields.io/badge/Kubernetes-EKS-blue)
![License](https://img.shields.io/badge/License-MIT-green)

<br>

A modern cloud-native monitoring solution built with Python, containerized with Docker, and deployed on AWS EKS with images stored in AWS ECR.

<br>

## Features

<br>

- Real-time system metrics monitoring  
- Custom metric collection  
- Alerting system with configurable thresholds  
- REST API for metric queries  
- Kubernetes-native deployment  
- Scalable microservice architecture  

<br>

## Prerequisites

<br>

| Requirement       | Version       |
|-------------------|---------------|
| Python            | 3.8+          |
| Docker            | 20.10+        |
| AWS CLI           | Configured    |
| kubectl           | EKS-configured|
| Helm (optional)   | 3.0+          |

<br>

## Project Structure

<br>
<ul>
app/ # Application source code
eks.py # Kubernetes clusters
app.py # Entry point
metrics/ # Metric collection modules
index.html # source code
Dockerfile # Docker configuration
requirements.txt # Python dependencies
deployment/ # Kubernetes manifests
deployment.yaml
service.yaml
hpa.yaml # Horizontal Pod Autoscaler
build_push.sh # Build and push to ECR
</ul>

<br>


## Getting Started
<br>
1 Build Docker Image
```bash
docker build -t cloud-native-monitoring-app .

2 Push to AWS ECR
```bash
# Authenticate Docker to your ECR registry
aws ecr get-login-password | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com

# Tag your image
docker tag cloud-native-monitoring-app:latest <AWS_ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/cloud-native-monitoring-app:latest

# Push to ECR
docker push <AWS_ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/cloud-native-monitoring-app:latest

3 Deploy to EKS
```bash

# Update kubeconfig
aws eks --region <REGION> update-kubeconfig --name <CLUSTER_NAME>

# Apply Kubernetes manifests
kubectl apply -f deployment/deployment.yaml
kubectl apply -f deployment/service.yaml

# Verify deployment
kubectl get pods -n monitoring

Configuration

Environment variables can be set in the Kubernetes deployment manifest:

yaml
Copy
env:
- name: METRIC_INTERVAL
  value: "60"  # Collection interval in seconds
- name: ALERT_THRESHOLD
  value: "90"   # CPU% threshold for alerts

Monitoring Endpoints

Endpoint	Description
GET /metrics	Prometheus-formatted metrics
GET /health	Health check endpoint
GET /alerts	List active alerts

Scaling

The application includes Horizontal Pod Autoscaler configuration:

bash
Copy
kubectl apply -f deployment/hpa.yaml

Cleanup

bash
Copy
# Delete Kubernetes resources
kubectl delete -f deployment/deployment.yaml
kubectl delete -f deployment/service.yaml

# Remove image from ECR
aws ecr batch-delete-image --repository-name cloud-native-monitoring-app --image-ids imageTag=latest

Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss what you would like to change.





<div align="center"> Made with ❤️ for cloud-native monitoring </div> ```
