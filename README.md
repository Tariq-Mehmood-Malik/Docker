# Docker
---
## Table of Contents

- [What is Docker](#what-is-docker)
- [Docker Engine Installation](#docker-engine-installation)
- [Docker Desktop Installation](#docker-desktop-installation)
- [Building a website on Nginx](#building-a-website-on-nginx)
---
# What is Docker
Docker is a platform that simplifies the development, delivery, and operation of applications by using containers. It allows you to bundle an application with everything it needs, such as libraries and dependencies, into a single container. This container can then be run on any system that supports Docker, without needing to change the host system.

### Docker Key Terms

1. **Docker Images**:     
   These are like blueprints for containers, containing everything the container needs, including the operating system, libraries, and application code. You create images using a Dockerfile, and they can be shared or reused.
2. **Docker Containers**:    
3. These are live instances of images. They run in their own isolated environment, sharing the host’s core system, but remaining lightweight and portable.
3. **Dockerfile**:             
   A Dockerfile is a text file with a series of commands for creating an image.
4. **Docker Volumes**:           
    Used for storing data that you need to keep, even if the container restarts or deleted.
5. **Docker Engine**:           
   The core component that runs and manages Docker containers, consisting of the Docker daemon, REST API, and the Docker CLI.
   
## Docker Architecture
In Docker's architecture, the Client, Host, and Registry are key components:

![Docker Architecture](images/01.webp)  

1. **Client**:          
   The Docker client is the interface through which users interact with Docker. It can be a command-line interface (CLI) or a graphical user interface (GUI). The client sends requests to the Docker daemon (server) via REST API to build, run, and manage containers. It can be on the same machine as the Docker daemon or on a remote system.

2. **Host**:         
   The Docker host is the machine (physical or virtual) that runs the Docker daemon.It is responsible for building, running, and managing containers. The Docker daemon listens for API requests from the Docker client and communicates with other components, like the registry, to fetch or store images.

3. **Registry**:             
   A registry is a storage system for Docker images. Docker Hub is the default public registry, but private registries can also be used. The registry stores Docker images, which are used to create containers. The Docker client can pull images from the registry or push images to it.

## Docker Container Lifecycle
Docker containers complete life cycle are managed by Docker daemon, Runc, Shim, and Containerd.                  
![Docker Life](images/02.webp)  

1. **Docke-Daemon** (`dockerd`):          
   It is the central server that manages Docker containers. It listens for Docker API requests (from the Docker CLI) and coordinates the container lifecycle. It interacts with `containerd` to manage container creation, execution, and destruction.

2. **containerd**:            
   It is a high-level container runtime responsible for pulling images, creating container, and managing containers. It communicates with `runc` to start containers and `shim` to maintain their lifecycle.

3. **runc**:          
   It is a low-level container runtime that sets up the container’s namespaces, cgroups, and other Linux features to ensure process isolation and resource management. It runs the actual container’s process.

4. **shim**:             
   It is a lightweight tool that ensures the container’s main process stays running, even after the Docker CLI disconnects. It manages logging and other metadata while working with `containerd` and `runc`.

### Workflow:      
Docker CLI sends a command to the Docker Daemon. The Daemon communicates with containerd to manage the container lifecycle. containerd pulls the container image and invokes runc to set up and run the container. shim keeps the container’s process running and ensures logging and metadata are handled. containerd and shim continue to manage the container life until it's stopped/killed.


## Basic Docker Commands

- `docker run`: Start a new container from an image.
- `docker ps`: View a list of containers that are currently running.
- `docker stop`: Stop a running container.
- `docker rm`: Delete a stopped container.
- `docker build`: Create a new image from a Dockerfile.
- `docker push`: Upload an image to a registry (like Docker Hub).
- `docker pull`: Download an image from a registry.

---
# Docker Engine Installation
Following are steps to install docker engine on Ubuntu linux.
1. Update system `apt` source:
```bash
sudo apt update
```
![Updating apt](images/1.jpg)   

2. Add Docker's official GPG key
```bash
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```               
![Adding Docker's Official GPG key](images/2.jpg)   

3. Add the repository to `apt` sources
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
![Adding docker repository to system apt](images/3.jpg)   

4. Install docker engine (complete) with following command
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
![Installing Docker](images/4.jpg)   

                 
5. Verify that the installation by running `hello-world` image:
```bash
sudo docker run hello-world
```
![Running hello-world docker](images/5.jpg)    
       
6. (Optional) Add user to `docker` group to run Docker commands without needing to use sudo everytime.
```bash
sudo usermod -aG docker  <user-name>
```
![Adding user to docker group](images/6.jpg)   
Now log out and back log in (or restart system) for the changes to take effect.

---
# Docker Desktop Installation
Following are steps to install Docker-Desktop on any debian based linux system.
1. (i) If you are installing `Docker-Desktop` on system having [Gnome](https://www.gnome.org/) Desktop enviroment you must also install [AppIndicator and KStatusNotifierItem](https://extensions.gnome.org/extension/615/appindicator-support/).                   
   (ii) If you are installing gnome on Debian based system having Desktop enviroment other than Gnome `gnome-terminal` must be installed:
    ```bash
    sudo apt install gnome-terminal
    ```      
![Gnome-Terminal](images/s1.jpg)   

2. Set up Docker's apt repository. See step 1 to 3 on [Docker Engine Installation](#docker-engine-installation) section.
3. Download latest version of [Deocker-Desktop.deb](https://desktop.docker.com/linux/main/amd64/docker-desktop-amd64.deb?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-linux-amd64).
4. Open terminal in directory where Docker-desktop.deb package is downloaded and run following command.
   ```bash
   sudo apt-get install ./docker-desktop-amd64.deb
   ```        
![Install-Desktop](images/s2.jpg)   
5. Open Docker-Desktop through GUI or through following command:
   ```bash
   systemctl --user start docker-desktop
   ```      
![Install-Desktop](images/s2.png)



