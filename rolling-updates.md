## Create Deployment

Recreate the nginx deployment that we did earlier:

```shell
$ kubectl create deployment nginx --image=nginx:1.7.9
```

And expose the pod using a load balancer service

```shell
$ kubectl expose deployment nginx --port 80 --type LoadBalancer
```

Note down the loadbalancer IP from the services command:

```shell
$ kubectl get service
```

Increase the replicas to four:

```shell
$ kubectl scale deployment nginx --replicas=4
```


## Update Deployment

Rollout an update to  the image:

```shell
$ kubectl set image deployment nginx nginx=nginx:1.9.1 --record
```

Check the rollout status:

```shell
$ kubectl rollout status deployment nginx
```

checkout  rollout history:

```shell
$ kubectl rollout history deployment nginx
```

Try rolling out other image version by repeating the `set image` command from
above.  Suggested image versions are 1.12.2, 1.13.12, 1.14.1, 1.15.2.

Try also rolling out a version that does not exist:

```shell
$ kubectl set image deployment nginx nginx=nginx:100.200.300 --record
```

```shell
(axial-method-253004)$ kubectl get pods -w
NAME                     READY   STATUS         RESTARTS   AGE
nginx-5fc6bc8999-hq9kz   0/1     ErrImagePull   0          33s
nginx-5fc6bc8999-srsjw   0/1     ErrImagePull   0          33s
nginx-7df4cb8544-lkg85   1/1     Running        0          4m29s
nginx-7df4cb8544-qvjz4   1/1     Running        0          4m37s
nginx-7df4cb8544-s6krl   1/1     Running        0          4m38s
nginx-5fc6bc8999-srsjw   0/1   ImagePullBackOff   0     43s
nginx-5fc6bc8999-hq9kz   0/1   ImagePullBackOff   0     44s
nginx-5fc6bc8999-srsjw   0/1   ErrImagePull   0     56s
nginx-5fc6bc8999-hq9kz   0/1   ErrImagePull   0     57s
nginx-5fc6bc8999-srsjw   0/1   ImagePullBackOff   0     70s
nginx-5fc6bc8999-hq9kz   0/1   ImagePullBackOff   0     72s
nginx-5fc6bc8999-srsjw   0/1   ErrImagePull   0     109s
nginx-5fc6bc8999-hq9kz   0/1   ErrImagePull   0     111s
nginx-5fc6bc8999-srsjw   0/1   ImagePullBackOff   0     2m4s
nginx-5fc6bc8999-hq9kz   0/1   ImagePullBackOff   0     2m4s

```

## Undo Update

The rollout above using a non-existing image version caused some pods to be
non-functioning. Next, we will undo this faulty deployment. First, investigate
rollout history:

```shell
$ kubectl rollout history deployment nginx
```

Undo the rollout and restore the previous version:

```shell
$ kubectl rollout undo deployment nginx
```

Investigate the running pods:

```shell
$ kubectl get pods
```

## Clean up

Delete deployments and services as follow:

```shell
$ kubectl delete deployment nginx
$ kubectl delete service nginx
```

