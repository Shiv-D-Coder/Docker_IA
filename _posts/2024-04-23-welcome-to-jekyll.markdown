---
layout: post
title:  "Dockerizing a Three-Tier Application"
date:   2024-04-23 10:19:43 +0530
categories:  jekyll update
---
 
# Dockerizing a Three-Tier Application and Pushing Images to Docker Hub

In this blog post, I'll walk you through the detailed process of dockerizing a three-tier application and pushing the Docker images to Docker Hub. We'll be using a simple MERN chat-aplication for this.
 
### Overview
- The `public` directory contains the frontend React application.
- The `server` directory contains the backend Node.js application.

## Installation Guide

### Requirements
- [Node.js](https://nodejs.org/)
- [React](https://react.dev/)
- [Docker](https://www.docker.com/)

Make sure Node.js and Docker are installed on your system.


## Step 1. Clone the Repository

```shell
git clone https://github.com/your-username/mern-chat-application.git

## Step 2: Make Docker file

1. Go to project directory then create **Dockerfile** in that directory(pulic/Dockerfile):

### DockerFile in public folder for forntend
```bash
  # Using node:16 as base image
  FROM node:16-alpine

  WORKDIR /app

  COPY package*.json ./

  RUN npm install

  COPY . .

  EXPOSE 3000

  CMD ["npm","start"] 
```

2. Now create **Dockerfile** in  directory(server/Dockerfile):

### DockerFile in public folder for forntend
```bash
  # Using node:16 as base image
  FROM node:16-alpine

  WORKDIR /app

  COPY package*.json ./

  RUN npm install

  COPY . .

  EXPOSE 5000

  CMD ["npm","start"]
```

## Dockerfile Explanation for (public/Dockerfile)

### `FROM node:16-alpine`

- This line specifies the base image to use for building our Docker image. In this case, we are using the Alpine

### `WORKDIR /app`

- The `WORKDIR` instruction sets the working directory inside the container to `/app`. This is where all subsequent instructions will be executed.

### `COPY package*.json ./`

-  This line copies the `package.json` file from your local machine to the container’s `/app` directory. The `.` at the end.


### `RUN npm install`

- Runs npm install inside the container to install the required Node.js dependencies for the React app.

### `COPY . .`

- Copies all files from your local project directory into the working directory of the container at `/app`.

### `EXPOSE 3000`

- The `EXPOSE` instruction informs Docker that the container will listen on port `3000` at runtime. This does not actually publish the port, but it serves as a documentation for developers to know which port to expose.

### `CMD ["npm", "start"]`

- Specifies the command to run when the container starts. It runs npm start to start the React application.

## Dockerfile Explanation for (server/Dockerfile)

### `FROM node:16-alpine`

- This line specifies the base image to use for building our Docker image. In this case, we are using the Alpine

### `WORKDIR /app`

- The `WORKDIR` instruction sets the working directory inside the container to `/app`. This is where all subsequent instructions will be executed.

### `COPY package*.json ./`

-  This line copies the `package.json` file from your local machine to the container’s `/app` directory. The `.` at the end.


### `RUN npm install`

- Runs npm install inside the container to install the required Node.js dependencies for the React app.

### `COPY . .`

- Copies all files from your local project directory into the working directory of the container at `/app`.

### `EXPOSE 500`

- The `EXPOSE` instruction informs Docker that the container will listen on port `5000` at runtime. 

### `CMD ["npm", "start"]`

- Specifies the command to run when the container starts. It runs npm start to start the React application.


## Step 3: Making **docker-compose.yml** file

```bash
  version: '3.8'
  # services  section contains the service configurations
  services:
  # build image for frontend
    front:
      build: ./public
      ports:
        - 3000:3000
  # build image for backend      
    api:
      build: ./server
      ports:
       - 5000:5000
```

### 3.1 Run docker compose

```bash
  docker-compose up -d
```
*make sure you are on same Directary as /app and if you get any error making image you shold try to restart Docker Desktop and make sure you are signed in*

now it will make 2 containers having each one image :

To stop it you can use
```bash
  docker-compose stop
```

**NOTE: Here shiv37 is my username on Docker hub if you do not have account on docker hub first create it. It is not mandatory to create but it is advisable to create one.Here is link for you [Dockerhub](https://hub.docker.com/)** 


## 3.2 Uploding image to Docker hub

To uplode you image on Dockerhub you shold first login into your account using the below command:

```bash
  docker login
```
It might ask you for your username and password make sure to type it correctly. After successful authentication you can push your image by running

```bash
  docker push shiv37/mern-app-front

  docker push shiv37/mern-app-api
```

**Note that if you don't have an account yet, you will be asked to create one. After creating the account you can proceed with pushing**

I have alrady  pushed my image to DockerHub so you can pull that image directly by running the following command:You can also specify the tag while pushing an imageYou can check if everything is done correctly by running:

```bash
  [docker push shiv37/mern-app-front](https://hub.docker.com/repository/docker/shiv37/mern-app-front/general)

  [docker push shiv37/mern-app-api](https://hub.docker.com/repository/docker/shiv37/mern-app-api/general)
```