# My Video SRT App Kubernetes Deployment

This repository contains Kubernetes manifests for deploying the `luminoussg/my-video-srt-app:v1.2` image using ArgoCD.

## Files

- `prod/deployment.yaml`: Contains the Deployment, Service, and Ingress definitions
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

### Method 1: Using NodePort

The application is exposed through a Kubernetes Service of type NodePort with port 30080. You can access it by using the IP address of any of your Kubernetes nodes:

```
http://192.168.31.100:30080
```

Or:

```
http://192.168.31.101:30080
```

Or:

```
http://192.168.31.102:30080
```

### Method 2: Using Ingress

If you have an Ingress controller (like NGINX) set up in your cluster, you can also access the application using the hostname defined in the Ingress resource:

1. Add the following entry to your hosts file (C:\Windows\System32\drivers\etc\hosts on Windows):
   ```
   192.168.31.100 srt-app.local
   ```

2. Then access the application using:
   ```
   http://srt-app.local
   ```

### Troubleshooting

If you're unable to access the application:

1. Verify that the application pods are running:
   ```
   kubectl get pods -l app=my-video-srt-app
   ```

2. Check the logs for the application:
   ```
   kubectl logs -l app=my-video-srt-app
   ```

3. Make sure your firewall allows traffic to port 30080 on your Kubernetes nodes. 