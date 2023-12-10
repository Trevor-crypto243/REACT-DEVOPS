All about docker in a nutshell
https://www.youtube.com/watch?v=G07FcRhYB2c&list=PL4cUxeGkcC9hxjeEtdHFNYMtCpjNBm3h7&index=5

predictable, consistent, isolated environment
Tool for managing containers
Deployment of containers to production servers
Share host operating system and kernel
Lightweight than virtual machines
Use less memory

Running containers in isolated environments
    -node app environment
    -react app environment
    -mongodb environment

Specific environment set up
Specific environment requirements
Versions of node, python, php
Running software in isolation 

____________Installing Docker
-There is no docker desktop for linux


___________Images and containers
    images
        -blueprints for containers
        -They contain
            1. Runtime environment e.g specific node version
            2. Application code itself
            3. Any dependencies needed to run
            4. Extra configuration(e.g environment variables)
            5. Commands (additional instructions)
            6. Their own file system independent from the computer
        -Images are read only, cannot be altered once created
        -Include every single thing a  container needs to run
        -we share images to enable people run them to generate containers on their devices
    
    Containers
        -Runnable instances of an image
        -When we run an image a container is created
        -They are isolated processes
        -They run independently of the processes in your 
            1. Stop
            2. Restart
            3. Delete
            4. Cli - interact with it, linux terminal, interact with the runtime

______Creating images and running containers based on those images
-Images are made up of several layers
    1. Parent layer
        -it is the first layer
        -describes the operating system and the runtime (node, python, php, java)environment of the container we want
        -docker hub
            -online repository for docker images
            -pre made images 
            -docker pull "image_name"
        -alpine-lightweight linux distro
        -specify the runtime version tag(e.g node) to avoid pulling the latest tag
    2. Additional layers
        -We use Dockerfile to do this
        -Images we make ourselves
        -Copying our source code
        -Installing dependencies
        -Changes to the image we are making
        -Installing commands to the image
        -Instructions to docker to create image


RUN is used for installing dependencies, setting up the image
Used for making the image

CMD is used for running the container
Used for runtime
When the container begins to run

Expose tells docker which port to expose
Not really needed when we are running docker from the commandline

docker build -t myapp .
. - relative path to Dockerfile
//myapp to name the additional image we just created
//Buids that image


_________.dockerignore file
-files ignored when copying files to custom image

_________starting and stopping containers
-giving containers names
-giving them ports
-port mapping

1. List all images you have(name, tag,id,       uptime)
    docker images

2.Run image
    use the name/repository
    docker run --name myapp_c1 myapp
    the name tag gives a name to give a name to youer container

3. Port mapping
    -specifying  a port on our computer that can be mapped to the port exposed by our docker container 
    docker run --name myapp_c2 -p 4000:4000 -d myapp
    -p //publish/map container ports
    port on right - docker
    port on left - port on computer
    -d detached flag, detach terminal from process
    -myapp_2 container name
    // --rm removes/deletes a container once it stops running

4. Listing docker processes/containers 
    docker ps //only lists currently running containers

    docker ps -a
    -lists all containers even those that are not running

5. Stopping a container
    docker stop "container_id"
    docker stop "container_name"

6. Starting an existing container
    docker start my_app2 
    we dont need to reconfigure the port mapping
    docker start runs in detached mode
    docker start -i my_app3 
7. Delete image
    docker image rm myapp
    -deletes image that has no container dependency

    docker image rm myapp -f
    -deletes even if used by a container

8. Delete container
    docker container rm myapp_2

9. docker system prune -a
    removes all containers, images, volumes



_______________Layer Caching
-Each line adds a new layer to a Dockerfile/image
-stacking layers on top of each other
-Everytime we make a new image docker has to go through all these layer
-This includes everytime we make chnages to our code
-Docker caches the image layers
-Shows layers grabbed from cache during build on the terminal
-next layers after the layer that chnaged have to be created from scratch
-ordering of layers to avoid rebuilding and appropriate use of cache
-Avoiding reinstallation of runtime(node, pip, php, java)


___________Managing Images and Containers
Versioning
    -tag names
    - docker build -t myapp:v1 .
        -builds image with that tag name :v1
        -tags can be letters, numbers


____________Volumes
-Hel us specify folders on our ccomputer that can be made available to running containers
-We can map those folders inside specific folders in our running containers
-If something changes in those folders in our computer those chnages would also be reflected on the folders mapped to on the container
-We see a change in the container without having to build a docker image
-Make changes to a project and preview those chnages without having to build new Images
-The images however do not change, volumes are usefull for testing and suchhe container is gonna be kept in sync with the project path
-T

//adiing the volume flag
docker run --name myapp_c2 -p 4000:4000 -d -rm -v  "absolute project path":"folder we wanna map in the container itself" myapp
docker run --name myapp_c2 -p 4000:4000 -d -rm -v  /software/docker/netninja/api:/app myapp

Creating  amore specific volume to accomodate folders or files that you do not want to change in the docker image
-Will be saved in a virtual location by docker
    docker run 
    --name myapp_c2                             //container name
    -p 4000:4000 -d -rm                         //port mapping
    -v  /software/docker/netninja/api:/app      //volume
    -v /app/node_modules                        //folder to be discreted
    myapp                                       //image name


_____________Docker Compose
-Easy way to manage containers to avoid complexity and complications
-docker0compose is a tool that comes with docker 
-contains all the container configurations of our project
-We can set up multiple containers here

        version: "3.8"  # version of docker compose we wanna use
        services:
        api:
            build: ./api  #root directory containing the Dockerfile to build the container
            container_name:  api_c #the name to give our container
            ports:  #do port mapping
            - '4000:4000'
            volumes:
            - ./api:/app
            - ./app/node_modules

docker-compose up  //start the services
docker-compose down //stop container
docker-compose down --rmi all -v //remove all images created by dockercompose and volumes


_____________Dockerizing a react app
Dockerfile   -to specify how the image should be built
.dockerignore - ignore node modules qwhen building the image
docker-compose file


____________Sharing images on docker hub
-pushing and pulling images to and from docker hub
-Sharing images with people
            






