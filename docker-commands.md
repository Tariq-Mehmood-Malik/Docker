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

To create & run container `nginx` latest version. `-v` attach custom volume to conatiner with read only access:
```bash
docker run -d -v /host/path:/container/path:ro nginx     # for Bind mount

docker run -d -v my-volume:/data:ro nginx      # for Named volume
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

To create named volume in docker
```bash
docker volume create my_volume
```
<br>

To list all named  in docker
```bash
docker volume ls
```
<br>

To inspect named volume
```bash
docker volume inspect my_volume
```
<br>

### Docker-Compose Commands

To build but not start infrastructure.
```bash
docker compose build 
```
<br>

To run already build services, if not availabe than created images.
```bash
docker compose up 
```
<br>

If you want to run the container in detached mode, just use the -d flag.    
```bash
docker compose up -d
```
<br>

To Stop infrastructure.
```bash
docker compose stop 
```
<br>

To start Stoped or already build services.
```bash
docker compose start 
```
<br>

To forcefully stop all running containers.
```bash
docker compose kill
```
<br>

To Stop & remove all services.
```bash
docker compose down
```

### Docker Swarm Commands

- Before creating docker-swarm we need to allow certain network port to open for docker swarm to work with following command:
  
```bash
sudo ufw allow 2377,7946,4789/tcp
```
<br>

- To initialize the docker swarm cluster we use the command called `docker swarm init`.

```bash
docker swarm init
```
<br>

- After this command docker will provide you token to use on nodes to join cluster as worker nodes.
Example:

```txt
docker swarm join --token <token-id> <network-id>:2377
```
<br>

- To check cluster node run following command in `Manager Node`:

```bash
docker node ls
```
<br>

- Creating service with 2 replication in swarm:

```bash
docker service create --name web --publish target=80,published=83 --replicas=2 nginx
```
<br>

- Creating service with 2 replication in swarm:

```bash
docker service create --name ping --replicas=2 alpine ping 8.8.8.8
```
<br>

- Checking running services

```bash
docker service ls
```
<br>

- Inspecting dokcer node

```bash
docker node inspect <node-id>
```
<br>

- Inspecting dokcer service

```bash
docker service inspect <service-name>
```
<br>

- Viewing dokcer service logs

```bash
docker service logs <service-name>
```
<br>

- Scaling running service:

```bash
docker service scale <service-name>=5
```
<br>

- Veiw new overlay networks created by docker-swarm:

```bash
docker network ls
```
<br>

- Viewing dokcer service logs

```bash
docker service rm <service-name>
```

## Docker Stack Commands

Before deploying stack of services/ application please make sure `Docker swarm` is initiated if not run following in master (leader) node:
```bash
docker swarm init
```
<br>

Deploying docker stack command:
```bash
docker stack deploy -c <file-name>.yml <New-Stack-Name>
```
<br>

View running stack.
```bash
docker stack ls
```
<br>

View running stack services
```bash
docker stack services <Stack-Name>
```
<br>

Lists the tasks (containers) in a stack.
```bash
docker stack ps <stack-name>
```
<br>

Removes a stack from the Docker Swarm.
```bash
docker stack rm <stack-name>
```
<br>

Inspects a stack in the Swarm to provide detailed information.
```bash
docker stack inspect <stack-name>
```
<br>

Executes a command on a service container in a stack.
```bash
docker stack exec <stack-name> <service-name> <command>
```
<br>

Validates and views a Docker Compose file for stack deployment.
```bash
docker stack config -c <compose-file>
```
<br>
