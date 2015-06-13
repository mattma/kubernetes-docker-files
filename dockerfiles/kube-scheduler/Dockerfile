FROM debian:jessie

ADD kube-scheduler /usr/bin/kube-scheduler
RUN chmod 755 /usr/bin/kube-scheduler

ENTRYPOINT ["/usr/bin/kube-scheduler"]
