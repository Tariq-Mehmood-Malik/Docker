# Docker Tasks

---

## Task 1
Create an image of a webpage of your choice.You may upload it to Docker Hub.
- Creating `index.htlm`
  ```bash
  nano index.html
  ```
  
  `index.html` content.
  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Simple Page</title>
  </head>
  <body>
      <h1>Welcome to My Website</h1>
      <p>This is a simple HTML page.</p>
  </body>
  </html>
  ```    
  ![01](01.png)
     
- Creating `Dockerfile` for nginx webserver.
  ```bash
  nano Dockerfile
  ```
  `Dockerfile` content:
  ```
  FROM nginx:latest
  MAINTAINER tariq
  COPY . /usr/share/nginx/html/
  ```
  ![02](02.png)

- Builtiding image name `webserv:v1`
  ```bash
  docker build -t webserv:v1 .
  ```
  ![03](03.png)
  
- Updating tag and upload it on docker-hub.
  ```bash
  docker tag webserv:v1 tariqmehmoodmalik/test-repo:v1
  docker push tariqmehmoodmalik/test-repo:v1
  ```
  ![04](04.png)

- verifying from Docker-Hub.        
  ![05](05.png)
  
---

## Task 2
Create 2 networks of Bridge Type named as `n01` and `w02`. Make two different containers, each in one of the networks.

---

## Task 3
Using the image from `engineerbaz/dockerlabs`, create a container to show output.

---

## Task 4
Get data from [this link](https://github.com/engineerbaz/DevOps-B07-TrainingCourse/blob/main/learningTasks/project-todoList.md) and run it as a project.

---

## Task 5
Create a file named `Dockerfile` for a modified CentOS (using the official CentOS image as a parent image), which has `figlet`, `ping`, `curl` tools, and a tool of your choice.

Save the file and build it. Now, check if the image is present by running `docker images` and start a container.
