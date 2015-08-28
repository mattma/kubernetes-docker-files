FROM scratch
MAINTAINER Matt Ma "matt@mattmadesign.com"
ENV REFRESHED_AT 2015-08-27

COPY kube-controller-manager /kube-controller-manager
VOLUME ["/etc/kubernetes", "/etc/ssl/certs"]
ENTRYPOINT ["/kube-controller-manager"]
