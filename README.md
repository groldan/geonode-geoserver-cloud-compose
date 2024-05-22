# GeoServer Cloud experimental GeoNode integration

GeoNode admin user: admin/geonode

* Run geonode with geoserver cloud:

```
docker compose pull
docker compose up -d
```

* Run geonode with vanilla geoserver

```
docker compose -f vanilla.yml up -d
```


## Configure GeoNode AuthZN

* Go to http://localhost/geoserver/ (note the trailing /)
* Log in as admin/geoserver

### Create ROLE service

* Go to Security -> Users, Groups, Roles -> Services -> Role Services/Add New
* Select AuthKEY REST
    - Name: geonode REST role service
    - Base Server URL: http://django:8000
* Save

### Set up geonode oauth2 security filter chain

* Go to Security -> Authentication -> New auth filter -> GeoNode Oauth2
* Use the following settings:
    - User Authorization URI
    - Access Token URI: http://django:8000/o/token/
    - User Authorization URI: http://localhost/o/authorize/
    - Redirect URI: http://localhost/geoserver/index.html
    - Check Token Endpoint URL: http://django:8000/api/o/v4/tokeninfo/
    - Logout URI: http://localhost/account/logout/
    - Scopes: write
    - Client ID: geonode
    - Client Secret: geonodesecret
    - ROLE Service: geonode REST role service
* Save
* Go to Filter Chains, and add the `geonode` filter chain to the `web`, `rest`,
  `gwc`, and `default` chains BEFORE(above) the `gateway-shared-auth` one.
* Save
* Logout. You should see the GeoNode login button now in the GeoServer webui page header

Now log in with geonode and it should work as usual.

