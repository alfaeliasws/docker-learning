
1. To see docker image in the laptop:
   Docker Image: like installer
   ```docker image ls```

2. To download docker image latest
   ```docker image pull namaimage:tag```
   example:
   ```docker image pull redis:latest```

3. To delete docker image
    ```docker image rm namaimage:tag```
    example
    ```docker image rm alpine:tag```

4. Docker Container
   Docker Container: like the apps installed from the installer - but it can be installed multiple times, need to be run
   To see all Docker Container
   ```docker container ls -a```
   To see Docker Container that is running
   ```docker container ls```

5. Create Container
   ```docker container create --name namecontainer namaimage:tag```
   example
   ```docker container create --name redisexample redis:latest```

6. Run The Container
    ```docker container start containerId/namaContainer```
    * / = or
    example
    ```docker container start redisexample```

7. Stop the Container
    Container need to be stopped if you want to delete it
    ```docker container stop containerId/containerName```
    example
    ```docker container stop redisexample```

8. Delete the Container
    ```docker container rm containerId/containerName```
    example
    ```docker container rm redisexample```

9. To See Command Log in Docker
    Previous log (not realtime)
    ```docker container logs containerId/containerName```
    Realtime
    ```docker container logs -f containerId/containerName```

10. Get Into The Container
    ```docker container exec -i -t containerName /bin/bash```

11. Exit The Container
    ```exit```

12. Port Forwarding in Container
    set on creating container
    ```docker container create --name controllerName --publish posthost:portContainer image:tag```
    * posthost : port in our PC that we desire to give for the container to use
    * portContainer: the port we need to know which is the one that apps inside is defaultly using (example: redis-6793)
    example

13. Contaier Environment Variable
    set on creating container
    ```--env```
    example:
    ```docker container create --name mongoexample --publish 27017:27017 --env MONGO_INITDB_ROOT_USERNAME=root --env MONGO_INITDB_ROOT_PASSWORD=example mongo:latest```

14. Container Stats
    to see each RUNNING CONTAINER stats
    ```docker container stats```

15. Contaner Limit
    --memory (RAM - mb metric), --cpus (Processor - core metric float 1 decimal using dot)
    example
    ```docker container create --name smallmemnginx --publish 8081:80 --memory 100m  --cpus 0.3 nginx:latest```

16. Bind Mount
    use ```--mount```
    parameter for mount:
    1. Type: bind
    2. Source: source in our directory
    3. Destination: directory in the docker container
    4. Readonly: if the parameter is present, it will make the file not be able to be written in the container
    ```docker container create --name containerName --mount "type=bind,source=folder,destination=folder,readonly" image:tag```
    example
    ```docker container create --name containerName --mount "type=bind,source=E:\coding-learning\docker-sandbox\docker-basic/mongo-data,destination=/data/db,readonly" image:tag```

17. Create Volume
    Volume is where the container save the data
    ```docker volume create volumeName```
    example
    ```docker volume create mongovolume```

18. See Volume List
    ```docker volume ls```

19. Delete Volume
    make sure to stop container from running before we deleting volume, running container makes us not able to delete volume
    ```docker volume rm volumeName```
    example
    ```docker volume rm mongovolume```

20. Container Volume
    Use container volume that is created first then inject it to the container
    ```docker container create --name mongovolume --mount "type=volume,source=volume,destination=/data/db" --publish 27018:27017 --env MONGO_INITDB_ROOT_USERNAME=root --env MONGO_INITDB_ROOT_PASSWORD=example mongo:latest```

21. Backup Volume
    * Stop container that is wanted to be backed up
    ```docker container stop mongovolume```
    * Make container with 2 mount:
      * Volume that want to be backed up
      * Bind folder from the mount to the folder that we want to have
    ```docker container create --name backup --mount "type=bind,source=E:\coding-learning\docker-sandbox\docker-basic/backup,destination=/backup" --mount "type=volume,source=mongovolume,destination=/data/db" --publish 3444:80 nginx:latest```
    * Back up using container and archive the volume then save in the bind mount
    * Delete container
  
22. Backup Volume with --rm (automatically remove container)
    ```docker container run --rm --name ubuntubackup --mount "type=bind,source=E:\coding-learning\docker-sandbox\docker-basic/backup,destination=/backup" --mount "type=volume,source=mongovolume,destination=/data/db" ubuntu:latest tar cvf /backup/backup.tar.gz /data```

23. Restore Volume
    * create volume
    * run this command (backup)
    ```docker container run --rm --name ubunturestore --mount "type=bind,source=E:\coding-learning\docker-sandbox\docker-basic/backup,destination=/backup" --mount "type=volume,source=backupdata,destination=/data" ubuntu:latest bash -c "cd /data && tar xvf /backup/backup.tar.gz --strip 1"```
    * inject data to the container
    ```docker container create --name mongorestore --mount "type=volume,source=backupdata,destination=/data/db" --publish 27020:27017 --env MONGO_INITDB_ROOT_USERNAME=root --env MONGO_INITDB_ROOT_PASSWORD=example mongo:latest```

24. See Network List
    3 Network list:
    * Bridge
    * Host (Only for docker linux)
    * None 
    ```docker network ls```

25. Create Network
    ```docker network create --driver driverName networkName```
    example
    ```docker network create --driver bridge networkexample```

26. Remove Network
    ```docker network rm networkName```
    example
    ```docker network rm networkexample```

27. Create Container with Network
    ```docker container create --name containerName --network networkName image:tag```
    example:
    ```docker container create --network mongonetwork --name mongodb --env MONGO_INITDB_ROOT_USERNAME=root --env MONGO_INITDB_ROOT_PASSWORD=example mongo:latest```
    

28. Connect New Container
    ```docker container create --network mongonetwork --name mongodbexpress --publish 8081:8081 --env ME_CONFIG_MONGODB_URL="mongodb://root.example@mongo:27017/" mongo-express:latest```

29. Delete Container
    ```docker network disconnect networkName containerName```
    example
    ```docker network disconnect mongonetwork mongodb```

30. Connect Existing Container
    ```docker network connect networkName containerName```
    example
    ```docker network connect mongonetwork mongodb```

31. Inspect
    To see the details
    Image:
    ```docker image inspect imageName```
    Container:
    ```docker container inspect containerName```
    Volume:
    ```docker container inspect volumeName```
    Network:
    ```docker network inspect networkName```

32. Prune
    To delete many of the type of docker that is not in use
    Image:
    ```docker image prune```
    Container:
    ```docker container prune```
    Volume:
    ```docker container prune```
    Network:
    ```docker network prune```
    All things:
    ```docker system prune```
