# Docker Image

A `Docker image` is a lightweight, standalone, and executable package that contains everything needed to run a piece of software. It includes the application code, libraries, dependencies, environment variables, configuration files, and more, all bundled into a single image.

## Key Concepts

### 1. **Layered File System**
Docker images are built in layers. Each layer represents a change, such as adding a file or installing a package. Layers are cached, improving performance and reducing duplication across images.

### 2. **Base Image**
A base image serves as the foundation of a Docker image. It could be a minimal operating system like `ubuntu` or an application-specific image like `node`, `python`, etc.

### 3. **Immutable**
Once a Docker image is created, it is immutable. To make changes, you need to create a new image based on the previous one, rather than modifying the existing one.

### 4. **Dockerfile**
A Docker image is usually built from a `Dockerfile`. This is a script containing instructions on how to assemble the image, such as:
- The base image to use
- The software dependencies to install
- The files to copy
- The commands to run

### 5. **Reusability**
Docker images are reusable and portable. You can share images across different systems using Docker Hub or private registries.
   
---   

## Docker Images Commands

To pull image from Docker-Hub to local host
```bash
docker pull nginx:latest
```
<br>

To create container `nginx` latest version, after pulling image from Docker-Hub if not locally present, with name tag `my-container` but not start it :
```bash
docker create --name my-container nginx
docker start my-container        # to start container if created before
```
<br>

To create & run container with image `nginx` latest version. `-d` run in detach mode (foreground):   
```bash 
docker run -d --name my-container nginx
```
<br>

To list all local images
```bash
docker image ls

docker images
```
<br>

To inspect (view all details) image 
```bash
docker image inspect <image-name>
```
<br>

To build image from `Dockerfile` present in current directory
```bash
docker build -t <image-name>:<tag> .      # . means scan current directory for Dockerfile
```
<br>

Updating tag of custom image before uploading it to docker-hub
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

