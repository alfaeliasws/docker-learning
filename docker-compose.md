# Docker Compose

### INTRODUCTION TO DOCKER COMPOSE
- Docker compose is the tool that make defining and running multiple docker container in one platform
- In using docker compose we can use YAML to configure docker container
- And with one command we can create and run all the docker component
- So we should not use docker create command manually when we want to create docker container
### DOCKER COMPOSE FEATURE
- Have mulitple isolated env in one docker host/server (project) so we could have so many enviironment in docker compose
- Make the container change, so docker compose won't re create container but only change it when the file config is changing
### WHEN TO USE DOCKER COMPOSE
- To use development environment
- For automated testing configuration
- For deployment configuration
### INSTALL DOCKER COMPOSE
In Newer Docker version, docker compose is inside the docker
```
docker compose

# check the version
docker compose version
```
### CONFIGURATION FILE
- in YAML (just like json)
- naming: docker-compose.yaml
- The project name is the name of the folder where the docker-compose.yaml is in
- Configuration file version and docker file version is different
### YAML
- Just like JSON without curly brackets
```yaml
firstname: John
middlename: Mike
lastname: Johnson # equivalent to JSON "key":"value"
hobbies: # equivalent to JSON array
  - reading
  - coding
  - playing music
  - sociallizing 
address: # equivalent to JSON nested object
  street: Jalan jalan men
  city: Bandung
  coountry: Indonesia
eallet: # equivalent to JSON array of objects
  - type: cash
    amount: 29999
  -type: debit
    amount: 50999999999999
```
### cREATE CONTAINER CONFIGURATION
- We can make container from docker compose configuration file without having to do docker create in everytime
- In yaml we could add services to do the container manipualation
- In that service we could determine the name and the image that we use for the container
- After the compose is done we should create the docker container using compose with this command inside the folder that have the docker-composer.yaml
```sh
docker composer create
```
- It will not recreate the container that is already existed
### START CONTAINER CONFIGURATION
```
docker compose start
```
- To start all the docker containers inside the docker compose config
### SEE CONTAINER 
- To see all the containers that is running that is inside the docker compose
```
docker compose ps
```
### STOP CONTAINER 
- To stop (not removing) all the containers that is inside the docker compose
```
docker compose stop
```
### REMOVE CONTAINER 
- To remove all the containers and network and volume that is inside the docker compose (even the container that is started will be stopped down and removed)
```
docker compose down
```
### SEE PROJECT 
- To see all the project that is running that is inside the docker compose
```
docker compose ls
```
### COMPOSE: SERVICE
- Service = Container
### COMPOSE: COMMENT
- Hashtag in yaml is comment
### PORT
- We can do port forwarding in docker compose using ports attribute
- Ports attribute is inside the service that we want the port to be exposed for
- Ports attribute has value that is array of object port
- Short Syntax
	- HOST: our own PC port
	- CONTAINER: default container port
```yaml
port HOST:CONTAINER
```
- Long Syntax
	- Object port that contains
		- target: Port inside the container
		- published: Port of our PC that we want to expose to docker container
		- protocol: tcp/ip
		- mode: host for port in every node, ingress for swarm. Because we don't use docker swarm, we will only use host value
### ENVIRONMENT
```yaml
services:
  mongodb-example:
    container_name: mongodb-example
    image: mongo:latest
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: john
      MONGO_INITDB_ROOT_PASSWORD: john
      MONGO_INITDB_DATABASE: admin 
```
### BIND mOUNT
#### SHORT SYNTAX
```yaml
services:
  mongodb1:
    container_name: mongodb1
    image: mongo:latest
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: john
      MONGO_INITDB_ROOT_PASSWORD: john
      MONGO_INITDB_DATABASE: admin 
    volumes:
      - "./data-mongo1:/data/db"
```
#### LONG SYNTAX
```yaml
services:
  mongodb2:
    container_name: mongodb2
    image: mongo:latest
    ports:
      - "27018:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: john
      MONGO_INITDB_ROOT_PASSWORD: john
      MONGO_INITDB_DATABASE: admin 
    volumes:
      - type: bind
        source: "./data-mongo2"
        target: "/data/db"
        read_only: false
```
### VOLUME
- Volume must be used by the service
- Volume must be deleted manually by using ```docker volume rm volumeName```
``` yaml
services:
  mongodb1:
    container_name: mongodb1
    image: mongo:latest
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: john
      MONGO_INITDB_ROOT_PASSWORD: john
      MONGO_INITDB_DATABASE: admin 
    volumes:
      - "mongo-data1:/data/db"
  mongodb2:
    container_name: mongodb2
    image: mongo:latest
    ports:
      - "27018:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: john
      MONGO_INITDB_ROOT_PASSWORD: john
      MONGO_INITDB_DATABASE: admin 
    volumes:
      - type: volume
        source: "mongo-data2"
        target: "/data/db"
        read_only: false

volumes:
  mongo-data1:
    name: mongo-data1
  mongo-data2:
    name: mongo-data2
```
### NETWORK
- All the container that is in the same compose file will be in the one network by default (name convention: ```name-project_default```
)
- Network that is created must be injected to service (container) so it can be created
- In compose down, the network will be removed (not like the volume)
```yaml
services:
  network1:
    container_name: network1
    image: mongo:latest
    ports:
      - "27018:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: john
      MONGO_INITDB_ROOT_PASSWORD: john
      MONGO_INITDB_DATABASE: admin 
    networks:
      - network_example

networks:
  network_example:
    name: network_example
    driver: bridge
```
### DEPENDS ON
- To make a container that depends on other container
```yaml
services:
  mongodb-example:
    container_name: mongodb-example
    image: mongo:latest
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: john
      MONGO_INITDB_ROOT_PASSWORD: john
      MONGO_INITDB_DATABASE: admin 
  mongodb-expres-example:
    container_name: mongodb-expres-example
    image: mongo-express:latest
    depends_on:
      - mongodb-example
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: john
      ME_CONFIG_MONGODB_ADMINPASSWORD: john
      ME_CONFIG_MONGODB_SERVER: mongodb-example 
    networks:
      - network_example

networks:
  network_example:
    name: network_example
    driver: bridge
```
### RESTART
- restart value:
	- no: default
	- always
	- on-falure: if there is error
	- unless-stopped: always restart container, except when manually stopped
