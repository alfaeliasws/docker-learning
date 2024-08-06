1. Docker Build
   *  -t for tag
    example
    ```docker build -t alfa:1.00 folder-dockerfile```
    example multiple
    ```docker build -t alfa:1.0.0 -t alfa:latest folder-dockerfile```

2. Docker Syntax
   1. (#) Hashtag : Comment
   2. ALL CAPITAL: Instruction
   3. arguments: lower case is argument
   
3. From Syntax
   ```FROM alpine:3```

4. Run Syntax
   Running on build command to produce image
   ```RUN ```
   RUN format
   1. RUN command
   2. RUN ["executable", "argument", ...]

5. Display Output
   * To display out put use this argument
    ```--progress=plain```
   * If there are no changes, it will run from the cache
   * If we want to run with no cache
    ```--no-cache```
    example:
    ```docker build -t alfa/simple-output-text run --progress=plain --no-cache```

6. Command Syntax
   ```CMD ```
   * Running when container is running
   * Docker will only execute the last command
   * Syntax
     1. CMD command param param
     2. CMD ["executable", "param", "param"]
     3. CMD ["param", "param"] will use executable ENTRY POINT

7.To See The Content of The file:
    1. Craete and start the container
    ```docker container logs command```

8. Label Syntax
   * To add metadata to the project
   * Will not be executed anyway
   * Syntax:
     1. LABEL <key>=<value>
     2. LABEL <key1>=<value1> <key2>=<value2> 
   * Will make us able to add labels in docker image inspect

9. Add Syntax
   * To add folder from the external source or url
   * Will not add the compressed file to the image but instead extracting it so what is inside is the extracted version
   * This command can add multiple file in one go
   * Using Pattern of golang 
   * Syntax:
     1. ADD source destination
   * Using Golang pattern

10. Copy Syntax
    * Copy instruct to just copy the file
    * It doesn't support url download
    * Copy won't extract anything
    * Best practice is to use COPY then add
    * If you want to uncompress, use RUN
    * Syntax is similar to add

11. dockerIgnore
    * Similar to gitignore

12. Expose Syntax
    * format untuk expose port
    * Syntax
      * EXPOSE port/tcp
      * EXPOSE port
    * Default of EXPORT is tcp
  
12. Environment Variable Syntax
    * To change environment variable
    * Env can be used with this in the Dockerfile file: ${ENV_NAME}
    * It can be overwritten when using --env parameter in docker cotnainer create
    * Syntax
      * ENV key=value
      * ENV key1=value1 key2=value2

13. Volume Syntax
   * To create volume automatically when we create docker container
   * All file in the volume will get copied automatically to Docker Volume, even when we don't make volume when we create the docker container
   * It is good when we are storing data in file format
   * Syntax
       * VOLUME /location/folder
       * VOLUME /location/folder1 /location/folder2
       * VOLUME ["location/folder1","location/folder2"]
   * To see the volume use inspect the container
   ```docker container inspect volume```

14. Working Directory Syntax
   * To make the docker knows about the home folder where the command should be run
   * If there are not directory of the file, it will be created by docker
   * Working directory will get appended if there are many working directory
   * Working directory is the one that we got into when we use docker container exec 
   * Syntax
     * WORKDIR /app means the working directory will be at /app
     * WORKDIR docker  (after WORKDIR /app) means the working directory will be at /app/docker
     * WORKDIR /home/app means the working directory will be /home/app

15. User Syntax
    * To change default user to be not root (default is root)
    * Syntax
      * User <user>
      * User <user>:<group> (with group)
    * Make sure that the user is present in the linux system of the docker image

16. Argument Syntax
   * To define variable when building the docker image using this command
   ```--build-arg key=value```
   * Will only be run on the build command, not working in container (it is env that can be used inside the container)
   * To access the arg in the Dockerfile is the same like using env using ${ARG}
   * Example of building
   ```docker build -t alfa/arg arg --build-arg app=alfa```
   * In the end ARG should be passed to ENV too, because ARG is run on the build time while CMD will be run at the container run time

17. Health Check Syntax
   * To check if docker container is running well or not
   * If there is HEALTHCHECK syntax there will be 3 status
     * Starting: when starting
     * Healthy: if healthy after starting
     * Unhealthy: if unhealthy after starting
   * Syntax
     * HEALTHCHECK NONE (disable healthcheck)
     * HEALTHCHECK [OPTIONS] CMD
       * OPTIONS:
         * --interval=DURATION (default 30s)
         * --timeout=DURATION (default 30s)
         * --start-period=DURATION (default 0s)
         * --retries=N (default 3)

18. Entrypoint Syntax
   * To choose the executeable file that will be executed by container
   * Entrypoint is entangled with CMD
   * When we use CMD without executable file, we will use ENTRYPOINT
   * Syntax
     * ENTRYPOINT ["executable","param1", "param2"]
     * ENTRYPOINT executable param1 param2
     * CMD["param1", "param2"] (it will use executable)
   * ENTRYPOINT to be simple is the command that we use to run somthing

19. Multibuild Syntax
    *  Use from image that is not too big, we should not use the image base that is big
    *  The Solution is using multibuild
    *  It is the steps of the build, so we could make the image size smaller through compiling/etc first
    *  If we use multiple FROM syntax, it is recognized as the next step of the build
    *  It can be use for compiling so the image is lighter

20. Docker Push
   * Pushing to dockerhub:
     * Go to account
     * Go to Account Settings
     * Set Access Token
     * Login with ```docker login -u username```
     * Paste the password from set access token
     * Make sure the image that you want to push have the same prefix as your username (alfaeliasws/yourImage)
     * Then do ```docker push username/yourImage```