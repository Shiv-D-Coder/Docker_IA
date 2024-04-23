---
layout: post
title:  "Dockerizing a Three-Tier Application"
date:   2024-04-23 10:19:43 +0530
categories:  jekyll update
---
 
# Dockerizing a Three-Tier Application and Pushing Images to Docker Hub

In this blog post, I'll walk you through the detailed process of dockerizing a three-tier application and pushing the Docker images to Docker Hub. We'll be using a simple Todo application built with Flask and SQLite as our example.
 
## Step 1: Setting Up the Project
 
### 1.1 Cloning the Todo App Repository
First, let's clone the Todo application repository from GitHub:
 
```bash
  git clone https://github.com/pj8912/todo-app.git
```

This is a simple **Todo application** built using *Python* and *SQLite*. The application allows users to add, view, and delete tasks. The data is stored in a SQLite database and the front-end is rendered using the Jinja2 template engine.

## 1.2 Requirements
- Python 3.x
- Flask
- SQLite3

## Step 2: Make Docker file

Go to project directory then create **Dockerfile** in that directory:

### DockerFile
```bash
  # Use official lightweight Python image
  FROM python:3.10-slim

  # Set working directory
  WORKDIR /app

  # Copy local code to the container workdir
  COPY requirements.txt .

  # Installing requirements.txt
  RUN pip install  -r requirements.txt

  # copy  the current folder to the container workdir
  COPY . .

  # Make database
  RUN python db_create.py

  # Expose the port the app runs on
  EXPOSE 5050

  # Command to run the application
  CMD ["python", "app.py"]
```

## Dockerfile Explanation

This Dockerfile is used to create a Docker image for a Python-based application, specifically a Flask app with a SQLite database. Let's break down each instruction:

### `FROM python:3.10-slim`

- This line specifies the base image to use for building our Docker image. Here, we're using the official lightweight Python image with Python 3.10.

### `WORKDIR /app`

- The `WORKDIR` instruction sets the working directory inside the container to `/app`. This is where all subsequent instructions will be executed.

### `COPY requirements.txt .`

- This instruction copies the `requirements.txt` file from the local directory (where the Dockerfile is located) into the `/app` directory in the container. This file typically lists all the Python packages required by the application.

### `RUN pip install -r requirements.txt`

- The `RUN` instruction executes a command in the container. Here, we're using it to install the Python packages listed in `requirements.txt` using `pip install`.

### `COPY . .`

- This line copies all the files and folders from the local directory into the `/app` directory in the container. This includes our Flask application code, such as `app.py`, templates, and static files.

### `RUN python db_create.py`

- This `RUN` command runs the `db_create.py` script inside the container. This script is responsible for creating the SQLite database for our application. It sets up the necessary tables and schema.

### `EXPOSE 5050`

- The `EXPOSE` instruction informs Docker that the container will listen on port `5050` at runtime. This does not actually publish the port, but it serves as a documentation for developers to know which port to expose.

### `CMD ["python", "app.py"]`

- Finally, the `CMD` instruction specifies the command to run when the container starts. Here, it runs the Flask application by executing `python app.py`.

### Summary

This Dockerfile sets up a Python environment inside the container, installs the required packages, copies the application code, creates the SQLite database, and specifies the command to start the Flask application. When an image is built from this Dockerfile and a container is run, it will execute the Flask app, making it accessible on port 5050 within the container.

## Step 3: Making Docker image and uploding it to  Docker Hub

### 3.1 To make a Docker image from our Dockerfile, we use the following command in terminal:

```bash
  docker build -t shiv37/to-do:v1 .
```
*make sure you are on same Directary as app.py and if you get any error making image you shold try to restart Docker Desktop and make sure you are signed in*

now make a container and run it  with this command :

```bash
  docker run --name worklist <image_name> shiv37/to-do:v1
```

**Or**

You can also run image directly by specifing ports by follwing command but remembere it will create one container by itself by any random name

```bash
  docker run -it -p 5050:5050 shiv37/to-do:v1
```

**NOTE: Here shiv37 is my username on Docker hub if you do not have account on docker hub first create it. It is not mandatory to create but it is advisable to create one.Here is link for you [Dockerhub](https://hub.docker.com/)** 

### 3.2 Uploding image to Docker hub

To uplode you image on Dockerhub you shold first login into your account using the below command:

```bash
  docker login
```
It might ask you for your username and password make sure to type it correctly. After successful authentication you can push your image by running

```bash
  docker push shiv37/to-do:v
```

**Note that if you don't have an account yet, you will be asked to create one. After creating the account you can proceed with pushing**

I have alrady  pushed my image to DockerHub so you can pull that image directly by running the following command:You can also specify the tag while pushing an imageYou can check if everything is done correctly by running:

```bash
  docker pull shiv37/to-do:v
```