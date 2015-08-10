FROM scratch
MAINTAINER Matt Ma "matt@mattmadesign.com"
ENV REFRESHED_AT 2015-08-10

COPY kube-proxy /kube-proxy
VOLUME ["/etc/kubernetes", "/etc/ssl/certs", "/usr/sbin/iptables", "/lib64"]
ENTRYPOINT ["/kube-proxy"]
