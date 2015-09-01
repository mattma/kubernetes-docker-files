FROM scratch
MAINTAINER Matt Ma "matt@mattmadesign.com"
ENV REFRESHED_AT 2015-09-01

COPY kube-controller-manager /kube-controller-manager
VOLUME ["/srv/kubernetes", "/etc/ssl"]
ENTRYPOINT ["/kube-controller-manager"]
