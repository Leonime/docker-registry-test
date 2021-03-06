# docker-registry-test
A project to test a configuration for docker registry

```shell script
mkdir ./auth/
mkdir -p ./www/html
```

To create new user and password run this command.
```
docker run --entrypoint htpasswd registry:2 -Bbn testuser testpassword > ./auth/htpasswd
```

Docker UI basic configurator.
```
# Listen interface.
listen_addr: 0.0.0.0:8000
# Base path of Docker Registry UI.
base_path: /ui

# Registry URL with schema and port.
registry_url: http://registry:5000
# Verify TLS certificate when using https.
verify_tls: true

# Docker registry credentials.
# They need to have a full access to the registry.
# If token authentication service is enabled, it will be auto-discovered and those credentials
# will be used to obtain access tokens.
registry_username: testuser
registry_password: testpassword

# Event listener token.
# The same one should be configured on Docker registry as Authorization Bearer token.
event_listener_token: token
# Retention of records to keep.
event_retention_days: 7

# Event listener storage.
event_database_driver: sqlite3
event_database_location: data/registry_events.db
# event_database_driver: mysql
# event_database_location: user:password@tcp(localhost:3306)/docker_events

# Cache refresh interval in minutes.
# How long to cache repository list and tag counts.
cache_refresh_interval: 10

# If users can delete tags. If set to False, then only admins listed below.
anyone_can_delete: false
# Users allowed to delete tags.
# This should be sent via X-WEBAUTH-USER header from your proxy.
admins: []

# Debug mode. Affects only templates.
debug: true

# CLI options.
# How many days to keep tags but also keep the minimal count provided no matter how old.
purge_tags_keep_days: 90
purge_tags_keep_count: 10
```

Create certificate.
```shell script
sudo docker-compose up --force-recreate --no-deps certbot
```

Prepare nginx
```shell script
sudo docker-compose stop nginx

mkdir dhparam

sudo openssl dhparam -out ./.data/dhparam/dhparam-4096.pem 4096
```