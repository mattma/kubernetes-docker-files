FROM scratch
MAINTAINER Matt Ma "matt@mattmadesign.com"
ENV REFRESHED_AT 2015-09-01

COPY kube-scheduler /kube-scheduler
VOLUME ["/srv/kubernetes", "/etc/ssl"]
ENTRYPOINT ["/kube-scheduler"]
