# Installation

With an introduction to the core concepts in the previous section, lets move onto setting up K-Atlas and using it.

## Install DGraph

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

## Install K-Atlas

{% hint style="info" %}
 K-Atlas service will be deployed to a Kubernetes cluster
{% endhint %}

```text
$ kubectl create -f deploy/cutlass.yaml
```

Output:

```bash
deployment "cutlass-api" created
service "cutlass-api" created
ingress "cutlass-api" created
```

## Install K-Atlas Kubernetes Collector

```text
$ kubectl create -f deploy/collector.yaml
```



