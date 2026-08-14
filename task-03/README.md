# Task 03: Setup Kubernetes Namespaces and PODs

## Objective

The Nautilus DevOps team is planning to deploy some micro services on Kubernetes platform. The team has already set up a Kubernetes cluster and now they want to set up some namespaces, deployments etc. Based on the current requirements, the team has shared some details as below:


Create a namespace named `dev` and deploy a POD within it. Name the pod `dev-nginx-pod` and use the `nginx` image with the `latest` tag. Ensure to specify the tag as `nginx:latest`.

## Solution

First, we need to create the namespace.

```bash
kubectl create namespace dev
# Output
namespace/dev created
```
Next, we create the pod within the namespace using the `nginx:latest` image

```bash
kubectl run dev-nginx-pod --image=nginx:latest --namespace=dev
# Output
pod/dev-nginx-pod created
```
Finally, to make sure that the task is complete, we can check the status of the pod; if it's running, we're done
```bash
kubectl get pods -n dev
# Output
NAME            READY   STATUS    RESTARTS   AGE
dev-nginx-pod   1/1     Running   0          82s
```
