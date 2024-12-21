# Backend Application Fixes and Verification

 Deploying a backend application with Redis using Docker and Helm on Minikube.

## Prerequisites

Ensure you have the following tools installed and configured:
- Docker
- Minikube
- Helm
- kubectl

## Building the Docker Image

1. First, point your shell to minikube's docker daemon:
```bash
eval $(minikube docker-env)
```

2. Navigate to the backend-app directory and build the Docker image:
```bash
docker build -t backend-app:latest .
```

Note: The Dockerfile has been corrected with the proper CMD line.

## Helm Chart Modifications

The following bugs were identified and fixed in the sample-helm-chart:

### 1. Deployment.yaml Fixes
- Fixed label selectors to ensure proper pod matching
- Corrected port configuration to match Dockerfile exposures
- Added missing Redis environment variable for application connectivity

### 2. Redis.yaml Fixes
- Corrected container image name typo
- Updated port number to the correct value
- Fixed label selector mismatches

## Deployment Steps

1. Deploy the application using Helm:
```bash
cd sample-helm-chart
helm upgrade --install backend-app .
```

2. Verify the deployment:
```bash
kubectl get pods
kubectl get services
```

## Testing the Application

### Testing the Hit Counter API

1. Get your Minikube IP:
```bash
minikube ip
```

2. Test the endpoint:
```bash
curl http://<minikube-ip>:31000/
```

### Verifying Redis Data

1. Get the Redis pod name:
```bash
kubectl get pods
```

2. Connect to Redis CLI:
```bash
kubectl exec -it <redis-pod-name> -- redis-cli
```

3. Check the hits counter:
```bash
GET hits
```

## SnapShots

![image](https://github.com/user-attachments/assets/dce48c76-ed56-4792-9c60-bb83df273085)

![image](https://github.com/user-attachments/assets/4ae1ff04-5dab-415c-bba7-db81a5832ddd)

![image](https://github.com/user-attachments/assets/900c2f2b-7339-4685-8942-88e13352dba4)

![image](https://github.com/user-attachments/assets/8a7666bb-a9ba-4022-a6a8-d70e9a58f68e)

![image](https://github.com/user-attachments/assets/1d283b9a-fc90-4202-91fe-592daf238aa2)


Clean up your resources when done:
```bash
helm uninstall backend-app
```
