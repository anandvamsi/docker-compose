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


## Understanding the difference between CMD and ENTRYPOINT.
