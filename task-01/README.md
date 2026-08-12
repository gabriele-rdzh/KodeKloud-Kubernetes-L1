# Task 01: Deploy Pods in Kubernetes Cluster

## Objective
The Nautilus DevOps team is diving into Kubernetes for application management. One team member has a task to create a pod according to the details below:


1. Create a pod named `pod-nginx` using the `nginx` image with the `latest` tag. Ensure to specify the tag as `nginx:latest`.

2. Set the `app` label to `nginx_app`, and name the container as `nginx-container`.

## Solution
To solve this task we can create a yaml file.

```bash
vi pod.yaml
```
Setting the right configuration.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-nginx
  labels:
    app: nginx_app
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
```
Then, use `apply` to make the pod.

```bash
kubectl apply -f pod.yaml
# output
pod/pod-nginx created
```
Now verify the pod status using `get pods`

```bash
kubectl get pods
# Output
NAME        READY   STATUS    RESTARTS   AGE
pod-nginx   1/1     Running   0          2m10s
```
## Key Learnings
We can use the yaml file as a template for future tasks.
We can also create a yaml file from the terminal using the following command

```bash
kubectl run pod-nginx --image=nginx:latest --labels="app=nginx_app" --dry-run=client -o yaml > pod.yaml
```
Now we just need to change the container name and delete some things we don’t need right now.

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: nginx_app
  name: pod-nginx
spec:
  containers:
  - image: nginx:latest
    name: pod-nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
and the just use `apply` like before :D
