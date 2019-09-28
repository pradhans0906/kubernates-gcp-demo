provides control for GKE access 

two types permission :

namespace specific,accross pods and nodes and targeting resource given namespace.

role and rolebinding :

A role is a collections permission rules 

A rolebinding connects user and role

Pod security :

cluster level resource managing access to create and update pods 

it is a set of condition :

 allows to run previliaged contaier 
 use host namespace and networking and filesystem 


Adavance securiry :

Cluster master ip rotation 

    master ip address is static , so this can be hacked 
    takes 7 days to complete and old ip addrss wil be not in use. 

    inititite ip rotaion :
```shell
   gcloud container clusters update crendentials <nameof the cluster> --start-ip-rotation
```

update new ip addrss for the client 

```shell
gcloud container cluster get crendentials <clustername> 

completing iprotation :

```shell
  gcloud container clusters update <clustername> --complete-ip-rotation
```

Credentials rotation for worker node :

  once initiated, two ip address used 
  nodes get updated crendentials and recreted 
  competes in 7days 

initiate crendentials
```shell
   gcloud container clusters update crendentials <nameof the cluster> --start-ip-rotation
   gcloud container cluster get crendentials <clustername>
   gcloud container clusters update <clustername> --complete-crendential-rotation
```
