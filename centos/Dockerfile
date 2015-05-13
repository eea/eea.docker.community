FROM eeacms:centos

MAINTAINER David Ichim <david.ichim@eaudeweb.ro>

RUN yum -y update

# Python install
ENV PYTHON_VERSION 2.4.6

RUN wget https://www.python.org/ftp/python/$PYTHON_VERSION/Python-$PYTHON_VERSION.tgz && \
    tar -xvf Python-$PYTHON_VERSION.tgz && \
    cd Python-$PYTHON_VERSION && ./configure && make && make install

# CYNIN installation
ENV CYNIN_VERSION 3.2.2
ENV CYNIN_PATH /var/local
ENV CYNIN_BUILDOUT https://svn.eionet.europa.eu/repositories/Zope/trunk/community.eea.europa.eu/trunk
ENV CYNIN_NAME community.eea.europa.eu

RUN cd $CYNIN_PATH &&  svn co CYNIN_BUILDOUT && \
    cd $CYNIN_NAME && ./install.sh

# Clean up
RUN yum clean all && rm -r Python-$PYTHON_VERSION Python-$PYTHON_VERSION.tgz
