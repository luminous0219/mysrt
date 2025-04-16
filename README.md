# My Video SRT App Kubernetes Deployment

This repository contains Kubernetes manifests for deploying the `luminoussg/my-video-srt-app:v1.2` image using ArgoCD.

## Files

- `prod/my-video-srt-app-deployment.yaml`: Deployment definition
- `prod/my-video-srt-app-svc.yaml`: Service definition 
- `prod/kustomization.yaml`: Kustomize configuration for the application

## Deploying with ArgoCD

To deploy this application with ArgoCD:

1. In the ArgoCD UI, click "+ New App" (top-left corner)

2. Fill in the application details:
   - Application Name: my-video-srt-app
   - Project: default
   - Sync Policy: Manual (or Automatic if you prefer GitOps automation)
   - Repository URL: [Your Git Repository URL]
   - Revision: HEAD
   - Path: prod
   - Cluster URL: https://kubernetes.default.svc (your in-cluster Kubernetes)
   - Namespace: default

3. Click "Create"

4. Click "Sync" to deploy the application to your cluster

## Accessing the Application

After deployment, the service will automatically be assigned NodePort values. To find out which ports were assigned, run:

```
kubectl get svc my-video-srt-app
```

You should see output similar to:
```
NAME               TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)                                     AGE
my-video-srt-app   NodePort   10.111.26.128   <none>        80:30XXX/TCP,1935:31XXX/TCP,1935:31XXX/UDP,4200:32XXX/UDP   1m
```

Where 30XXX, 31XXX, and 32XXX are the assigned NodePort values.

Access the application using any of your Kubernetes node IPs with the appropriate NodePort:

- Web UI: http://192.168.31.100:30XXX (replace 30XXX with actual web port)
- SRT (TCP): srt://192.168.31.100:31XXX (replace 31XXX with actual SRT TCP port)
- SRT (UDP): srt://192.168.31.100:31XXX (replace 31XXX with actual SRT UDP port)
- Alternative SRT (UDP): srt://192.168.31.100:32XXX (replace 32XXX with actual SRT alt port)

## Port Forwarding (Alternative Access Method)

If you prefer not to use NodePort, you can set up port forwarding similar to the guestbook example:

```
# Forward web interface
kubectl port-forward svc/my-video-srt-app 8080:80

# Forward SRT port
kubectl port-forward svc/my-video-srt-app 1935:1935
```

Then access the application at:
- Web UI: http://localhost:8080
- SRT: srt://localhost:1935

## About SRT Applications

Based on research:
1. SRT typically uses UDP protocol (but may also use TCP)
2. Common SRT ports are 1935 (standard) and 4200
3. Web interfaces typically run on port 8080 inside the container

## Important Notes

Based on research of similar SRT applications:
1. SRT typically uses UDP protocol, so try the UDP port options first
2. SRT often uses port 1935 (standard) or 4200 (seen in some implementations)
3. The web interface is likely on port 8080 inside the container (mapped to 30080 on nodes)

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

5. Make sure your firewall allows traffic to the NodePort ports (30080, 31935, 32400) on your Kubernetes nodes. 