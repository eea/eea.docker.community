#FROM eeacms:centos
FROM eeacms/community
ENV SLEEP_DELAY 0


# MAINTAINER David Ichim <david.ichim@eaudeweb.ro>
WORKDIR /var/local/community.eea.europa.eu
CMD sleep $SLEEP_DELAY; bin/www1 console;

EXPOSE 8081
