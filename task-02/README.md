# Task 02: Deploy Applications with Kubernetes Deployments

## Objective

The Nautilus DevOps team is delving into Kubernetes for app management. One team member needs to create a deployment following these details:


Create a deployment named `nginx` to deploy the application `nginx` using the image `nginx:latest` (ensure to specify the tag)

## Solution

To create a deployment we need to use `create deployment`
Example:

```bash
kubectl create deployment <my-dep> --image=<container-image>
```
Now change to the right setting.

```bash
kubectl create deployment nginx --image=nginx:latest
# Output
deployment.apps/nginx created
```
Verify the deployment status.

```bash
kubectl get deployments
# Output
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   1/1     1            1           35s
```
Last verify the pod status.

```bash
kubectl get pods
# Output
NAME                     READY   STATUS    RESTARTS   AGE
nginx-7c5d8bf9f7-v2dj7   1/1     Running   0          2m50s
```
