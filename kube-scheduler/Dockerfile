FROM debian:jessie
MAINTAINER Kelsey Hightower <kelsey.hightower@gmail.com>

RUN apt-get update
RUN apt-get -y -q install ca-certificates

COPY kube-scheduler /usr/bin/kube-scheduler
RUN chmod 755 /usr/bin/kube-scheduler

ENTRYPOINT ["/usr/bin/kube-scheduler"]
