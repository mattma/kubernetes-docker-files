FROM scratch
MAINTAINER Matt Ma "matt@mattmadesign.com"
ENV REFRESHED_AT 2015-08-10

COPY kube-apiserver /kube-apiserver
VOLUME ["/etc/kubernetes", "/etc/ssl/certs", "/var/run/kubernetes"]
ENTRYPOINT ["/kube-apiserver"]
