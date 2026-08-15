# Task 04: Set Resource Limits in Kubernetes Pods

## Objective

The Nautilus DevOps team has noticed performance issues in some Kubernetes-hosted applications due to resource constraints. To address this, they plan to set limits on resource utilization. Here are the details:


Create a pod named `httpd-pod` with a container named `httpd-container`. Use the `httpd` image with the `latest` tag (specify as `httpd:latest`). Configure the following container-level resource requests and limits for the container:

Requests: Memory: `15Mi`, CPU: `100m`

Limits: Memory: `20Mi`, CPU: `100m`

## Solution

To create the pod with limited resources, we first need to create the YAML file. Within the YAML file, we can specify the resources that are required.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
  - name: httpd-container
    image: httpd:latest
    resources:
      requests:
        memory: "15Mi"
        cpu: "100m"
      limits:
        memory: "20Mi"
        cpu: "100m"
```

With the YAML file created using the specifications we need, we proceed to create the pod.

```bash
kubectl apply -f httpd-pod.yaml
# Output
pod/httpd-pod created
```
Finally, we can check the pod's status using `get pod` and `describe` to verify that the resources are limited.

```bash
kubectl get pod httpd-pod
# Output
NAME        READY   STATUS    RESTARTS   AGE
httpd-pod   1/1     Running   0          41s
```


```bash
kubectl describe pod httpd-pod
# Output (relevant section)
Containers:
  httpd-container:
    ...
    Limits:
      cpu:     100m
      memory:  20Mi
    Requests:
      cpu:        100m
      memory:     15Mi
```
