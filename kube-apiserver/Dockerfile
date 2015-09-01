FROM scratch
MAINTAINER Matt Ma "matt@mattmadesign.com"
ENV REFRESHED_AT 2015-09-01

COPY kube-apiserver /kube-apiserver
VOLUME ["/srv/kubernetes", "/etc/ssl", "/var/run/kubernetes", "var/log"]
ENTRYPOINT ["/kube-apiserver"]
