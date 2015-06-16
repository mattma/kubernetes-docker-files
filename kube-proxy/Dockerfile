FROM debian:jessie
MAINTAINER Kelsey Hightower <kelsey.hightower@gmail.com>

RUN apt-get update
RUN apt-get -y -q install iptables ca-certificates

COPY kube-proxy /usr/bin/kube-proxy
RUN chmod 755 /usr/bin/kube-proxy

ENTRYPOINT ["/usr/bin/kube-proxy"]
