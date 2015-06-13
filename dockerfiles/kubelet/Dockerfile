FROM debian:jessie

ADD kubelet /usr/bin/kubelet
RUN chmod 755 /usr/bin/kubelet

ENTRYPOINT ["/usr/bin/kubelet"]
