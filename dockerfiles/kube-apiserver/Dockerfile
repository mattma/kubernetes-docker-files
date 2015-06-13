FROM debian:jessie

ADD kube-apiserver /usr/bin/kube-apiserver
RUN chmod 755 /usr/bin/kube-apiserver

ENTRYPOINT ["/usr/bin/kube-apiserver"]
