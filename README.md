# docker-registry-test
A project to test a configuration for docker registry

To create new user and password run this command.
```
docker run --entrypoint htpasswd registry:2 -Bbn testuser testpassword > ./auth/htpasswd
```