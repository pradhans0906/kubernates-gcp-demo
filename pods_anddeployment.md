
 Create your pod from the file

 ```shell
 kubectl create/apply -f files/nginx-deployment.yml

 deployment.extensions/nginx created
```
Verify that the deployment is created:
```shell
 kubectl get deployments
NAME        DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
nginx       1         1         1            1           36s
```
verify pods

```shell
 kubectl get pods
 kubectl get pods -w
```

## Clean up

Delete the `nginx` deployment:
```shell
$ kubectl delete deployment nginx
```
## Creating a standalone pod:
```shell
apiVersion: v1
kind: Pod
metadata:
  name: standalone-nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:alpine
```
Save the above few lines of code as a yaml file, and use kubectl create -f <filename> to create this pod.
```shell
$ kubectl create -f files/standalone-nginx-pod.yaml
pod "standalone-nginx-pod" created

$ kubectl get pods
NAME                   READY     STATUS    RESTARTS   AGE
standalone-nginx-pod   1/1       Running   0          4s
$
The example above will work with container images, which have some sort of daemon/service process running as their entrypoint. If you want to run something which does not have a service process in the container image, you can pass it a custom command, such as shown below:
apiVersion: v1
kind: Pod
metadata:
  name: standalone-busybox-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command: ['sh', '-c', 'echo Hello Kubernetes! && sleep 3600']
```
The above code will create a pod, which will go into a sleep for 3600 seconds ( one hour), and will exit (die) dilently. Good to run certain diagnostics.
```shell
$ kubectl create -f files/standalone-busybox-pod.yaml
pod "standalone-busybox-pod" created
$

$ kubectl get pods
NAME                     READY     STATUS    RESTARTS   AGE
standalone-busybox-pod   1/1       Running   0          30s
$
```

## This image has all the networking tools

```shell
kubectl run tmp-shell --rm -i --tty --image nicolaka/netshoot -- /bin/bash - to run a network pod to connect to different pod
```
