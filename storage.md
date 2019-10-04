# Kubernetes storage (PV and PVC):

List the available storage classes:

```shell
kubectl get storageclass
```

Create a claim for a dynamically provisioned volume (PVC) for nginx. 

```shell
$ kubectl create -f files/pvc-nginx.yaml
```

Check that the PVC exists and is bound:

```shell
$ kubectl get pvc
```

Example:

```shell
$ kubectl get pvc
```

check for persistent volume (PV) against this PVC:

```shell
$ kubectl get pv
```

We are going to use the file `files/nginx-persistent-storage.yaml` file to use storage/volume directives:

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      volumes:
      - name: nginx-htmldir-volume
        persistentVolumeClaim:
          claimName: pvc-nginx
      containers:
      - name: nginx
        image: nginx:1.9.1
        ports:
        - containerPort: 443
        - containerPort: 80
        volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: nginx-htmldir-volume
```

Deploy nginx again. 

```shell
$ kubectl create -f support-files/nginx-persistent-storage.yaml
```

After it starts, you may want to examine it by using `kubectl describe pod nginx` and look out for volume declarations.

Now, you access the Nginx instance using curl. You should get a "403 Forbidden", because now the volume you mounted is empty.


Create a file in the htmldir inside the nginx pod and add some text in it:

```shell
$ kubectl exec -it nginx-deployment-6665c87fd8-cc8k9 -- bash

root@nginx-deployment-6665c87fd8-cc8k9:/# echo "<h1>Welcome to Nginx</h1>This is Nginx with html directory mounted as a volume from GCP."  > /usr/share/nginx/html/index.html
root@nginx-deployment-6665c87fd8-cc8k9:/#
```

Exit the nginx pod again. From the netshoot container, run curl again, you should see the web page:

```shell
$ kubectl run tmp-shell --generator=run-pod/v1 --rm -i --tty --overrides='{"spec": {"hostNetwork": true}}' --image nicolaka/netshoot -- /bin/bash

bash-4.4#
 curl 10.0.96.7
<h1>Welcome to Nginx</h1>This is Nginx with html directory mounted as a volume from GCE Storage.
bash-4.4#
```

Kill the pod:

```shell
$ kubectl delete pod nginx-deployment-6665c87fd8-cc8k9
```

Check if it is up

```shell
$ kubectl get pods -o wide
```


## Clean up

```shell
$ kubectl delete deployment nginx-deployment
$ kubectl delete service nginx-deployment
$ kubectl delete pvc pvc-nginx
```


Any storage that the containers use inside the pod is ephemeral by nature. They get removed automatically with every pod restart.
To address this problem, Kuberenetes provides Volumes. In their purest state, a volume can be a cloud disk that you create, a fiber channel LUN, or even a local disk on the node.
You can also decouple storage creation and administration from pod management by using Persistent Volumes and Persistent Volume Claims. This model uses the same volume types that Kubernetes supports.
Dynamic Volumes allow you to avoid having to create Persistent Volumes before using Persistent Volume Claims. All you need to do is create a Storage Class with the appropriate backend storage type and capability. The Persistent Volume Claim uses the suitable Storage Class directly without having to know the volume name.
