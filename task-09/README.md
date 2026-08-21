# Task 09: Create Countdown Job in Kubernetes

## Objective

The Nautilus DevOps team is crafting jobs in the Kubernetes cluster. While they're developing actual scripts/commands, they're currently setting up templates and testing jobs with dummy commands. Please create a job template as per details given below:


1. Create a job named `countdown-xfusion`.

2. The spec template should be named `countdown-xfusion` (under metadata), and the container should be named `container-countdown-xfusion`

3. Utilize image `debian` with `latest` tag (ensure to specify as `debian:latest`), and set the restart policy to `Never`.

4. Execute the command `sleep 5`

## Solution
For this task, we're going to wwrite the YAML file as follows. As you can see, we've changed the `kind` to `job`, and in the `containers` sectionwe've added the `command`
```YAML
apiVersion: batch/v1
kind: Job
metadata:
  name: countdown-xfusion
spec:
  template:
    metadata:
      name: countdown-xfusion
    spec:
      containers:
      - name: container-countdown-xfusion
        image: debian:latest
        command: ["sleep", "5"]
      restartPolicy: Never
```

Now let's create the job using `apply`
```bash
kubectl apply -f job.yaml
# Output
job.batch/countdown-xfusion created
```
Finally check for the job
```bash
kubectl get jobs
# Output
NAME                STATUS     COMPLETIONS   DURATION   AGE
countdown-xfusion   Complete   1/1           12s        23s
```
