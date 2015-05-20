FROM eeacms/community
ENV SLEEP_DELAY 0
ENV INSTANCE_PATH /var/local/community.eea.europa.eu

WORKDIR $INSTANCE_PATH
RUN sed -i "s/127.0.0.1/zeo/g" parts/instance/etc/zope.conf;
CMD sleep $SLEEP_DELAY;  bin/instance console;

EXPOSE 8080
