# Docker Registry

- what is docker registry
- Dockerhub Account Creation
- Docker login,logout
- Docker push,pull

## Docker registry
Docker registries play a crucial role in the Docker ecosystem by providing a centralized location for storing and sharing Docker images. 
This allows developers to ```easily distribute their applications and services```, as well as collaborate with others by sharing their images through the registry.

There are both public and private Docker registries available. Public registries, such as Docker Hub, allow anyone to upload and download images freely. 
```Private registries```, on the other hand, are often used by organizations to securely store proprietary or sensitive images within their own infrastructure


## How to upload and download docker image

### Docker push
```bash
docker push anandvamsiv/generalimages:ubuntussh
The push refers to repository [docker.io/anandvamsiv/generalimages]
dae8403eaee1: Layer already exists
a0263510adad: Layer already exists
8e8951781b5c: Layer already exists
13f01e2bda0a: Layer already exists
5498e8c22f69: Layer already exists
ubuntussh: digest: sha256:cefa959886ac2f3d079498d10b31b337aeddf13da7bdcfe64f3e1ac7eac1e812 size: 1363
```

### Docker pull 
```bash
docker pull anandvamsiv/generalimages:ubuntussh
```
