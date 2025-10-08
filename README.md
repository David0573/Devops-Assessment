1. Setup Instructions for Local Environment:

Install Node.js (version 18 or compatible).

Install Docker on your machine.

Install Minikube for local Kubernetes cluster.

2. Local Run Commands:

To run the Next.js app locally without Docker:

npm install
npm run dev

To build and run the app in a Docker container locally:
docker build -t nextjs-ghcr-demo .
docker run -p 3000:3000 nextjs-ghcr-demo

3. Deployment Steps for Minikube:


Start Minikube:
minikube start

Apply Kubernetes manifests:

kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

Confirm pods and services are running:

kubectl get pods
kubectl get svc

4. Accessing the Application on Minikube:

Get Minikube IP:

minikube ip
minikube ip

Get service NodePort:

kubectl get svc nextjs-service

Access the app at http://<minikube-ip>:<node-port>.

Kubernetes Deployment Details

Replica count is set to 2 for high availability.

Readiness and liveness probes check the health of the app:

readinessProbe:
  httpGet:
    path: /
    port: 3000
  initialDelaySeconds: 5
  periodSeconds: 10

livenessProbe:
  httpGet:
    path: /
    port: 3000
  initialDelaySeconds: 15
  periodSeconds: 20

The Docker image is pulled from GHCR with specific tags for traceability.

CI/CD Pipeline
GitHub Actions workflow builds the Docker image on every push to main branch.

The image is pushed to GitHub Container Registry with the tag corresponding to the commit SHA.

Deployment manifests should be updated accordingly to pull images by specific tag.





