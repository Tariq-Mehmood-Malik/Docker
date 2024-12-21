# Docker Volume

A Docker volume is a persistent storage mechanism used to store data outside of a container’s filesystem. Unlike data stored inside containers, which is lost when the container is deleted, volumes are stored on the host system and remain intact even if containers are stopped or removed. Volumes allow data to persist across container restarts, share data between containers, and provide better performance and management for data storage compared to using the container's internal filesystem. They are commonly used for storing application data, logs, or databases.

## Docker Storage Types

Docker provides two main types of storage for containers: `Named Volumes` and `Bind Mounts`.

### 1. Named Volumes
This is managed by Docker, stored in a specific directory on the host system, and isolated from the host filesystem. Best for persistent data that should be handled by Docker (e.g., databases, application data). Can only be changed by Docker Group users only.
  ```bash
  docker volume create my_volume      # to create named volume
  docker run -v my_volume:/data my_container    # to attach named volume to container
  ```

### 2. Bind Mounts
This mounts a specific file or directory from the host system into the container. Useful for syncing files between the host and container or when you need direct control over the host filesystem.     
  ```bash
  docker run -v /host/path:/container/path my_container
  ```

## Other type of docker volume

### tmpfs Mount  
A tmpfs mount in Docker is a type of temporary file storage that is stored in memory (RAM) rather than on disk. It provides a fast, in-memory filesystem for containers to store data that doesn't need to persist after the container is stopped or removed. This is useful for sensitive or ephemeral data that doesn't need to be saved for long-term.

You can mount a **tmpfs** in a Docker container using the `--tmpfs` flag:

```bash
docker run -d --tmpfs /tmp:rw,size=100m my_container
```
`--tmpfs /tmp:rw,size=100m`: Mounts a `tmpfs` filesystem at the `/tmp` directory inside the container, with read-write (`rw`) permissions and a maximum size of 100 MB.
