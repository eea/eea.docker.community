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

During the first time deployement, create the secret environment files

    $ cp .postfix.secret.example .postfix.secret

Edit the secret files with real settings (see deployed eea.docker.redmine for example)

    $ vim .postfix.secret

2. **You may also want to restore existing database** within data container:
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
4. After all containers are started, you can access the application at **http://localhost:7070**. See the docker-compose.yml file for the ports that are exposed to the host.


### Upgrade

    $ docker-compose stop
    $ docker-compose pull
    $ docker-compose rm -v apache haproxy www1 www2 zeo postfix
    $ docker-compose up -d --no-recreate


### Data migration

You can access production Data.fs on Gerbil
