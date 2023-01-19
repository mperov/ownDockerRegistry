# ownDockerRegistry
Own Docker registry with primitive authorization

## Requirements
- [Docker](https://docs.docker.com/install/)
- [docker-compose](https://docs.docker.com/compose/install/)

## Starup server
1. Generate admin credentials:
```console
$ docker run --rm --entrypoint htpasswd registry:2.7.0 -Bbn admin mypassword > auth/htpasswd
```
2. Run containers:
```console
$ docker-compose up -d
```
3. See two run containers by
```console
$ docker ps
```
4. Login into own registry:
```console
$ docker login -u=admin -p=mypassword  myserver:5000
```
5. Commit and push your image:
```console
$ docker commit <some hash> myserver/my-image
$ docker push myserver:5000/my-image
```

## Docker client configure
This solution use unsafe http protocol, so be careful! And on client side, you should configure docker daemon how did me:
```json
$ cat /etc/docker/daemon.json
{
    "insecure-registries":["myserver:5000"]
}

```
