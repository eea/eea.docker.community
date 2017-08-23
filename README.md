## EEA Community application docker setup

EEA Community site orchestration for deployment, including  **Apache**, **HAProxy**, **ZEO server**, **ZEO client**,  **Postfix**.

**ZEO server** and **ZEO client** have the same base image, you can find it on
[Docker Hub](https://registry.hub.docker.com/u/eeacms/cynin/) or you can
inspect the [Github repository](https://github.com/eea/eea.docker.cynin)
to see the Dockerfile.

### Installation
1. Install [Docker](https://www.docker.com/).
2. Install [Docker Compose](https://docs.docker.com/compose/).

### Usage

1. Production environment:


    $ git clone https://github.com/eea/eea.docker.community
    $ cd eea.docker.community


During the first time deployment, create the secret environment files


    $ cp .env.example .env


Edit the secret files with real settings (see deployed eea.docker.redmine for example)


    $ vim .env


2. **You may also want to restore existing database** within data container:


    $ docker-compose up -d zeo
    $ docker cp Data.fs eeadockercommunity_zeo_1:/var/local/community.eea.europa.eu/var/filestorage/
    $ docker exec -it eeadockercommunity_zeo_1 chown 1000:1000 /var/local/community.eea.europa.eu/var/filestorage/Data.fs

3. **Start services**

        $ docker-compose up -d


4. After all containers are started, you can access the application at **http://localhost:80**. See the docker-compose.yml file for the ports that are exposed to the host.


### Upgrade

    $ cd eea.docker.community
    $ git pull
    $ docker-compose pull

    $ docker-compose down
    $ docker-compose up -d

### Data migration

You can access production data inside `filestorage` and `blobstorage` volumes:

    docker volume ls

Thus:

1. Start **rsync client** on host **from where** do you want to migrate data (SOURCE HOST):


    $ docker run -it --rm --name=r-client --volumes-from=eeadockercommunity_zeo_1 eeacms/rsync sh


2. Start **rsync server** on host **where** do you want to migrate data (DESTINATION HOST):


    $ docker-compose up -d zeo
    $ docker run -it --rm --name=r-server -p 2222:22 --volumes-from=eeadockercommunity_zeo_1 \
                 -e SSH_AUTH_KEY="<SSH-KEY-FROM-R-CLIENT-ABOVE>" \
             eeacms/rsync server


3. Within **rsync client** container from step 1 run:


    $ rsync -e 'ssh -p 2222' -avz /var/local/community.eea.europa.eu/var/filestorage/ root@<DESTINATION HOST IP>:/var/local/community.eea.europa.eu/var/filestorage/
    $ rsync -e 'ssh -p 2222' -avz /var/local/community.eea.europa.eu/var/blobstorage/ root@<DESTINATION HOST IP>:/var/local/community.eea.europa.eu/var/blobstorage/


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
