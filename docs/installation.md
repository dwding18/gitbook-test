# Installation

With an introduction to the core concepts in the previous sections, lets move onto setting up K-Atlas on a Kubernetes cluster and using it.

## Prerequisites

{% hint style="info" %}
We will assume a Kubernetes cluster with Minikube is being used. If using another Kubernetes cluster the Installation yaml files will need to be modified to use the correct cluster and namespace.
{% endhint %}

1. Install VirtualBox \(Required for Minikube\)

```text
$ vboxmanage --version
5.2.22r126460
```

{% embed url="https://www.virtualbox.org/wiki/Downloads" %}

2. Install Minikube

```text
minikube version
minikube version: v0.30.0
```

{% embed url="https://github.com/kubernetes/minikube" %}

3. Note that cluster "minikube"  and namespace "default" exists after the above steps. The yaml files used in the Installation use this cluster and namespace.

```text
$ kubectl config get-clusters
NAME
minikube
 
$ kubectl get namespace
NAME          STATUS   AGE
default       Active   11m
kube-public   Active   11m
kube-system   Active   11m
 
```

## Install All-in-one

Install everything to have a ready-to-use setup of K-Atlas:

```text
$ kubectl create -f https://raw.githubusercontent.com/dgraph-io/dgraph/master/contrib/config/kubernetes/dgraph-ha.yaml
$ kubectl create -f deploy/dgraph-schema.yaml

$ kubectl create -f deploy/cutlass-api.yaml
$ kubectl expose deployment cutlass-api --type=NodePort

$ kubectl create -f deploy/cutlass-collector.yaml

$ kubectl create -f deploy/cutlass-browser.yaml
```

Check all pods are up

```text
$ kubectl get pods
NAME                                 READY   STATUS    RESTARTS   AGE
cutlass-api-684cc89fdd-h7bll         1/1     Running   0          4h
cutlass-controller-878b7b8ff-jsq8x   1/1     Running   0          28s
dgraph-alpha-0                       1/1     Running   0          1d
dgraph-alpha-1                       1/1     Running   0          1d
dgraph-alpha-2                       1/1     Running   0          1d
dgraph-ratel-7b7bc97649-46nw5        1/1     Running   0          1d
dgraph-zero-0                        1/1     Running   0          1d
dgraph-zero-1                        1/1     Running   0          1d
dgraph-zero-2                        1/1     Running   0          1d

```

Check all Services are running

```text
INTUL16837867a:~ sshroff3$ kubectl get services
NAME                  TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                         AGE
cutlass-api           NodePort       10.105.111.199   <none>        8011:31759/TCP                  4h
dgraph-alpha          ClusterIP      None             <none>        7080/TCP                        1d
dgraph-alpha-public   LoadBalancer   10.101.113.42    <pending>     8080:30620/TCP,9080:31265/TCP   1d
dgraph-ratel-public   LoadBalancer   10.109.199.98    <pending>     8000:32116/TCP                  1d
dgraph-zero           ClusterIP      None             <none>        5080/TCP                        1d
dgraph-zero-public    LoadBalancer   10.102.1.152     <pending>     5080:31480/TCP,6080:32583/TCP   1d
```

Point your browser to the following URL to start using K-Atlas:

```text
http://ingress/
```

Or you can install individual components using below instructions. 

The individual components must be installed in the below order-

* Dgraph
* K-Atlas API
* K-Atlas Kubernetes Collector
* K-Atlas Browser

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
$ kubectl create -f deploy/cutlass-api.yaml
deployment.apps/cutlass-api created

$ kubectl expose deployment cutlass-api --type=NodePort
service/cutlass-api exposed
```

### Install K-Atlas Kubernetes Collector

```text
$ kubectl create -f deploy/cutlass-collector.yaml
role.rbac.authorization.k8s.io/controller-reader created
serviceaccount/cutlass-controller created
clusterrolebinding.rbac.authorization.k8s.io/cutlass-controller created
deployment.extensions/cutlass-controller created
```

### Install K-Atlas Browser

```text
$ kubectl create -f deploy/cutlass-browser.yaml
```

For issues with Installation, please check the [FAQ](faq.md) Section.

