# Todo Infrastructure

This repository contains the Kubernetes infrastructure manifests for the [ToDo-CICD-Project](https://github.com/AbdullatifHabiba/ToDo-CICD-Project). It provides GitOps-based deployment using ArgoCD for automated application delivery and management.

## Overview

This infrastructure repository includes:
- **Kubernetes Manifests**: Complete deployment configuration for the Todo application
- **ArgoCD Application**: GitOps configuration for automated deployments
- **MongoDB StatefulSet**: Persistent database with initialization scripts
- **Ingress Configuration**: Load balancing and external access


## Architecture

The infrastructure deploys:
- **Todo Node.js Application** (`abdullatifhabiba/todo-nodejs:latest`)
- **MongoDB Database** (StatefulSet with persistent storage)
- **ArgoCD Application** (GitOps controller)
- **Ingress Controller** (NGINX-based routing)

## Requirements

- Kubernetes cluster (1.20+)
- ArgoCD installed in the cluster
- NGINX Ingress Controller
- Persistent Volume support
- kubectl configured for cluster access

## ArgoCD Integration

This repository is designed to work with ArgoCD for GitOps-based deployments:

1. **Source Repository**: `https://github.com/AbdullatifHabiba/todo-k8s-manifests`
2. **Target Namespace**: `todo-app`
3. **Sync Policy**: Automated with self-healing enabled
4. **Path**: `k8s/` directory contains all manifests

## Deployment

### Option 1: Deploy via ArgoCD (Recommended)

1. Apply the ArgoCD application:
   ```bash
   kubectl apply -f k8s/argocd.yml
   ```

2. ArgoCD will automatically sync and deploy all resources from this repository.

### Option 2: Manual Deployment

1. Apply all manifests directly:
   ```bash
   kubectl apply -f k8s/namespace.yml
   kubectl apply -f k8s/secret.yml
   kubectl apply -f k8s/mongodb.yml
   kubectl apply -f k8s/app.yml
   ```

## Configuration

### Database Configuration
- **MongoDB Version**: 6.0
- **Database Name**: `todolist`
- **Storage**: 10Gi persistent volume
- **Initialization**: Automatic user creation via ConfigMap

### Application Configuration
- **Replicas**: 2 (for high availability)
- **Resources**: 128Mi-256Mi memory, 100m-200m CPU
- **Health Checks**: Liveness and readiness probes on `/health` endpoint
- **Environment**: Production mode

### Network Configuration
- **Service**: ClusterIP on port 80
- **Ingress**: Available at `todo.local` (configure DNS accordingly)
- **Internal Port**: Application runs on port 4000

## Monitoring

The application includes:
- **Liveness Probe**: HTTP GET `/health` (30s initial delay, 10s period)
- **Readiness Probe**: HTTP GET `/health` (5s initial delay, 5s period)

## Security

- Database credentials stored in Kubernetes Secrets
- MongoDB connection URI encoded in base64
- Network policies can be added for additional security

## Related Repositories

- **Main Application**: [ToDo-CICD-Project](https://github.com/AbdullatifHabiba/ToDo-CICD-Project)
- **Infrastructure Manifests**: This repository (`todo-k8s-manifests`)

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test in a development cluster
5. Submit a pull request

Changes to this repository will automatically trigger ArgoCD sync if the application is configured with automated sync policy.