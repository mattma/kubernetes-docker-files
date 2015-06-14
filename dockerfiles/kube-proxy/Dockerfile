FROM debian:jessie

RUN apt-get update && apt-get -y install iptables
ADD kube-proxy /usr/bin/kube-proxy
RUN chmod 755 /usr/bin/kube-proxy

ENTRYPOINT ["/usr/bin/kube-proxy"]
