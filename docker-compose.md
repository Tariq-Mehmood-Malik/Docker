# Docker Compose

## Why we need Docker Compose   

Let's assume you were assigned on a project which requires at least 10 different services in a running state to run your project. For example, let's say your project requires Java 8, Node 14, MySQL, MongoDB, Ruby on rails, RabbitMQ, and others.   
In such case, you have to pull all those images individually from Docker and start all of them in their containers. At some point, one process may depend on another to run. So, you have to order them.   
It would be good if it's a one time process. But, not just once – every day, every time you start working on your project – you have to start all these services.   
To overcome this, Docker introduced a concept called Docker Compose.

## Docker Compose   

Docker Compose is a tool you can use to define and share multi-container applications. This means you can run a project with multiple containers using a single source.   
For example, assume you're building a project with NodeJS and MongoDB together. You can create a single `docker-compose.yaml` file that starts both containers as a service – you don't need to start each separately.   

### Key Benefits of Docker Compose

1. **Network Isolation**  
   Docker Compose automatically creates a new network for that infrastructure, ensuring isolated communication from other container/ ifrrastructures.

2. **Unique Resource Names**  
   By default, Docker Compose sets a prefix for all resources (containers, networks, volumes) based on the project directory name, making it easier to manage and identify resources specific to a project.

3. **Consistency**  
   Ensures that the environment is consistent across all stages (development, testing, production).

4. **Automation**  
   Automates container deployment, scaling, and orchestration tasks.

5. **Easy Setup and Scaling**  
   Easily start, stop, and scale up/down services with simple commands (`docker-compose up`, `docker-compose down`).


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
      MYSQL_PASSWORD: pass
      MYSQL_DB: test
    depends_on:
      - mongo

  mongo:
    image: mongo
    container_name: app_mongo
    restart: always
    ports:
      - 27017:27017
    volumes:
      - my_volume:/data/db

volumes:
  my_volume:
```

Let's discuss the above code and understand it piece by piece:

`version`: refers to the docker-compose version (Latest 3)   

`services`: defines the services that we need to run   

`app`: is a custom name for one of our service      

`image`: the image which we have to pull. Here we are using node:latest and mongo.   

`container_name`: is the name for each container, if not defined will take service name  

`restart`: starts/restarts a service container   

`ports`: defines the custom ports to run the container   

`working_dir`: is the current working directory for the service container   

`environment`: defines the environment variables, such as DB credentials, and so on.   

`command`: is the command to run the service  

`volumes`: to attached named volume or bind mount with container.

`my_volume`: named volume created by docker.

`depends_on`: This ensures that app starts only after mongo is initialized

---   

## Docker-Compose Commands 

After creating compose file we can built & run our infrastructure with single command.

To build but not start infrastructure.
```bash
docker compose build 
```

To run already build services, if not availabe than created images.
```bash
docker compose up 
```

If you want to run the container in detached mode, just use the -d flag.    
```bash
docker compose up -d
```

To Stop infrastructure.
```bash
docker compose stop 
```

To start Stoped or already build services.
```bash
docker compose start 
```

To forcefully stop all running containers.
```bash
docker compose kill
```

To Stop & remove all services.
```bash
docker compose down
```   
---

## Defining Credentials

   In docker-compose file you can define credentiasl in enviroment section as below for dependent service like your app service need access of your database service.   
   
   ```yaml
   environment:
      MYSQL_USER: root
      MYSQL_PASSWORD: pass
   ```

   This is not recommended method as it can expose credentials if we share our docker-compose file to github or locally, to solve this we can use any of the following methods:

   ### 1. Set environment variables temporarily   
   In this method user export credentials to terminal and not define in docker-compose file like following:     
   In docker-compose we define veriables as following:   
   ```yaml
   environment:
      MYSQL_USER: ${USER}
      MYSQL_PASSWORD: ${PASSWORD}
   ```
      
   Before running docker-compose file user will run following in terminal:   
   ```bash
   export USER=admin
   export PASSWORD=pass
   ```   

   ### 2. Top-Level Secrets in Docker-Compose    
   In Docker Compose, a top-level `secrets` element is used to define sensitive data (such as passwords, API keys, or certificates) that you want to make available to services in a secure way. This feature was introduced in Docker Compose file version 3.1 and later, and it allows you to securely manage sensitive information without embedding them directly in your service definitions.
   
   ```yaml
   version: '3.7'

   # Top-level secrets element
   secrets:
     my_secret_1:
       file: ./my_secret_file_1.txt  # This file contains the secret
     my_secret_2:
       file: ./my_secret_file_2.txt  # This file contains the secret

   services:
     web:
       image: nginx:latest
       secrets:
         - my_secret_1  # Referencing the secret for the web service

     app:
       image: myapp:latest
       secrets:
         - my_secret_2  # Referencing the same secret for another service
   ```   

   Secret text file synatax example:   
   ```txt
   MY_SECRET_KEY=abcdef1234567890
   MY_API_KEY=9876543210abcdef
   ```


   
