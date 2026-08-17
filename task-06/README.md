# Task 05: Revert Deployment to Previous Version in Kubernetes

## Objective

Earlier today, the Nautilus DevOps team deployed a new release for an application. However, a customer has reported a bug related to this recent release. Consequently, the team aims to revert to the previous version.


There exists a deployment named `nginx-deployment`; initiate a rollback to the previous revision.

## Solution
The solution is very simple. We're going to use `describe` just to verify the `rollback`

```bash
kubectl describe deployment nginx-deployment
# Output
Name:                   nginx-deployment
Namespace:              default
...
  Containers:
   nginx-container:
    Image:         nginx:alpine
...
```
We can see that the container image is `ngnx:alpine`. Now let's use `rollback`

```bash
kubectl rollout undo deployment/nginx-deployment
# Output
deployment.apps/nginx-deployment rolled back
```
we can check the status of the `rollback`
```bash
kubectl rollout status deployment/nginx-deployment
# Output
deployment "nginx-deployment" successfully rolled out
```
Finally, we can use `describe` again. We can see that the container image has changed
```bash
kubectl describe deployment nginx-deployment
# Output
Name:                   nginx-deployment
Namespace:              default
...
  Containers:
   nginx-container:
    Image:         nginx:1.16
...
```
