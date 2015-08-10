FROM scratch
MAINTAINER Matt Ma "matt@mattmadesign.com"
ENV REFRESHED_AT 2015-08-10

COPY kube-scheduler kube-scheduler
VOLUME /etc/kubernetes /etc/ssl/certs
ENTRYPOINT ["/kube-scheduler"]
