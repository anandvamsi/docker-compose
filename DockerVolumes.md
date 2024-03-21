# Docker volumes.
- Create a docker volume.
- List the docker docker volumes.
- Inspecting the properties of docker volume
- Mounting the docker container to new created volume.


## Create a docker volume 
```bash
$ docker volume create my-vol
```

## Listing the available volumes
```bash
docker volume ls
local               my-vol
```


## Inspect a volume
```bash
$ docker volume inspect my-vol
[
    {
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/my-vol/_data",
        "Name": "my-vol",
        "Options": {},
        "Scope": "local"
    }
```

## Mounting a volume to conatiner
```bash
docker run -d \
  --name devtest \
  --mount source=myvol2,target=/app \
  nginx:latest
  
  --->>------
  "using -v option"
  $ docker run -d \
  --name devtest \
  -v myvol2:/app \
  nginx:latest
```

Note:- we can mount different container to same volume storage
