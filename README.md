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
2. Generate private key and X.509 self-signed certificate on 10 years:
```console
$ openssl req -newkey rsa:2048 -subj "/CN=myserver" -addext "subjectAltName = DNS:myserver" -nodes -keyout auth/server.key -x509 -days 3650 -out auth/server.crt
```
3. Run containers:
```console
$ docker-compose up -d
```
4. See two run containers by
```console
$ docker ps
```
5. Login into own registry:
```console
$ docker login -u=admin -p=mypassword  myserver
```
6. Commit and push your image:
```console
$ docker commit <some hash> myserver/my-image
$ docker push myserver/my-image
```

## Docker client configure
On the second step you have generated self-signed certificate but docker may only communicate from docker registry with trusted certificate.  
So you should place the certificate to special directory on each machines:  
```console
$ sudo mkdir -p /etc/docker/certs.d/myserver/
$ sudo cp server.crt /etc/docker/certs.d/myserver/server.crt
```
Or apply ansible playbook on all docker clients:
```console
$ ansible-playbook docker_cert_pb.yml -e server_name=myserver
```
