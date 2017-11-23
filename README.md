# EEA Community application docker setup

EEA Community site orchestration for deployment, including  **Apache**, **HAProxy**, **ZEO server**, **ZEO client**,  **Postfix**.

**ZEO server** and **ZEO client** have the same base image, you can find it on
[Docker Hub](https://registry.hub.docker.com/u/eeacms/cynin/) or you can
inspect the [Github repository](https://github.com/eea/eea.docker.cynin)
to see the Dockerfile.

## Usage

1. Production environment:

> **Note:** See **EEA SVN** for `answers.txt` file

* From **Rancher Catalog > EEA** deploy:
  * Community (EEA Forum)

## Upgrade

### Upgrade `community` stack

1. **Add new catalog version** within [eea.rancher.catalog](https://github.com/eea/eea.rancher.catalog/tree/master/templates/community)

   * Prepare next release, e.g.: `17.9`:

        ```
        $ git clone git@github.com:eea/eea.rancher.catalog.git
        $ cd eea.rancher.catalog/templates/community

        $ cp -r 59 60
        $ git add 60
        $ git commit -m "Prepare release 17.9"
        ```

   * Release new version, e.g:. `17.9`:

        ```
        $ vim config.yml
        version: "17.9"

        $ vim 60/rancher-compose.yml
        ...
        version: "17.9"
        ...
        uuid: community-60
        ...

        $ vim 60/docker-compose.yml
        ...
        - image: eeacms/cynin:17.9
        ...

        $ git add .
        $ git commit -m "Release 17.9"
        $ git push
        ```

   * See [Rancher docs](https://docs.rancher.com/rancher/v1.2/en/catalog/private-catalog/#rancher-catalog-templates) for more details.

2. **Upgrade Rancher** deployment

   * Click the available upgrade button

   * Confirm the upgrade

   * Or roll-back if something goes wrong and abort the upgrade procedure


## Data migration

You can access production data inside `filestorage` and `blobstorage` volumes:

        $ docker volume ls

Thus:

1. Start **rsync client** on host **from where** do you want to migrate data (SOURCE HOST):

        $ docker run -it --rm --name=r-client \
                     -v community-filestorage:/filestorage \
                     -v community-blobstorage:/blobstorage \
                 eeacms/rsync sh

2. Start **rsync server** on host **where** do you want to migrate data (DESTINATION HOST):

        $ docker run -it --rm --name=r-server -p 2222:22 \
                     -v community-filestorage:/filestorage \
                     -v community-blobstorage:/blobstorage \
                     -e SSH_AUTH_KEY="<SSH-KEY-FROM-R-CLIENT-ABOVE>" \
                 eeacms/rsync server

3. Within **r-client** container from step 1 run:

        $ rsync -e 'ssh -p 2222' -avz /filestorage/ root@<DESTINATION HOST IP>:/filestorage/
        $ rsync -e 'ssh -p 2222' -avz /blobstorage/ root@<DESTINATION HOST IP>:/blobstorage/


## First time steps to get a clean Cyn.in portal

If you don't have already an existing Cyn.in site (zodb data.fs) than the following step will get you started to create a fresh Cyn.in site.

1. Point your public LB to cynin instance instead of apache
2. open in browser: http://my.cynin.com/manage
user: admin
password: secret
(default zope admin user)
3. Change `admin` password within **acl_users**
4. From ZMI add a "Plone Site" object, set the id: "cynin", press the "Add plone site" button
5. Select "cynin", look for "portal_quickinstaller", check the product called "Ubify Site Policy", press the "Install" button
6. Open in browser: http://my.cynin.com/cynin
7. Point back your public LB to apache

Documentation at http://cyn.in/
