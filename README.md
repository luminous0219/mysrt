# My Video SRT App Kubernetes Deployment

This repository contains Kubernetes manifests for deploying the `luminoussg/my-video-srt-app:v1.2` image using ArgoCD.

## Files

- `prod/deployment.yaml`: Contains the Deployment and Service definitions
- `prod/kustomization.yaml`: Kustomize configuration for the application

## Deploying with ArgoCD

To deploy this application with ArgoCD:

1. In the ArgoCD UI, click "New App"
2. Set the application name (e.g., "my-video-srt-app")
3. Set the project to "default" (or your preferred ArgoCD project)
4. For "Sync Policy", choose your preferred option (e.g., "Automatic")
5. For repository URL, use the URL of this Git repository
6. For path, use "prod" (the folder containing the manifests)
7. For destination cluster, use "in-cluster" (or your preferred cluster)
8. For namespace, use "default" (or your preferred namespace)
9. Click "Create" to deploy the application

ArgoCD will automatically sync the application and deploy it to your Kubernetes cluster.

## Accessing the Application

The application is exposed through a Kubernetes Service of type ClusterIP. To access it from outside the cluster, you can either:

1. Set up an Ingress resource
2. Use port-forwarding: `kubectl port-forward svc/my-video-srt-app 8080:80` 