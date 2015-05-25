## EEA Community application docker setup

Docker images created for EEA Community site, including images for **ZEO server**, **ZEO client**, **Varnish**, mailserver and a dedicated data container.

**ZEO server** and **ZEO client** have the same base image, you can find it on [Docker Hub](https://registry.hub.docker.com/u/eeacms/cynin/) or you can inspect the [Github repository](https://github.com/eea/eea.docker.cynin) to see the Dockerfile.


### Installation
1. Install [Docker](https://www.docker.com/).
2. Install [Docker Compose](https://docs.docker.com/compose/).

### Usage

Production environment:
  
    $ git clone https://github.com/eea/eea.docker.community
    $ cd eea.docker.community
    $ docker-compose up

Development environment:

    $ git clone https://github.com/eea/eea.docker.community
    $ cd eea.docker.community
    $ docker-compose -f docker-compose-dev.yml up

After all containers are started, you can access the application on **http://\<IP\>**, where **IP** is address of your machine.

### Restore application data
If you have a Data.fs file for EEA Community application, you can add it with the following commands:

    $ docker-compose up data
    $ docker run -it --rm --volumes-from eeadockercommunity_data_1 -v \ 
      /path/to/parent/folder/of/Data.fs/file/:/mnt eeacms/centos:7 /bin/bash -c \ 
      "cp /mnt/Data.fs /var/local/community.eea.europa.eu/var/filestorage/ && chown 1000:1000 /var/local/community.eea.europa.eu/var/filestorage/Data.fs"

### Data migration
You can access production Data.fs on Gerbil
