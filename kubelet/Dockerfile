FROM debian:jessie
MAINTAINER Kelsey Hightower <kelsey.hightower@gmail.com>

RUN apt-get update
RUN apt-get -y -q install ca-certificates

COPY kubelet /usr/bin/kubelet
RUN chmod 755 /usr/bin/kubelet

ENTRYPOINT ["/usr/bin/kubelet"]
