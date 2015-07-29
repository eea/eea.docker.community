## EEA Community application docker setup

EEA Community site orchestration for deployment, including 
**Apache**, **HAProxy**, **ZEO server**, **ZEO client**, 
mailserver and a dedicated data container.

**ZEO server** and **ZEO client** have the same base image, you can find it on
[Docker Hub](https://registry.hub.docker.com/u/eeacms/cynin/) or you can
inspect the [Github repository](https://github.com/eea/eea.docker.cynin) 
to see the Dockerfile.

### Installation
1. Install [Docker](https://www.docker.com/).
2. Install [Docker Compose](https://docs.docker.com/compose/).

### Usage


1. Production environment:

```
    $ git clone https://github.com/eea/eea.docker.community
    $ cd eea.docker.community
```

2. **Before starting you may want to restore existing database** within data container. 
```
    $ docker-compose up data
    $ docker run --rm \
      --volumes-from eeadockercommunity_data_1 \
      --volume /var/local/backup:/backup \
      busybox \
      /bin/sh -c "cp /backup/Data.fs /var/local/community.eea.europa.eu/var/filestorage/ && chown 1000:1000 /var/local/community.eea.europa.eu/var/filestorage/Data.fs"
```
3. Start services
```
    $ docker-compose up -d --no-recreate
```

4. After all containers are started, you can access the application at **http://localhost** 

5. Update Mail Server via Administration > Mail settings: Replace **localhost** with **postfix**


### Upgrade

    $ docker-compose stop
    $ docker-compose pull
    $ docker-compose rm -v apache haproxy www zeo mailserver
    $ docker-compose up -d --no-recreate


### Data migration

You can access production Data.fs on Gerbil
