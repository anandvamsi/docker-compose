# Docker Network
Docker networks configure communications between ```neighboring containers and external services```. Containers must be connected to a Docker network to receive any network connectivity.
The communication routes available to the container depend on the network connections it has.

There are different types of container types
- bridge
- host
- overlay
- Macvlan

## Bridge- Default network
-  Containers can communicate with each other within the same network but are isolated from external networks unless explicitly exposed..
- Each container in the network is assigned its own IP address. Because the network’s bridged to your host, containers are also able to communicate on your LAN and the internet.

```bash
docker inspect 9b356e563177| grep -i Network
            "NetworkMode": "bridge",
```


## Host
In Docker, the "Host Network" mode allows a container to share the ```network namespace with the Docker host```. 
This means that the container directly uses the networking stack of the host system rather than getting its own isolated network stack.
```Performance Benefits:``` Because the container bypasses Docker's network abstraction layer, 
it can achieve better network performance compared to other network modes like bridge or overlay

## Overlay
Overlay networks are ```distributed networks that span multiple Docker hosts```. 
The network allows all the containers running on any of the hosts to communicate with each other without requiring OS-level routing support
Overlay networks are required when containers on different Docker hosts need to communicate directly with each other. 
These networks let you set up your own distributed environments for high availability.


## Resolving a container by hostname
### step0:- Create bridged network
```bash
docker network create my_custom_network
```

### step1:- Create a bridnged network and attach two containers to the network.
```bash
docker run -itd --name ng4 --network my_custom_network debian
docker run -itd --name ng3 --network my_custom_network debian
```
### step2: Install the ping command
```bash
apt-het update
apt install -y iputils-ping
```

 ### Step3: Checking the ping connectivity
 ```bash
from the ng4 host :- ping ng3
from the ng3 host :- ping ng4
```



## Create a network 

### Step 1:-Creating a network
```bash
docker network create my_custom_network
b21423dc4805357585b413e204c9c3c0313d16bfbb252accedccf9527b430736
```

## we can also create a network bridged with defined network and gateway.
```bash
docker network create \
  --driver bridge \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  my_custom_bridge
```

### Step 2:- Create a bridged network.
```bash
docker run -d \
  --name mysql-db \
  --network my_custom_network \
  -e MYSQL_ROOT_PASSWORD=rootpass \
  -e MYSQL_DATABASE=mydb \
  -e MYSQL_USER=user \
  -e MYSQL_PASSWORD=pass \
  mysql:latest
```

### Step 3:-  Create a python flask file to front end container 
```bash
from flask import Flask
import mysql.connector

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Flask Backend!"

@app.route('/db')
def db_check():
    try:
        conn = mysql.connector.connect(
            host="mysql-db",
            user="user",
            password="pass",
            database="mydb"
        )
        return "Connected to MySQL!"
    except:
        return "Database connection failed!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### Step 4:- Create a Front end Docker image.
```bash
FROM python:3.9
WORKDIR /app
COPY app.py .
RUN pip install flask mysql-connector-python
CMD ["python", "app.py"]
```

### Step 5:- Create a Container from the image created in step 4
```bash
docker build -t flask-backend .
docker run -d --name backend --network my_custom_network -p 5000:5000 flask-backend
```

### Step 6: - Create a nginx conf file in the host machine.
```bash
server {
    listen 80;
    
    location / {
        proxy_pass http://backend:5000/;
    }
}
```
### Step 7: Create a volume mount with for a new nginx container.
```bash
docker run -d --name frontend \
  --network my_custom_network \
  -p 8080:80 \
  -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf:ro \
  nginx:latest
```

### Step 8 : Verify the same in the URLs
```bash
http://localhost:5000/
http://localhost:5000/db
```

### Summery
```bash
✅ We created a custom Docker network
✅ Launched a MySQL database inside the network
✅ Set up a Flask backend to communicate with the database
✅ Configured Nginx as a reverse proxy for the backend
✅ Verified that all services communicate via the Docker network
```

