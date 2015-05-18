FROM busybox

MAINTAINER "David Ichim" <david.ichim@eaudeweb.ro>

ENV VOLUME_PATH /var/local/community.eea.europa.eu/

RUN mkdir -p $VOLUME_PATH/var/filestorage
RUN chown -R 1000:1000 $VOLUME_PATH && chown -R 1000:1000 $VOLUME_PATH/var/filestorage

VOLUME $VOLUME_PATH/filestorage

USER 1000
