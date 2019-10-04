check files/pv.yml

The definition creates a PV that offers 5 GB of space. Notice that storageClassName parameter, which is one of the ways a Persistent Volume Claim can find the matching Persistent Volume.

```shell
kubectl apply -f pv.yaml
```

Create Persistent Volume Claim that our pod can access:

Use the pvc.yml and if you look the defination ,  PVC is searching for a PV with storageClassName “manual” and it is requesting a 1 GB storage from this volume.

 Apply this configuration to the cluster using: 

```shell
kubectl apply -f pvc.yaml
```

Both PV and PVC created and please get it verified with below command 

```shell
kubectl get pvc

```


