FROM scratch
MAINTAINER Matt Ma "matt@mattmadesign.com"
ENV REFRESHED_AT 2015-08-10

COPY kubelet kubelet
VOLUME /etc/os-release /var/lib/docker /var/lib/kubelet /var/run /sys /etc/kubernetes /etc/ssl/certs /rootfs /nsenter /usr /lib64
ENTRYPOINT ["/kubelet"]
