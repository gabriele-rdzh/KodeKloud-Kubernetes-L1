# Task 07: Deploy ReplicaSet in Kubernetes Cluster

## Objective
The Nautilus DevOps team is gearing up to deploy applications on a Kubernetes cluster for migration purposes. A team member has been tasked with creating a ReplicaSet outlined below:



Create a ReplicaSet using `httpd` image with `latest` tag (ensure to specify as `httpd:latest`) and name it `httpd-replicaset`.


Apply labels: `app` as `httpd_app`, `type` as `front-end`.


Name the container `httpd-container`. Ensure the replica count is `4`.

## Solution
for this task, we're going to set up our YAML as follows. As you can see, one od the main changes is that `kind` is now `ReplicaSet`, and we hace a section called `spec` that specifies the number of replicas we need.
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: httpd-replicaset
  labels:
    app: httpd_app
    type: front-end
spec:
  replicas: 4
  selector:
    matchLabels:
      app: httpd_app
      type: front-end
  template:
    metadata:
      labels:
        app: httpd_app
        type: front-end
    spec:
      containers:
      - name: httpd-container
        image: httpd:latest
```

Now we just need to create the replicaset
```bash        
kubectl apply -f httpd-replicaset.yaml
# Output
replicaset.apps/httpd-replicaset created
```

We can check that the configured replicas are running
```bash
kubectl get replicaset httpd-replicaset
# Output
NAME               DESIRED   CURRENT   READY   AGE
httpd-replicaset   4         4         4       33s
```

and the pods too
```bash
kubectl get pods -l app=httpd_app
# Output
NAME                     READY   STATUS    RESTARTS   AGE
httpd-replicaset-bqqkt   1/1     Running   0          62s
httpd-replicaset-mjgh2   1/1     Running   0          62s
httpd-replicaset-n9b8n   1/1     Running   0          62s
httpd-replicaset-wnzr5   1/1     Running   0          62s
```
