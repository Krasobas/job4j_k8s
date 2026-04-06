# Kubernetes Deployment for job4j_devops

This repository contains Kubernetes configuration files for the `job4j_devops` project. We follow the GitOps approach, where all infrastructure changes are managed through Git.

## Project Structure

* `deployment.yaml` - Kubernetes Deployment manifest to run the application.

## Kubernetes Deployment Manifest

The following configuration creates 3 replicas of the calculator application.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: job4j-devops
  labels:
    app: calculator
spec:
  replicas: 3
  selector:
    matchLabels:
      app: calculator
  template:
    metadata:
      labels:
        app: calculator
    spec:
      containers:
        - name: calculator
          image: krsobas/job4j_devops:latest
          ports:
            - containerPort: 8080
          resources:           
            requests:
              memory: "256Mi"
              cpu: "100m"
            limits:
              memory: "512Mi"
              cpu: "500m"