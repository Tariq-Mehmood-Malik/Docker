# Multi-Stage Dockerfile & Distroless Images

## Multi-Stage Dockerfile    
A multistage Dockerfile helps create smaller and cleaner Docker images by separating the process of building your application from the process of running it. In a normal Dockerfile, all dependencies (like tools needed to build your app) are included in the final image, which makes it large and potentially inefficient. With a multistage Dockerfile, you can use one stage to build your app (with all the necessary tools), and then use a second, cleaner stage to create a smaller image that only includes what’s needed to run the app.

For example, let’s say you're building a Node.js application. In the first stage, you would use a full Node.js image to install dependencies and build the app. In the second stage, you switch to a smaller Node.js image (called node:slim), and only copy the built app from the first stage, leaving behind unnecessary build tools and dependencies. This makes the final image smaller, faster to download, and more secure since it doesn't contain unneeded build files.



[Link](https://github.com/iam-veeramalla/Docker-Zero-to-Hero/tree/main/examples/golang-multi-stage-docker-build)



https://8grams.medium.com/distroless-using-minimal-container-image-for-kubernetes-workload-9b143f83bd10

