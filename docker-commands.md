# Docker Commands

To create container `nginx` latest version, after pulling image from Docker-Hub if not locally present, with name tag `my-container` but not start it :
```bash
docker create --name my-container nginx
docker start my-container        # to start container if created before
```
<br>

To create & run container `nginx` latest version. `-d` run in detach mode (foreground):   
```bash 
docker run -d --name my-container nginx
```
<br>

To create & run container `nginx` latest version. `-it` provide interactive `TTY` of container after creating it:
```bash
docker run -itd --name my-container nginx
```
<br>

To create & run container `nginx` latest version. `-p` port map 80 to 8080 port of host:
```bash
docker run -d -p 8080:80 --name my-container nginx
```
<br>

To create & run container `nginx` latest version. `-v` attach custom volume to conatiner:
```bash
docker run -d -v /path/to/host:/path/to/conatiner --name my-container nginx     # for Bind mount

docker run -d -v volume-name:/path/to/conatiner --name my-container nginx      # for Named volume
```
<br>

To list all conatiners running
```bash
docker ps

docker container ls
```
<br>

To list all containers running & stoped.
```bash
docker ps -a

docker container ls -a
```
<br>

To list all local images
```bash
docker image ls

docker images
```
<br>

To pull image from Docker-Hub to local host
```bash
docker pull nginx:latest
```
<br>

To get interactive bash shell of running container
```bash
docker exec -it my-conatiner /bin/bash
```
<br>

To inspect (view all details) container 
```bash
docker container inspect my-container
```
<br>

To inspect (view all details) image 
```bash
docker image inspect <image-name>
```
<br>

To view all running conatiner logs
```bash
docker logs
```
<br>

To stop docker container
```bash
docker stop <container_name_or_id>
```
<br>

To kill (sudden stop) docker container
```bash
docker kill <container_name_or_id>
```
<br>

To remove docker container
```bash
docker rm <container_name_or_id>
```
<br>

To create image from `Dockerfile` in current directory
```bash
docker build -t <image-name>:<tag> .      # . means scan current directory for Dockerfile
```
<br>

To login to docker-hub 
```bash
docker login
```
<br>

Updating tag of sutom image before uploading it to docker-hub
```bash
docker tag <imaage-name>:<tag> <Docker-hub repo>:<tag>
```
e.g:  docker tag webserv:v1 tariqmehmoodmalik/test-repo:v1

<br>

Pushing custom image to docker-hub
```bash
docker push <image-name>:<tag>
```
<br>

To list all docker networks
```bash
docker network ls
```
<br>

To create new network of driver bridge
```bash
docker network create --driver bridge <netwrok-name>
```
<br>

To inspect network
```bash
docker network inspect <network-name>
```
<br>

To create conatiner in custom network
```bash
docker run -d --net <network-name> nginx
```
<br>

To connect container running in 1 network to an other network also
```bash
docker network connect <new-network-name> <container-name>
```
<br>

