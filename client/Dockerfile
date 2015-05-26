FROM eeacms/cynin
ENV SLEEP_DELAY 0
ENV INSTANCE_PATH /var/local/community.eea.europa.eu
ENV INSTANCE_NAME $HOSTNAME

WORKDIR $INSTANCE_PATH
RUN sed -i "s/127.0.0.1/zeo/g" parts/instance/etc/zope.conf;

USER cynin

CMD sleep $SLEEP_DELAY; sed -i "s/SERVER/$INSTANCE_NAME/g" parts/instance/etc/zope.conf; bin/instance console;

EXPOSE 8080
