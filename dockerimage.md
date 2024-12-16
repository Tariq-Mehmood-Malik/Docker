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
