# Task 05: Execute Rolling Updates in Kubernetes

## Objective

An application currently running on the Kubernetes cluster employs the nginx web server. The Nautilus application development team has introduced some recent changes that need deployment. They've crafted an image `nginx:1.17` with the latest updates.


Execute a rolling update for this application, integrating the `nginx:1.17` image. The deployment is named `nginx-deployment`.

Ensure all pods are operational post-update.

## Solution
To update the application, we first need to know the name of the container. To do this, we can us `describe`.

```bash
kubectl describe deployment nginx-deployment
# Output (relevant section)
Name:                   nginx-deployment
...
StrategyType:           RollingUpdate
...
  Labels:  app=nginx-app
  Containers:
   nginx-container:
    Image:         nginx:1.16
    ...
```
Now that we have the name, we can set the updated image as follows

```bash
kubectl set image deployment/nginx-deployment nginx-container=nginx:1.17
# Output
deployment.apps/nginx-deployment image updated
```
We can check the status of the update
```bash
kubectl rollout status deployment/nginx-deployment
# Output
deployment "nginx-deployment" successfully rolled out
```
Finally, we verify that the pods are running. We can also verify that the container image has changed.
```bash
kubectl get pods
# Output
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-544f9896c8-6hrk5   1/1     Running   0          3m28s
nginx-deployment-544f9896c8-dk89h   1/1     Running   0          3m24s
nginx-deployment-544f9896c8-s9x8j   1/1     Running   0          3m23s
```
```bash
kubectl describe deployment nginx-deployment
# Output (relevant section)
Name:                   nginx-deployment
...
StrategyType:           RollingUpdate
...
  Containers:
   nginx-container:
    Image:         nginx:1.17
    ...
# we can see here how the pods were updated
OldReplicaSets:  nginx-deployment-fc677cbc9 (0/0 replicas created)
NewReplicaSet:   nginx-deployment-544f9896c8 (3/3 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  14m    deployment-controller  Scaled up replica set nginx-deployment-fc677cbc9 from 0 to 3
  Normal  ScalingReplicaSet  4m     deployment-controller  Scaled up replica set nginx-deployment-544f9896c8 from 0 to 1
  Normal  ScalingReplicaSet  3m56s  deployment-controller  Scaled down replica set nginx-deployment-fc677cbc9 from 3 to 2
  Normal  ScalingReplicaSet  3m56s  deployment-controller  Scaled up replica set nginx-deployment-544f9896c8 from 1 to 2
  Normal  ScalingReplicaSet  3m55s  deployment-controller  Scaled down replica set nginx-deployment-fc677cbc9 from 2 to 1
  Normal  ScalingReplicaSet  3m55s  deployment-controller  Scaled up replica set nginx-deployment-544f9896c8 from 2 to 3
  Normal  ScalingReplicaSet  3m54s  deployment-controller  Scaled down replica set nginx-deployment-fc677cbc9 from 1 to 0
```
