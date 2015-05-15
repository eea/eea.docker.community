FROM busybox

MAINTAINER "David Ichim" <david.ichim@eaudeweb.ro>

ENV VOLUME_PATH /var/local/community.eea.europa.eu/var/
RUN mkdir -p $VOLUME_PATH && mkdir -p $VOLUME_PATH/filestorage && mkdir -p $VOLUME_PATH/blobstorage

RUN chown -R 1000:1000 $VOLUME_PATH
#VOLUME $VOLUME_PATH

USER 1000
