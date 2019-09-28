
 Create your pod from the file 
 
 ```shell
 kubectl create/apply -f files/nginx-deployment.yml
 
 deployment.extensions/nginx created
```
Verify that the deployment is created:

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

```shell
kubectl run tmp-shell --generator=run-pod/v1 --rm -i --tty --overrides='{"spec": {"hostNetwork": true}}' --image nicolaka/netshoot -- /bin/bash - to run a network pod to connect to different pod 
```
