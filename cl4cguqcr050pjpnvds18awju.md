## A Quick Overview of Docker


Docker is a tool to create, run and deploy the application by using light-weighted containers. These containers help the applications to work efficiently in different environments. By using Docker, developers can quickly build, pack, ship, and run applications as lightweight, portable, self-sufficient containers and run virtually anywhere. It helps developers to focus on the application code without worrying about underlying OS or deployment systems


### How it is different from a Virtual Machine?

Docker is often compared to Virtual Machine but they are not the same. Let's take a look at the below image(credits:KodeKloud.com) to see the differences.

![Screenshot 2022-06-13 at 12.58.47 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1655105357539/HNaoBGjuW.png align="left")

Unlike virtual machines, Docker utilises the host OS instead of creating a new Guest OS. Since a Docker Container does not have a boot operating system, it starts up instantly and consumes less memory than VM.

### Important Terminologies in Docker

**Image: **

- It is defined inside .Dockerfile in the root directory.
- It is a read-only file with a set of instructions and commands to run a docker container.
- Each instruction will be created as a layer and each layer will be cached during docker image building. If any issues occur, we can rebuild the image after rectifying them. Docker will rebuild the modified layer and use the cache for other layers. 
	
** Containers:**

- It is a running instance of a docker image. From one image you can create multiple containers (all running the sample application) on multiple Docker platform
- Containers are isolated from one another and bundle their own software, libraries and configuration files; they can communicate with each other through well-defined channels. 
- All containers are run by a single operating system kernel and are thus more lightweight than virtual machines.

** Container Registry:**

- Docker Container Registry is where we put Docker Image remotely before deploying it to the server. In another way, it is a standard way to store and distribute Docker images.

**Volume:**	

- When the container is deleted, It will also remove the data associated with that container. If you want to retain that data we can store it outside the docker as volume and it can be mounted again to the new container whenever it is required. 

### Basic commands

```
#To pull the image from registry
   docker pull <image-name>

#To start the container. images are a set of instructions to run the container.
  docker run <image-name>

#To run a specific version of the container. 
  docker run <imagename>:tagname

#To list all containers including the killed container.
  docker ps -a	

#To see the logs
  docker logs <container-name>

#To kill all running containers. -q flag list only container ids
  docker kill $(docker ps -q)	

#To remove all docker containers from docker.
  docker rm $(docker ps -a -q)

# To remove all Docker images
  docker rm $(docker images -q)

#To build a docker using the docker-compose file
  docker-compose up --build	

#To run docker in detach (background) mode which enables terminal to run other commands
  docker run -d <image-name>

#To run a container in interactive(enables the terminal to print and receive user input) mode. 
  docker run -it <image-name>	

#Port mapping each container has to be mapped to the Docker host port to access the application through docker.
   docker run -p <docker_port>:<container port>  <image-name>	

#To mount the docker volumes
	docker run -v <external volume dir>:<docker dir> <image-name>	


``` 

**Notes**

- Containers go to sleep if there is no running process in images. For Example, Ubuntu is a container for other images to execute. It will not have any running process in it. So it will go to sleep as soon as it is started. To avoid that you can pass sleep <sec> argument.      
```
docker run ubuntu sleep 10
```
- By default docker will attach the latest tag when you run the docker run command. It will pull the latest image from the registry. you can add a tag to build using the -t flag when building.  For Example, docker build . -t tagname <imagename>. if you want to pull tagname , you can use  .
```
docker run <imagename>:tagname
```
    


**References**
Debugging - https://www.docker.com/blog/live-debugging-docker/
 

