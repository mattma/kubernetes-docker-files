FROM debian:jessie

ADD kube-controller-manager /usr/bin/kube-controller-manager
RUN chmod 755 /usr/bin/kube-controller-manager

ENTRYPOINT ["/usr/bin/kube-controller-manager"]
