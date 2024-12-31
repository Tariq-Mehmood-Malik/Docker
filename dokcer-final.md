# DOCKER FINAL TASK

## DOCKER NETWORKS

#### Create two networks prod and dev for creating the following containers (Database, website, and a Linux) 

docker network create --driver bridge prod

docker network create --driver bridge dev

```bash
docker run -d --name data-base -e MYSQL_ROOT_PASSWORD=pass -p 3306:3306 mysql
```

```bash
docker run -d --name web-site -p 8080:80 nginx
```

```bash
docker run -d --name linuxe alpine ping 8.8.8.8
```
#### Each container should be running in both networks

```bash
docker network connect prod linux
docker network connect dev linux
```

```bash
docker network connect prod data-base
docker network connect dev data-base
```

```bash
docker network connect prod web-site
docker network connect dev web-site
```

#### The database container of Dev network should be accessible to prod one during migration

```bash
docker exec -it 
```

## DOCKER VOLUME

#### 4. Create a named volume pv-0123475

```bash
docker volume create pv-0123475
cd /var/lib/docker/volumes/data/_data
nano index.html
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Web Page</title>
</head>
<body>
    <header>
        <h1>Welcome to My Website</h1>
    </header>

    <main>
        <section>
            <h2>About Me</h2>
            <p>This is a simple HTML page to showcase a basic structure.</p>
        </section>

        <section>
            <h2>Contact</h2>
            <p>You can reach me at <a href="mailto:example@example.com">example@example.com</a></p>
        </section>
    </main>

    <footer>
        <p>&copy; 2024 Your Name. All rights reserved.</p>
    </footer>
</body>
</html>
```
docker volume ls

#### 5. Make /tmp/baz as volume

```bash
mkdir /tmp/baz
cd /tmp/baz/
nano index.html
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Web Page</title>
</head>
<body>
    <header>
        <h1>Welcome to My Website</h1>
    </header>

    <main>
        <section>
            <h2>About Me</h2>
            <p>This is a simple HTML page to showcase a basic structure.</p>
        </section>

        <section>
            <h2>Contact</h2>
            <p>You can reach me at <a href="mailto:example@example.com">example@example.com</a></p>
        </section>
    </main>

    <footer>
        <p>&copy; 2024 Your Name. All rights reserved.</p>
    </footer>
</body>
</html>
```

#### 6. Connect volume to three containers and access data, but container03 should only read access.

```bash
docker run --name cont-01 -p 8081:80 -v pv-0123475:/usr/share/nginx/html/ nginx 

docker run --name cont-02 -p 8082:80 -v /tmp/baz/:/usr/share/nginx/html/ nginx 

docker run --name cont-03 -p 8083:80 -v pv-0123475:/usr/share/nginx/html/:ro nginx 
```

## DOCKER COMPOSE

#### 7. You need to run a 3-tier application on a containerized environment containing (mongo, nginx and Redis) and you will take help of Docker Compose, but with following conditions

#### a) Web container should be built with own image showing your name & Batch details

#### b) mongo db can be run directly

#### c) redis should run on image which is one version below latest

## DOCKER SWARM

#### 8)Install Docker Swarm and deploy 3 containers

#### 9) We need to create a web container but image can not be downloaded from Docker hub or local

#### 10) Install Portainer and deploy any two Application from templates list

## MISC

#### 11) We have 17 containers and we are unable to manage it properly, as a DevOps Engineer what is your suggestion

#### 12) Use Image (https://hub.docker.com/repository/docker/engineerbaz/app-devops-project1/) and test a container, but add another package of your choice and rebuilt the image to push on the registry

13) Get data from https://github.com/engineerbaz/DevOps-B07-TrainingCourse/blob/main/learningTasks/project-todoList.md and run as a project
