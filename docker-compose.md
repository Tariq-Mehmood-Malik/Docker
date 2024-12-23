# Docker Compose
---

## Why we need Docker Compose   

Let's assume you were assigned on a project which requires at least 10 different services in a running state to run your project. For example, let's say your project requires Java 8, Node 14, MySQL, MongoDB, Ruby on rails, RabbitMQ, and others.   
In such case, you have to pull all those images individually from Docker and start all of them in their containers. At some point, one process may depend on another to run. So, you have to order them.   
It would be good if it's a one time process. But, not just once – every day, every time you start working on your project – you have to start all these services.   
To overcome this, Docker introduced a concept called Docker Compose.

## Docker Compose   

Docker Compose is a tool you can use to define and share multi-container applications. This means you can run a project with multiple containers using a single source.   
For example, assume you're building a project with NodeJS and MongoDB together. You can create a single `docker-ompose.yaml` file that starts both containers as a service – you don't need to start each separately.   

### Key Benefits of Docker Compose

1. **New Network**  
   Docker Compose automatically creates a new network for that infrastructure, ensuring isolated communication between them.

2. **Set Prefix for All Resources**  
   By default, Docker Compose sets a prefix for all resources (containers, networks, volumes) based on the project directory name, making it easier to manage and identify resources specific to a project.

3. **Multi-Container Management**  
   Easily define and run multi-container Docker applications with a single configuration file (`docker-compose.yml`).

4. **Simplified Configuration**  
   Allows you to configure and link multiple services (e.g., databases, web servers) with a single command.

5. **Consistency**  
   Ensures that the environment is consistent across all stages (development, testing, production).

6. **Automation**  
   Automates container deployment, scaling, and orchestration tasks.

7. **Isolation**  
   Each service runs in its container, providing isolation while allowing them to communicate seamlessly.

8. **Easy Setup and Scaling**  
   Easily start, stop, and scale up/down services with simple commands (`docker-compose up`, `docker-compose down`).

9. **New Network**  
   Docker Compose automatically creates a new network for that infrastructure, ensuring isolated communication between them.

10. **Set Prefix for All Resources**  
   By default, Docker Compose sets a prefix for all resources (containers, networks, volumes) based on the project directory name, making it easier to manage and identify resources specific to a project.


### Docker Compose YAML   

The compose file is a YML file defining services, networks, and volumes for a Docker container. There are several versions of the compose file format available.    
- Example
```yaml
version: '3'
services:
  app:
    image: node:latest
    container_name: app_main
    restart: always
    command: sh -c "yarn install && yarn start"
    ports:
      - 8000:8000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: localhost
      MYSQL_USER: root
      MYSQL_PASSWORD: 
      MYSQL_DB: test
  mongo:
    image: mongo
    container_name: app_mongo
    restart: always
    ports:
      - 27017:27017
    volumes:
      - ~/mongo:/data/db
volumes:
  mongodb:
```

Let's discuss the above code and understand it piece by piece:

`version`: refers to the docker-compose version (Latest 3)   

`services`: defines the services that we need to run   

`app`: is a custom name for one of your containers   

`image`: the image which we have to pull. Here we are using node:latest and mongo.   

`container_name`: is the name for each container   

`restart`: starts/restarts a service container   

`ports`: defines the custom port to run the container   

`working_dir` is the current working directory for the service container   

`environment` defines the environment variables, such as DB credentials, and so on.   

`command` is the command to run the service   

### How to run the multi-container    
We need to build our multi-container using docker build.

docker compose build
After successfully building, we can run the containers using the up command.

docker compose up
If you want to run the container in detached mode, just use the -d flag.

docker compose up -d
