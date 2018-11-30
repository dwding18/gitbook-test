# Installation

With an introduction to the core concepts in the previous sections, lets move onto setting up K-Atlas and using it.

## Install All-in-one

Install everything to have a ready-to-use setup of K-Atlas:

```text
$ kubectl create -f https://raw.githubusercontent.com/dgraph-io/dgraph/master/contrib/config/kubernetes/dgraph-ha.yaml
$ kubectl create -f deploy/k-atlas.yaml
$ kubectl create -f deploy/collector.yaml
```

Point your browser to following URL to start using K-Atlas:

```text
http://ingress/
```

Or you can install individual components using below instructions.

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

### Install K-Atlas API

{% hint style="info" %}
 K-Atlas service will be deployed to a Kubernetes cluster
{% endhint %}

```text
$ kubectl create -f deploy/k-atlas.yaml
```

Output:

```bash
deployment "cutlass-api" created
service "cutlass-api" created
```

### Install K-Atlas Kubernetes Collector

```text
$ kubectl create -f deploy/collector.yaml
```

### Install K-Atlas Browser

```text
$ kubectl create -f deploy/browser.yaml
```



