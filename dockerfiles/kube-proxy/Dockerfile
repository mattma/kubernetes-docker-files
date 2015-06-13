FROM debian:jessie

ADD kube-proxy /usr/bin/kube-proxy
RUN chmod 755 /usr/bin/kube-proxy

ENTRYPOINT ["/usr/bin/kube-proxy"]
