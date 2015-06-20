# Kubernetes Docker Files

Build Docker images from upstream binaries.

- [Build](#build)
- [Release](#release)
- [Run](#run)
  - [Docker CLI](#docker-cli)
  - [Docker Compose](#docker-compose)
- [Covert to ACIs](docs/aci.md)

## Build

Edit the `settings` file.

```
KUBERNETES_VERSION="0.19.1"
DOCKER_REGISTRY_HOST="quay.io"
DOCKER_REGISTRY_USERNAME="kelseyhightower"
```

Run the build script.

```
./build
```

## Release

Edit the `settings` file.

```
KUBERNETES_VERSION="0.19.1"
DOCKER_REGISTRY_HOST="quay.io"
DOCKER_REGISTRY_USERNAME="kelseyhightower"
```

Run the release script.

```
./release
```

## Run

### Docker CLI

Use the docker cli tool to bootstrap a single node Kubernetes cluster.

Start etcd.

```
sudo docker run --detach --net=host --privileged --name=etcd \
--restart=always \
--volume=/var/lib/etcd:/var/lib/etcd \
--volume=/usr/share/ca-certificates/:/etc/ssl/certs \
quay.io/coreos/etcd:v2.0.11 \
--advertise-client-urls http://127.0.0.1:2379 \
--data-dir /var/lib/etcd \
--listen-client-urls http://127.0.0.1:2379 \
--listen-peer-urls http://127.0.0.1:2380 \
--name etcd0
```

Start the Kubernetes API service.

```
sudo docker run --detach --net=host --name=kube-apiserver \
--restart=always \
--volume=/etc/kubernetes:/etc/kubernetes \
--volume=/usr/share/ca-certificates:/etc/ssl/certs \
--volume=/var/run/kubernetes:/var/run/kubernetes \
quay.io/kelseyhightower/kube-apiserver:0.19.1 \
--etcd-servers=http://127.0.0.1:2379 \
--insecure-bind-address=0.0.0.0 \
--insecure-port=8080 \
--logtostderr=true \
--service-cluster-ip-range=10.200.20.0/24 \
--v=2
```

Start the Kubernetes controller manager service.

```
sudo docker run --detach --net=host --name=kube-controller-manager \
--restart=always \
--volume=/etc/kubernetes:/etc/kubernetes \
--volume=/usr/share/ca-certificates:/etc/ssl/certs \
quay.io/kelseyhightower/kube-controller-manager:0.19.1 \
--logtostderr=true \
--master=http://127.0.0.1:8080 \
--v=2
```

Start the Kubernetes scheduler.

```
sudo docker run --detach --net=host --name=kube-scheduler \
--restart=always \
--volume=/etc/kubernetes:/etc/kubernetes \
--volume=/usr/share/ca-certificates/:/etc/ssl/certs \
quay.io/kelseyhightower/kube-scheduler:0.19.1 \
--logtostderr=true \
--master=http://127.0.0.1:8080 \
--v=2
```

Start the Kubernetes kubelet service.

```
sudo docker run --detach --net=host --name=kubelet --privileged \
--restart=always \
--volume=/usr/bin/nsenter:/nsenter \
--volume=/usr:/usr \
--volume=/lib64:/lib64 \
--volume=/:/rootfs:ro \
--volume=/etc/kubernetes:/etc/kubernetes \
--volume=/etc/os-release:/etc/os-release \
--volume=/usr/share/ca-certificates/:/etc/ssl/certs \
--volume=/sys:/sys:ro \
--volume=/var/lib/docker/:/var/lib/docker:ro \
--volume=/var/lib/kubelet/:/var/lib/kubelet:rw \
--volume=/var/run:/var/run:rw \
quay.io/kelseyhightower/kubelet:0.19.1 \
--address=0.0.0.0 \
--api-servers=http://127.0.0.1:8080 \
--containerized \
--enable-server \
--hostname-override=127.0.0.1 \
--machine-id-file=/rootfs/etc/machine-id \
--v=2
```

Start the Kubernetes proxy service.

```
sudo docker run --detach --net=host --name=kube-proxy --privileged \
--restart=always \
--volume=/etc/kubernetes:/etc/kubernetes \
--volume=/usr/share/ca-certificates:/etc/ssl/certs \
--volume=/usr:/usr \
--volume=/lib64:/lib64 \
quay.io/kelseyhightower/kube-proxy:0.19.1 \
--logtostderr=true \
--master=http://127.0.0.1:8080 \
--v=2
```

### Docker Compose

Use [docker-compose](https://github.com/docker/compose) to bootstrap a single node Kubernetes cluster.

```
git clone https://github.com/kelseyhightower/kubernetes-docker-files.git
cd kubernetes-docker-files
docker-compose up -d
```

## Testing

Download kubectl.

```
curl -o kubectl -L https://storage.googleapis.com/kubernetes-release/release/v0.19.1/bin/linux/amd64/kubectl
chmod +x kubectl
```

Test the kubectl client against the Kubernetes API server.

```
kubectl cluster-info
Kubernetes master is running at http://127.0.0.1:8080
```

Run some pods

```
kubectl run nginx --image=nginx
CONTROLLER   CONTAINER(S)   IMAGE(S)    SELECTOR        REPLICAS
nginx        nginx          nginx       run=nginx       1
```

```
kubectl get pods
NAME              READY     REASON    RESTARTS   AGE
nginx-6ipnq       1/1       Running   0          25s
```
