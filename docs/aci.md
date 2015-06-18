# ACIs

The docker images produced by these docker files can be converted to ACIs.

## Convert Docker images to ACI using docker2aci

After running the commands in this section you will have the following ACIs:

```
kelseyhightower-kube-apiserver-0.19.0.aci
kelseyhightower-kube-controller-manager-0.19.0.aci
kelseyhightower-kube-proxy-0.19.0.aci
kelseyhightower-kube-scheduler-0.19.0.aci
kelseyhightower-kubelet-0.19.0.aci
```

### quay.io/kelseyhightower/kubelet:0.19.0

```
docker2aci docker://quay.io/kelseyhightower/kubelet:0.19.0
```

```
docker2aci docker://quay.io/kelseyhightower/kubelet:0.19.0
Downloading c6b09d8961e4: [====================================] 32 B/32 B
Downloading 57858edd3470: [====================================] 7.92 MB/7.92 MB
Downloading 312fd182bf0d: [====================================] 32 B/32 B
Downloading 3620fdd80548: [====================================] 32 B/32 B

Converted volumes:
	name: "volume-sys", path: "/sys", readOnly: false
	name: "volume-var-lib-docker", path: "/var/lib/docker", readOnly: false
	name: "volume-var-run", path: "/var/run", readOnly: false
	name: "volume-etc-kubernetes", path: "/etc/kubernetes", readOnly: false
	name: "volume-etc-os-release", path: "/etc/os-release", readOnly: false
	name: "volume-etc-ssl-certs", path: "/etc/ssl/certs", readOnly: false
	name: "volume-lib64", path: "/lib64", readOnly: false
	name: "volume-nsenter", path: "/nsenter", readOnly: false
	name: "volume-rootfs", path: "/rootfs", readOnly: false
	name: "volume-usr", path: "/usr", readOnly: false
	name: "volume-var-lib-kubelet", path: "/var/lib/kubelet", readOnly: false

Generated ACI(s):
kelseyhightower-kubelet-0.19.0.aci
```

Repeat for the other images:

```
docker2aci docker://quay.io/kelseyhightower/kube-apiserver:0.19.0
```
```
docker2aci docker://quay.io/kelseyhightower/kube-controller-manager:0.19.0
```
```
docker2aci docker://quay.io/kelseyhightower/kube-proxy:0.19.0
```
```
docker2aci docker://quay.io/kelseyhightower/kube-scheduler:0.19.0
```

## Patch the ACI manifest using actool

After running the commands in this section you will have the following ACIs:

```
kube-apiserver-0.19.0-linux-amd64.aci
kube-controller-manager-0.19.0-linux-amd64.aci
kube-proxy-0.19.0-linux-amd64.aci
kube-scheduler-0.19.0-linux-amd64.aci
kubelet-0.19.0-linux-amd64.aci
```

### kube-apiserver

```
actool patch-manifest --name=kube-apiserver \
kelseyhightower-kube-apiserver-0.19.0.aci \
kube-apiserver-0.19.0-linux-amd64.aci
```

### kube-controller-manager 

```
actool patch-manifest --name=kube-controller-manager \
kelseyhightower-kube-controller-manager-0.19.0.aci \
kube-controller-manager-0.19.0-linux-amd64.aci
```

### kube-proxy

```
actool patch-manifest --name=kube-proxy --capability=CAP_NET_ADMIN \
kelseyhightower-kube-proxy-0.19.0.aci \
kube-proxy-0.19.0-linux-amd64.aci
```

### kube-scheduler

```
actool patch-manifest --name=kube-scheduler \
kelseyhightower-kube-scheduler-0.19.0.aci \
kube-scheduler-0.19.0-linux-amd64.aci
```

### kubelet

```
actool patch-manifest --name=kubelet --capability=CAP_NET_ADMIN \
--mounts="volume-resolv-conf,path=/etc/resolv.conf" \
kelseyhightower-kubelet-0.19.0.aci \
kubelet-0.19.0-linux-amd64.aci
```

Note: Docker binds in `/etc/resolv.conf` from the host. rkt does not, so we need to "patch" it in.

## Signing the ACIs

Export and upload the public key:

```
gpg --armor --export release@rktscience.io > pubkeys.gpg
```

Sign the images:

```
for aci in kube-apiserver kube-controller-manager kube-proxy kube-scheduler kubelet; do
  gpg -u release@rktscience.io --armor --detach-sig \
  --output ${aci}-0.19.0-linux-amd64.aci.asc \
  --detach-sig ${aci}-0.19.0-linux-amd64.aci
done
```

## Running the ACIs

Trust the rktscience signing key:

```
sudo rkt trust --root https://storage.googleapis.com/rktscience/pubkeys.gpg
```

```
Prefix: ""
Key: "https://storage.googleapis.com/rktscience/pubkeys.gpg"
GPG key fingerprint is: CDFF 0C6A EE50 D93A 5E71  A738 B6F7 807B 1EB4 DDAE
    Subkey fingerprint: 8FB7 603F 1238 E44C B127  6028 1F84 E96C 07B2 596F
	Rocket Science (ACI Builder) <release@rktscience.io>
Are you sure you want to trust this key (yes/no)? yes
Trusting "https://storage.googleapis.com/rktscience/pubkeys.gpg" for prefix "".
Added root key at "/etc/rkt/trustedkeys/root.d/cdff0c6aee50d93a5e71a738b6f7807b1eb4ddae"
```
