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

The application is exposed through a Kubernetes Service of type NodePort with port 30080. You can access it using any of your Kubernetes node IPs:

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

## Important Notes

1. The application container uses port 3000 internally (standard for many Node.js applications)
2. The service routes external traffic on port 80 to container port 3000
3. The NodePort service exposes this on port 30080 on all cluster nodes

## Troubleshooting

If you're unable to access the application:

1. Verify that the application pods are running:
   ```
   kubectl get pods -l app=my-video-srt-app
   ```

2. Check if the service is properly configured:
   ```
   kubectl get svc my-video-srt-app
   ```

3. Check the endpoints to see if the service is connected to the pods:
   ```
   kubectl get endpoints my-video-srt-app
   ```

4. Check the logs for the application:
   ```
   kubectl logs -l app=my-video-srt-app
   ```

5. Make sure your firewall allows traffic to port 30080 on your Kubernetes nodes. 