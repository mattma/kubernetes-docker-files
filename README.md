# Kubernetes Docker Files

Build Docker images from upstream binaries

## etcd

```
sudo docker run --detach --net=host --name=etcd \
--volume=/usr/share/ca-certificates/:/etc/ssl/certs \
quay.io/coreos/etcd:v2.0.11
```

## kubelet

```
docker build -t quay.io/kelseyhightower/kubelet:0.19.0 .
```

```
sudo docker run --detach --net=host --name=kubelet \
--volume=/var/run/docker.sock:/var/run/docker.sock \
--volume=/etc/kubernetes/manifests:/etc/kubernetes/manifests \
--volume=/etc/machine-id:/etc/machine-id \
quay.io/kelseyhightower/kubelet:0.19.0 \
--address=0.0.0.0 \
--api-servers=http://localhost:8080 \
--boot-id-file=/etc/kubernetes/boot-id \
--enable_server \
--hostname-override=127.0.0.1 \
--config=/etc/kubernetes/manifests \
--v=2
```

## kube-apiserver

```
docker build -t quay.io/kelseyhightower/kube-apiserver:0.19.0 .
```

```
sudo docker run --detach --net=host --name=kube-apiserver \
quay.io/kelseyhightower/kube-apiserver:0.19.0 \
--etcd-servers=http://127.0.0.1:2379 \
--insecure-bind-address=0.0.0.0 \
--insecure-port=8080 \
--logtostderr=true \
--service-cluster-ip-range=10.200.20.0/24 \
--v=2
```

## kube-controller-manager

```
docker build -t quay.io/kelseyhightower/kube-controller-manager:0.19.0 .
```

```
sudo docker run --detach --net=host --name=kube-proxy --privileged \
quay.io/kelseyhightower/kube-controller-manager:0.19.0 \
--logtostderr=true \
--master=http://127.0.0.1:8080 \
--v=2
```

## kube-proxy

```
docker build -t quay.io/kelseyhightower/kube-proxy:0.19.0 .
```

```
sudo docker run --detach --net=host --name=kube-proxy --privileged \
quay.io/kelseyhightower/kube-proxy:0.19.0 \
--logtostderr=true \
--master=http://127.0.0.1:8080 \
--v=2
```

## kube-scheduler

```
docker build -t quay.io/kelseyhightower/kube-scheduler:0.19.0 .
```

```
sudo docker run --detach --net=host --name=kube-scheduler \
quay.io/kelseyhightower/kube-scheduler:0.19.0 \
--logtostderr=true \
--master=http://127.0.0.1:8080 \
--v=2
```
