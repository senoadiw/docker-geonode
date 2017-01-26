# docker-geonode

GeoNode 2.5.10 with GeoServer oauth2 configured

## How to run

This will run GeoNode on the default docker machine.

Make sure the default machine is accessible at http://geonode. Edit the /etc/hosts file if necessary.

If using Docker with the VirtualBox driver on Windows, make sure to assign at least 4096MB of RAM to the default machine VM.

* git clone https://github.com/senoadiw/docker-geonode.git
* cd docker-geonode
* docker-compose up -d
* docker-compose ps
* docker exec -it dockergeonode_django_1 bash
    * django-admin migrate --noinput && django-admin collectstatic --noinput
    * django-admin loaddata sample_admin
    * django-admin loaddata initial_data
    * django-admin loaddata fixtures/default_oauth_apps.json
    * exit
* wait a few minutes while GeoServer initializes
* access GeoNode at http://geonode
* try signing in from the oauth login button at http://geonode/geoserver (admin admin)
* done!

## Rancher

Make sure the host machine is accessible at http://geonode. Edit the /etc/hosts file if necessary.

* access rancher server in browser
    * select default environment on rancher server
    * create environment api key for rancher compose
        * click API on menu bar > ADVANCED OPTIONS
            * name: rancher-compose
            * keep note of access and secret key pair
* run rancher-compose to create geonode-rancher stack
    * download rancher-compose (download link at bottom right of rancher server)
    * extract rancher-compose executable to docker-geonode dir
    * cd to docker-geonode
    * run this: ./rancher-compose --url=http://RANCHERSERVER:8080/ --access-key=ENVKEYUSER --secret-key=ENVKEYPASS --verbose -p geonode-rancher -f docker-compose.rancher.yml create
    * the stack is successfully created if you get this output: level=debug msg="Project [geonode-rancher]: Project created "
* access rancher server in browser
    * geonode-rancher stack should be available in user stacks
    * click the geonode-rancher stack to see the service list
    * click the play button and wait while images are download and all containers become active
    * click the django service
        * click the menu dropdown button on the container > Execute Shell
        * on django container shell run these to initialize the geonode tables:
            * django-admin migrate --noinput && django-admin collectstatic --noinput
            * django-admin loaddata sample_admin
            * django-admin loaddata initial_data
* create default oauth2 app entry for GeoServer
    * access http://geonode/admin/oauth2_provider/application/add (login with admin admin)
    * enter these values:
        * client id: Jrchz2oPY3akmzndmgUTYrs9gczlgoV20YPSvqaV
        * user: admin
        * redirect uris: http://geonode/geoserver/
        * client type: confidential
        * authorization grant type: authorization-code
        * client secret: rCnp5txobUo83EpQEblM8fVj3QT5zb5qRfxNsuPzCqZaiRyIoxM4jdgMiZKFfePBHYXCLd7B8NlkfDBY9HKeIQPcy5Cp08KQNpRHQbjpLItDHv12GvkSeXp6OxaUETv3
        * name: GeoServer
        * skip authorization: check
        * click save
* try signing in from the oauth login button at http://geonode/geoserver (admin admin)
* done!

## Images used

* https://hub.docker.com/r/senoadiw/geonode_oauth/
* https://hub.docker.com/r/senoadiw/geoserver_oauth/
* https://hub.docker.com/r/senoadiw/geoserver_oauth_data/
