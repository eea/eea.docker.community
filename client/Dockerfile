#FROM eeacms:centos
FROM eeacms/community
ENV SLEEP_DELAY 0
ENV INSTANCE_PATH /var/local/community.eea.europa.eu


# MAINTAINER David Ichim <david.ichim@eaudeweb.ro>
COPY instance.cfg $INSTANCE_PATH/instance.cfg
WORKDIR $INSTANCE_PATH
#VOLUME /var/local/community.eea.europa.eu
RUN bin/buildout -c instance.cfg
CMD sleep $SLEEP_DELAY; bin/instance console;

EXPOSE 8080
