

To create and view the secrets 

```shell
kubectl create secret tls nginx-certs --cert=tls.crt --key=tls.key
kubectl get secrets
```

Create the configmaps
```shell
kubectl create configmap nginx-config --from-file=nginx-custom.conf
```
To view configmaps
```shell
kubectl get configmaps
```

to get the both secrets and configmaps
```shell
kubectl get secrets,configmaps
```
to deploy nginx with ssl mode 
```shell
kubectl create -f file/nginx-ssl.yml
```

```shell
kubectl get pods -w 

kubectl describe pod <pod name>

kubectl exec -it <pod name> /bin/sh

```

test your websites with 443 
 expose your deployment with Nodeport and test your set up 




