FROM debian:jessie
MAINTAINER Kelsey Hightower <kelsey.hightower@gmail.com>

RUN apt-get update
RUN apt-get -y -q install ca-certificates

COPY kube-controller-manager /usr/bin/kube-controller-manager
RUN chmod 755 /usr/bin/kube-controller-manager

ENTRYPOINT ["/usr/bin/kube-controller-manager"]
