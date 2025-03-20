# Minikube Cluster Setup

## Overview

This Minikube cluster is configured to deploy a backend and frontend application using Kubernetes. The deployment strategy follows a rolling update approach with defined resource limits. Persistent storage is implemented for the Django backend logs using a Persistent Volume (PV) and a Persistent Volume Claim (PVC).

## Cluster Components

### 1. Backend Application

- **Deployment:** Defines the backend pods with rolling update strategy.
- **Service:** Exposes the backend to other services within the cluster.
- **Resource Limits:** CPU and memory requests and limits are set to optimize resource utilization.
- **Persistent Volume & PVC:** Logs from the backend application are stored persistently in a volume mounted at `/usr/src/app/logs`.

### 2. Frontend Application

- **Deployment:** Manages frontend pods and updates them gradually using rolling updates.
- **Service:** Exposes the frontend to the public or other services.
- **Resource Limits:** Defines CPU and memory constraints to prevent resource exhaustion.

## Persistent Storage for Django Backend Logs

- **Persistent Volume (PV):** A hostPath volume at `/mnt/data/django-logs` for storing logs.
- **Persistent Volume Claim (PVC):** Requests storage from the PV for use by the backend deployment.

## Rolling Update Strategy

- Ensures zero downtime during deployments.
- Gradually replaces old pods with new ones.
- Configured to maintain a minimum number of available replicas during updates.

## Steps to Apply Manifests

1. **Apply Persistent Volume (PV) and Persistent Volume Claim (PVC):**
   ```sh
   kubectl apply -f pv.yaml
   kubectl apply -f pvc.yaml
   ```

2. **Deploy Backend Application:**
   ```sh
   kubectl apply -f backend-deployment.yaml
   kubectl apply -f backend-service.yaml
   ```

3. **Deploy Frontend Application:**
   ```sh
   kubectl apply -f frontend-deployment.yaml
   kubectl apply -f frontend-service.yaml
   ```

## How to View Logs

1. Connect to the backend pod:
   ```sh
   kubectl exec -it <backend-pod-name> -- bash
   ```
2. Navigate to the logs directory:
   ```sh
   cd /usr/src/app/logs
   ls -la
   ```

## Managing the Cluster

- **Check running pods:**
  ```sh
  kubectl get pods -o wide
  ```
- **Delete and restart a pod:**
  ```sh
  kubectl delete pod <pod-name>
  ```
- **Check logs:**
  ```sh
  kubectl logs <pod-name>
  ```