```yaml
services:
  mongodb-example:
    container_name: mongodb-example
    image: mongo:latest
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: john
      MONGO_INITDB_ROOT_PASSWORD: john
      MONGO_INITDB_DATABASE: admin 
  mongodb-expres-example:
    container_name: mongodb-expres-example
    image: mongo-express:latest
    depends_on:
      - mongodb-example
    restart: always
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: john
      ME_CONFIG_MONGODB_ADMINPASSWORD: john
      ME_CONFIG_MONGODB_SERVER: mongodb-example 
    networks:
      - network_example

networks:
  network_example:
    name: network_example
    driver: bridge
```

	- to see events ```docker events --filter 'container=mongodb-expres-example'```
### RESOURCE LIMIT
```yaml
services:
  nginx-port1:
    container_name: nginx-port1
    image: nginx:latest
    ports:
      - protocol: tcp
        published: 8080k
        target: 80
    deploy:
      resources:
        reservations:
          cpus: "0.25"
          memory: 50M
        limits:
          cpus: "0.25"
          memory: 50M
```
### BUILD (DOCKERFILE)
- If build attribute doesn't exist, the image will be downloaded from docker hub
- If build attribute exists, then the base image is built from what is inside the build object
Syntax:
```yaml
services:
  app: 
    container_name: app
    build:
      context: "./app"
      dockerfile: Dockerfile
    image: "app-golang:1.0.0"
    environment: 
      - "APP_PORT:8080"
    ports:
      - "8080:8080"
```
- For building using compose: we need to use ```docker compose build```
- Image that is built by compose wouldn't be deleted with docker compose down, so we should deleted manually with ```docker image rm imageName:tag```
- Building is not reload, so if there are changes, we should down the compose and rebuild to new image and recreate new container, because the previous container is using the previous image not the image with changes
### HEALTH CHECK
```yaml
services:
  app: 
    container_name: app
    build:
      context: "./app"
      dockerfile: Dockerfile
    image: "app-golang:1.0.0"
    environment: 
      - "APP_PORT:8080"
    ports:
      - "8080:8080"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080/health"]
      interval: 5s
      timeout: 5s
      retries: 3
      start_period: 5s

```
### EXTEND SERVICE LONG
- File 1 (docker-compose.yaml)
```yaml
services:
  app: 
    container_name: app
    build:
      context: "./app"
      dockerfile: Dockerfile
    image: "app-golang:1.0.0"
    environment: 
      - "APP_PORT=8080"
      - "MODE=local"
    ports:
      - "8080:8080"
```
- File 2 (prod.yaml)
```yaml
services:
  app: 
    container_name: app
    build:
      context: "./app"
      dockerfile: Dockerfile
    image: "app-golang:1.0.0"
    environment: 
      - "APP_PORT=8080"
      - "MODE=prod"
    ports:
      - "8080:8080"

```
- To use other compose file: ```docker compose -f prod.yaml create``` and ```docker compose -f prod.yaml start``` and ```docker compose -f prod.yaml down```
### EXTEND SERVICE SHORT
(other file)
```yaml
services:
  app: 
    environment: 
      - "MODE=prod"
```
(docker compose)
```yaml
services:
  app: 
    container_name: app
    build:
      context: "./app"
      dockerfile: Dockerfile
    image: "app-golang:1.0.0"
    environment: 
      - "APP_PORT=8080"
    ports:
      - "8080:8080"
```
How to run:
```docker compose -f docker-compose.yaml -f prod.yaml create```
```docker compose -f docker-compose.yaml -f prod.yaml start```
```docker compose -f docker-compose.yaml -f prod.yaml down```