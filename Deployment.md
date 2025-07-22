#🌐🚀 Full Stack E-Commerce App Deployment Guide (Local → Docker → EKS)

##📁 Project Structure

E-CommerceStore/
├── backend/
│   ├── user-service/
│   ├── product-service/
│   ├── cart-service/
│   └── order-service/
├── frontend/
├── k8s/
└── docker-compose.yml


##📌 Overview
This guide walks you through:
✅ Run the app locally to test functionality
🐳 Build and push Docker images
☁️ Deploy to Amazon EKS
⚙️ Automate with Jenkins Pipeline

##⚙️Prerequisites:

Git installed
Node.js and npm
Docker Desktop (running)
AWS CLI configured (aws configure)
kubectl and eksctl installed
A working EKS Cluster and VPC
Jenkins installed and configured
DockerHub account with credentials saved in Jenkins

🧱 Step A: Clone the Project
git clone https://github.com/<YOUR_USERNAME>/E-CommerceStore.git
cd E-CommerceStore

🧪Step B: Run backend services and frontend Locally
eg: Backend Services: cd backend/user-service
npm install
npm start

Repeat for: product-service, cart-service, order-service and Frontend

🐳Step C: Dockerize the App

1. Create a dockerfile for all services (user-service, product-service, cart-service, order-service and Frontend)
2. Build & Tag Docker Images
docker build -t yourdockerhub/user-service:latest backend/user-service
docker push yourdockerhub/user-service:latest
Repeat for other services + frontend.
Create a docker-compose.yml to run all-containers with one command.(when ever needed)
<img width="1365" height="672" alt="Screenshot 2025-07-22 003539" src="https://github.com/user-attachments/assets/f7511ccf-22c7-4ba6-97a1-b5813e1131fd" />


🧾Step D: Create Kubernetes Manifests (k8s/ folder)
For all backend services:
configmap.yaml(ConfigMap for environment variables (e.g., PORT, DB_URL))
deployment.yaml
service.yaml

For frontend service:
deployment.yaml
service.yaml

Create mongo-pvc.yaml, mongo-deployment.yaml, and mongo-service.yaml for MongoDB.

☸️Step E: Create EKS Cluster
eksctl create cluster --name yash-e-commerce --region ap-northeast-3 --nodegroup-name workers --node-type t3.medium --nodes 3

🚀Step F: Apply Kubernetes Files
kubectl apply -f k8s/

Check: 
kubectl get pods
kubectl get svc
kubectl get pvc

🧪Step G: Setup Jenkins Pipeline and stored in pipeline
Create DockerHub credentials in Jenkins:
ID: dockerhub-creds (Username + Password)

Build pipeline in Jenkins and run.

##▶️Result:
Fully functioning app after deploying app in EKS with Jenkinsfile(frontend as nodeport):
<img width="1365" height="683" alt="Screenshot 2025-07-22 000201" src="https://github.com/user-attachments/assets/6cda6646-5ce9-4e50-9849-2dc76be2d579" />
<img width="1365" height="680" alt="Screenshot 2025-07-22 000244" src="https://github.com/user-attachments/assets/71c6e684-bae7-47d8-9a02-314095e06bc9" />
<img width="1365" height="683" alt="Screenshot 2025-07-22 000303" src="https://github.com/user-attachments/assets/039bc04d-7be7-4d60-948f-8f8376d65fb6" />
<img width="1365" height="687" alt="Screenshot 2025-07-22 000320" src="https://github.com/user-attachments/assets/4f49da41-3c0d-4a59-a422-c138d68deafd" />
<img width="1365" height="675" alt="Screenshot 2025-07-22 000337" src="https://github.com/user-attachments/assets/a65c3aba-8619-47b2-99d7-1bdc2a5687e7" />

Fully functioning app after deploying app in EKS with Jenkinsfile(frontend as loadbalancer):
<img width="1365" height="670" alt="Screenshot 2025-07-22 221740" src="https://github.com/user-attachments/assets/16ece44f-8f68-4517-aa00-ecd9e9a1b9cb" />

Once account created the data storing on DB:
<img width="1348" height="680" alt="Screenshot 2025-07-22 000355" src="https://github.com/user-attachments/assets/25e451ef-3712-4419-813d-fb0523a5d4bd" />


