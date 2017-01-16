# docker-geonode

GeoNode 2.5.7 with GeoServer oauth2 configured

## How to run

This will run GeoNode on the default docker machine @ 192.168.99.100. If not adjust the IP addresses in the docker-compose.yml and fixtures/default_oauth_apps.json.

* git clone https://github.com/senoadiw/docker-geonode.git
* cd docker-geonode
* docker-compose up -d
* docker-compose ps
* docker exec -it dockergeonode_django_1 bash
    * django-admin migrate account --noinput && django-admin migrate --noinput && django-admin collectstatic --noinput
    * django-admin createsuperuser
        * admin adminpassword
    * django-admin loaddata fixtures/default_oauth_apps.json
* wait a few minutes while GeoServer initializes
* docker-compose stop
* docker-compose up -d
* access GeoNode at http://machineip
* done!

## Images used

* https://hub.docker.com/r/senoadiw/geonode_oauth/
* https://hub.docker.com/r/senoadiw/geoserver_oauth/
* https://hub.docker.com/r/senoadiw/geoserver_oauth_data/
