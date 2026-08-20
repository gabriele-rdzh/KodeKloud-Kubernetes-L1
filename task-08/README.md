# Task 08: Schedule Cronjobs in Kubernetes

## Objective

The Nautilus DevOps team is setting up recurring tasks on different schedules. Currently, they're developing scripts to be executed periodically. To kickstart the process, they're creating cron jobs in the Kubernetes cluster with placeholder commands. Follow the instructions below:



Create a cronjob named `xfusion`.


Set Its schedule to something like `*/10 * * * *`. You can set any schedule for now.


Name the container `cron-xfusion`.


Utilize the `httpd` image with `latest tag` (specify as `httpd:latest`).


Execute the dummy command `echo Welcome to xfusioncorp!`.


Ensure the restart policy is `OnFailure`.

## Solution

For this task, we're going to change the `kind` and a few other details(the ones listed above :u) in the YAML
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: xfusion
spec:
  schedule: "*/10 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: cron-xfusion
            image: httpd:latest
            command: ["echo", "Welcome to xfusioncorp!"]
          restartPolicy: OnFailure
```
with the YAML file created, we can deploy the cronjob. the cronjob will create a pod every 10 minutes, so to verify that the pod has been created and that it's running the command `echo Welcome to xfusioncorp!`, we'll have to wait.
```bash
kubectl apply -f cronjob.yaml
# Output
cronjob.batch/xfusion created
```
```bash
kubectl get cronjob xfusion
# Output
NAME      SCHEDULE       TIMEZONE   SUSPEND   ACTIVE   LAST SCHEDULE   AGE
xfusion   */10 * * * *   <none>     False     0        <none>          17s
```
```bash
kubectl get pods
# Output
No resources found in default namespace.
```
As we can see, there's nothing ther so...

*One Eternity Later*
```bash
kubectl get cronjob xfusion
# Output
NAME      SCHEDULE       TIMEZONE   SUSPEND   ACTIVE   LAST SCHEDULE   AGE
xfusion   */10 * * * *   <none>     False     0        4m48s           10m
```
```bash
kubectl get pods
# Output
NAME                     READY   STATUS      RESTARTS   AGE
xfusion-29786810-s5tb4   0/1     Completed   0          4m53s
```
Now we can see that the pod was created; we can view the logs.
```bash
kubectl logs xfusion-29786810-s5tb4
# Output
Welcome to xfusioncorp!
```
