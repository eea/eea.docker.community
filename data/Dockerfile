FROM busybox

MAINTAINER "David Ichim" <david.ichim@eaudeweb.ro>

ENV VOLUME_PATH /var/local/community.eea.europa.eu/

RUN mkdir -p $VOLUME_PATH/var/filestorage && mkdir -p $VOLUME_PATH/var/log
RUN chown -R 1000:1000 $VOLUME_PATH

VOLUME $VOLUME_PATH/var/filestorage
VOLUME $VOLUME_PATH/var/log

USER 1000
