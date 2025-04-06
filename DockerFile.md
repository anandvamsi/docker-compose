# DockerFile 
A Dockerfile is a text file that contains a set of instructions used to build a Docker image. 
These instructions define how to construct the filesystem and environment inside a Docker container.


## To create a Dockerfile, you'll typically follow these steps:

- ```Choose a Base Image```: ecide which base image best suits your application's needs.
  This is the starting point for your Docker image

- ```Write the Dockerfile:```: reate a text file named Dockerfile (without any file extension)
   and define the instructions for building your Docker image.

- ```Define Instructions:```   In the Dockerfile, use a series of instructions to specify how to build your image.
  Common instructions include FROM, COPY, RUN, EXPOSE, CMD, and ENTRYPOINT, among others.

- ```Build the Docker Image: ``` Use the docker build command to build your Docker image based on the instructions in the Dockerfile.
- ```Test and Iterate:``` Once the image is built, you can run containers based on it to test your application. If needed, you can iterate on your Dockerfile to make adjustments and improvements.


## Here is the sample docker file 
```
# Use an official Python runtime as the base image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed dependencies specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

## Understanding Docker file components.
```Base Image (FROM)```: instruction specifies the base image that the Docker image will be built upon. It is the starting point for your Docker image. 
You typically choose a base image that provides the necessary operating system and runtime environment for your application.

```Working Directory (WORKDIR):```: It is similar to the cd command in a shell. All relative paths used in the Dockerfile will be relative to this working directory.

```Copy Files (COPY / ADD):``` The COPY instruction copies files or directories from the host machine into the Docker image. It takes two arguments: the source path on the host and the destination path in the image.
Alternatively, you can use the ```ADD``` instruction, which has similar functionality to COPY but also supports URL downloads and automatic extraction of compressed file

```Install Dependencies (RUN):``` The RUN instruction executes commands within the Docker image during the build process. 
You can use RUN to install packages, run scripts, or perform any other necessary setup tasks.

```Expose Ports (EXPOSE):``` The EXPOSE instruction informs Docker that the container listens on specific network ports at runtime. 
It does not actually publish the ports; it simply documents which ports should be published when running the container.

```ENV``` : The ENV instruction sets environment variables inside the Docker image. 

```Define Startup Command (CMD / ENTRYPOINT):``` The CMD instruction specifies the default command to run when the container starts. It can be overridden at runtime by providing command-line arguments.
Alternatively, you can use the ENTRYPOINT instruction to specify the main command to run, with CMD providing default arguments.

```ENTRYPOINT``` defines the main command that always runs when the container starts. It sets the primary purpose of the container — the thing it was built to do.

## Understanding the difference between CMD and ENTRYPOINT.

ENTRYPOINT specifies the executable to run when the container starts. It defines the default application that will be run within the container.
CMD specifies the default command and/or parameters for the container.
```bash
FROM ubuntu
ENTRYPOINT ["echo"]
CMD ["Hello, World!"]
```

## Example 1:  Create a docker image for a python application
### Docker file
```bash
# Use the official slim Python image
FROM python:3.9-slim

# Set the working directory inside the container
WORKDIR /app

# Copy only requirements first to leverage Docker's layer caching
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application files
COPY . .

# Expose the application port
EXPOSE 5000

# Run the application
CMD ["python", "app.py"]
```

### Code for the app.py
```bash
from flask import Flask
app = Flask(__name__)
@app.route('/')
def home():
    return "Hello, Dockerized Flask App!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### code for requiremnt.txt
```
flask
```
### Execute the code 
```bash
docker build -t my-flask-app .
docker run -p 5000:5000 my-flask-app
```


## Arguements in docker
```bash
ARG PYTHON_VERSION=3.12
FROM python:${PYTHON_VERSION}
```
### Overide docker arguments
```bash
docker build --build-arg PYTHON_VERSION=3.10 -t myapp:3.10 .
```

### Why should we use Slim images
#### Smaller Size 
Slim images are significantly smaller than their full versions.
Example:
```
python:3.9 → ~900MB
python:3.9-slim → ~22MB
```
Smaller images reduce disk usage and download times.
####  Faster Build and Deployment
#### Improved Security
slim images remove unnecessary system utilities, reducing security vulnerabilities.
Fewer attack vectors mean a more secure container.
#### Lower Memory and CPU Usage
Less overhead compared to full-sized images.
Better for cloud environments like AWS, Kubernetes, and OpenShift where resource efficiency matters.


## Difference between entrypoint and cmd.
- Use ENTRYPOINT when you always want the container to run a specific command.
- Use CMD to provide default arguments or fallback values.
### Here is an example 
```bash
FROM alpine
ENTRYPOINT ["echo"]
CMD ["Hello from CMD"]
```

## Overide entrypoint and cmd.

### You can override cmd using command
```bash
docker run hello:v2 "testing"
testing
```

### you can override using below command
```bash
docker run --entrypoint <entry-point command> hello:v2
```
