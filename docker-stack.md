# Docker Stack

Docker Stack is a deployment and orchestration tool that comes built into Docker Engine. It allows you to define and run multi-container applications in Docker Swarm mode. 
It also provides a simple way to deploy the app and manage it’s entire lifecycle [initial deployment > health checks > scaling > updates > rollbacks]

The desired state of your app is defined in a Compose file, then deploy and managed in with “docker stack” command. 
The Compose file includes the entire stack of microservices that make up the app, includes :- volumes, networks, secrets etc.

Docker Stack makes it easier to deploy, scale, and manage services across multiple Docker hosts as part of a Swarm cluster. 
It abstracts away the complexity of handling individual containers and their interactions, allowing you to define an entire application's architecture in a single yaml file.
In Docker-Stack each stack create its own overlay network (over default overlay network) if network is not defined else create overlay network by name define in compose.yaml file.

Example Yaml file for Docker Stack:   
```yaml
version: '3'

services:
  frontend:
    image: my-web-app:latest
    deploy:
      replicas: 2  # Swarm mode: Scale the frontend to 2 instances
    networks:
      - webnet

  backend:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: pass  # Set the root password for MySQL
    networks:
      - webnet
    volumes:
      - db-data:/var/lib/mysql  # Persist MySQL data in a volume
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]  # Check if MySQL is reachable
      interval: 30s  # Check every 30 seconds
      retries: 5  # Retry 5 times before marking the service as unhealthy
      start_period: 10s  # Wait 10 seconds before starting the health check
      timeout: 10s  # Timeout the health check after 10 seconds

networks:
  webnet:  # Define the network for both frontend and backend to communicate

volumes:
  db-data:  # Define a volume to persist database data

```

**Explaination:**  
- Define Services: The frontend service runs the web app and the backend service runs MySQL.   
- Deployment Settings: The frontend service is set to run 2 replicas for high availability.    
- Networking: Both services communicate over a shared network webnet.   
- Volumes: The database data is stored in a persistent volume db-data.
- Health Check for MySQL: The backend service (MySQL) now has a health check to ensure it is running and accepting connections before other services try to use it.

---
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
