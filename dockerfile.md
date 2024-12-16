# Dockerfile

A `Dockerfile` is a text file that contains a series of instructions used to build a `Docker Image`. Each instruction in the Dockerfile specifies a step in the process of creating the image, such as setting up the base image, installing software, copying files, or defining environment variables.

### **Dockerfile Instructions**
- **`FROM`**: Specifies the base image to use.
  ```dockerfile
  FROM ubuntu:20.04
  ```
- **`RUN`**: Executes commands in the image (e.g., installing packages) at time of building image.
  ```dockerfile
  RUN apt-get update && apt-get install -y python3
  ```
- **`LABEL`**: Used to add metadata to an image in the form of key-value pairs.
  ```dockerfile
  LABEL version="1.0" description="My app"
  ```
- **`COPY`**: Copies files from the host system into the image at specified location.
  ```
  COPY /path/to/cource /path/to/destination
  ```
- **`WORKDIR`**: Sets the working directory location for remaining instructions.
  ```dockerfile
  WORKDIR /app
  ```
- **`ENV`**: Sets environment variables in the container..
  ```dockerfile
  ENV APP_ENV=production
  ```
- **`VOLUME`**: Volume is a persistent storage location that exists outside of the container. Volumes are useful for storing data that needs to persist even if the container is stopped or removed.
  ```dockerfile
  VOLUME ["/data"]
  ```
- **`CMD`**: Defines the default command to run when a container starts.
  ```dockerfile
  CMD ["python3", "app.py"]
  ```
- **`ENTRYPOINT`**: Defines the executable that always run when a container starts, It can be overridden with `CMD`. If both `ENTRYPOINT` and `CMD` are used, `CMD's` values are passed as arguments to `ENTRYPOINT`.
  ```dockerfile
  ENTRYPOINT ["python3", "app.py"]
  ```
- **`EXPOSE`**: Exposes a network port (for communication).
  ```dockerfile
  EXPOSE 8080
  ```
- **`ADD`**: Similar to COPY, but with extra features (e.g., auto-extracting tar, copy from URL).
  ```dockerfile
  ADD myapp.tar.gz /app/
  ```
- **`HEALTHCHECK`**: Defines a command to check the health of the container.             
  Syntax: HEALTHCHECK [OPTIONS] CMD <command>
  ```dockerfile
  HEALTHCHECK CMD curl http://localhost:8080
  ```
- **`USER`**: USER instruction sets the user to use when running the container. It creates a user, and the container will run user instead of the root user.
  ```dockerfile
  # Create a new user
  RUN useradd -m -s /bin/bash myappuser

  # Switch to the new user
  USER myappuser

  # Copy application files
  COPY . .

  # Expose the port
  EXPOSE 8080

  # Command to run the application
  CMD ["/app/my_app"]
  ```



Exaple of dockerfile with all possible instrcutions.


```dockerfile
# Use an official base image
FROM node:16

# Label metadata
LABEL maintainer="john.doe@example.com" \
      version="1.0" \
      description="A simple Node.js application"

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package.json package-lock.json ./

# Install application dependencies
RUN npm install

# Copy the entire application code into the container
COPY . .

# Set environment variables (optional)
ENV NODE_ENV=production

# Expose the port the app will run on
EXPOSE 3000

# Run a command to check dependencies (optional)
RUN ls -al /app

# Define the default command to run when the container starts
CMD ["npm", "start"]

# Define an entrypoint (optional, but useful for handling arguments)
ENTRYPOINT ["node", "server.js"]

# Healthcheck - Checks if the app is responding on port 3000
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD curl --silent --fail http://localhost:3000/ || exit 1
```









