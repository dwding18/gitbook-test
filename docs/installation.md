# Installation

With an introduction to the core concepts in the previous sections, lets move onto setting up K-Atlas and using it.

## Install All-in-one

Install everything to have a ready-to-use setup of K-Atlas:

```text
$ kubectl create -f https://raw.githubusercontent.com/dgraph-io/dgraph/master/contrib/config/kubernetes/dgraph-ha.yaml
$ kubectl create -f deploy/dgraph-schema.yaml

$ kubectl create -f deploy/k-atlas.yaml
$ kubectl expose deployment cmdb-api --type=NodePort

$ kubectl create -f deploy/collector.yaml

$ kubectl create -f deploy/browser.yaml
```

Check all pods are up

```text
$ kubectl get pods
NAME                             READY   STATUS        RESTARTS   AGE
cmdb-api-576bd4b976-gq5lq        1/1     Running       0          2m
cmk-controller-794f79476-bpst8   1/1     Running       0          5s
dgraph-alpha-0                   1/1     Running       3          1d
dgraph-alpha-1                   1/1     Running       3          1d
dgraph-alpha-2                   1/1     Running       3          1d
dgraph-ratel-7b7bc97649-46nw5    1/1     Running       4          1d
dgraph-zero-0                    1/1     Running       3          1d
dgraph-zero-1                    1/1     Running       3          1d
dgraph-zero-2                    1/1     Running       3          1d
```

Check all Services are running

```text
INTUL16837867a:~ sshroff3$ kubectl get services
NAME                  TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                         AGE
cmdb-api              NodePort       10.99.31.122    <none>        8011:30081/TCP                  14m
dgraph-alpha          ClusterIP      None            <none>        7080/TCP                        1d
dgraph-alpha-public   LoadBalancer   10.101.113.42   <pending>     8080:30620/TCP,9080:31265/TCP   1d
dgraph-ratel-public   LoadBalancer   10.109.199.98   <pending>     8000:32116/TCP                  1d
dgraph-zero           ClusterIP      None            <none>        5080/TCP                        1d
dgraph-zero-public    LoadBalancer   10.102.1.152    <pending>     5080:31480/TCP,6080:32583/TCP   1d                    1d
```

Point your browser to following URL to start using K-Atlas:

```text
http://ingress/
```

Or you can install individual components using below instructions. 

The individual components will be deployed in Kubernetes clusters. To run locally, please see the Section [Run K-Atlas locally](run-k-atlas-locally.md).

### Install DGraph

DGraph is the data store that Culass uses to persist node data. It is a fairly straight forward process to install DGraph into Kubernetes:

```bash
$ kubectl create -f https://raw.githubusercontent.com/dgraph-io/dgraph/master/contrib/config/kubernetes/dgraph-ha.yaml
```

Output:

```bash
service "dgraph-zero-public" created
service "dgraph-alpha-public" created
service "dgraph-alpha-0-http-public" created
service "dgraph-ratel-public" created
service "dgraph-zero" created
service "dgraph-alpha" created
statefulset "dgraph-zero" created
statefulset "dgraph-alpha" created
deployment "dgraph-ratel" created
```

### Setup Dgraph Schema

TODO- we will have a script/code for this

### Install K-Atlas API

```text
$ kubectl create -f deploy/k-atlas.yaml
deployment.apps/cmdb-api created

$ kubectl expose deployment cmdb-api --type=NodePort
service/cmdb-api exposed
```

### Install K-Atlas Kubernetes Collector

```text
$ kubectl create -f deploy/collector.yaml
role.rbac.authorization.k8s.io/controller-reader created
serviceaccount/cmk-controller created
clusterrolebinding.rbac.authorization.k8s.io/cmk-controller created
deployment.extensions/cmk-controller created
```

### Install K-Atlas Browser

```text
$ kubectl create -f deploy/browser.yaml
```



