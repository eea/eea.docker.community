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
 ```
    $ cp .postfix.secret.example .postfix.secret
    
 ```
Edit the secret files with real settings (see deployed eea.docker.redmine for example)
 ```
    $ vim .postfix.secret
 ```
2. **You may also want to restore existing database** within data container:
 ```
    $ docker-compose up data
    $ docker run --rm \
      --volumes-from eeadockercommunity_data_1 \
      --volume /var/local/backup:/backup \
      busybox \
      /bin/sh -c "cp /backup/Data.fs /var/local/community.eea.europa.eu/var/filestorage/ && chown 1000:1000 /var/local/community.eea.europa.eu/var/filestorage/Data.fs"
      
 ```
3. **Start services**
 ```
    $ docker-compose up -d --no-recreate
    
 ```
4. After all containers are started, you can access the application at **http://localhost:7070**. See the docker-compose.yml file for the ports that are exposed to the host.

5. **Troubleshooting**
   If the http://localhost:7070 is not responding or throws an 503 error
 ```
    $ docker-compose rm -v apache haproxy www1 www2 zeo postfix
    $ docker-compose up -d --no-recreate
    
 ```

### Upgrade

    $ docker-compose stop
    $ docker-compose pull
    $ docker-compose rm -v apache haproxy www1 www2 zeo postfix
    $ docker-compose up -d --no-recreate

### First time steps to get a clean Cyn.in portal

If you don't have already an existing Cyn.in site (zodb data.fs) than the following step will get you started to create a fresh Cyn.in site. 

1. expose a port for www1 container: in docker-compose.yml at the www1 section (this will allow you to get into the Plone www1 instance ZMI) add: ```ports: "8080:8080"```
2. run command: docker-compose up
3. open in browser: http://localhost:8080/manage
user: admin
password: secret
(default zope admin user)
4. from ZMI add a "Plone Site" object, set the id: "cynin", press the "Add plone site" button
5. select "cynin", look for "portal_quickinstaller", check the product called "Ubify Site Policy", press the "Install" button
6. open in browser: http://localhost:8080/cynin

You can than visit via apache and HAProxy via http://localhost:7070/cynin

Documentation at http://cyn.in/
