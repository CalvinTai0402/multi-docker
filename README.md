# multi-docker
My docker hub account: calvintjf


DOCKER COMMANDS

-docker run <image name>
-docker run -d <image name>																					*-d means detached (launch in background)
-docker run <image name> <command>																			*By specifying a command we will overwrite the default container's CMD
-docker create <image name>
-docker start -a <container id> 																			*-a means attached
-docker logs <container id>
-docker ps -a 																								*-a means list all containers
-docker system prune
-docker stop <container id>
-docker kill <container id>
-docker exec -it <container id> <command>																	*This line lets us execute another command inside a running container, -i directs keyboard input to stdin of command, -t formats the output from stdout to my screen
-docker exec -it <container id> sh																			*Launches shell inside the container
-docker build .																								*Must be inside the directory which contains Dockerfile (since is uses Dockerfile), builds the image
-docker build -t <docker id>/<project name>:<tag> .															*Must be inside the directory which contains Dockerfile, builds the image
-docker build -f <filename> .																				*Use another file that is NOT called Dockerfile
-docker run -p <localhost port>:<container port> <docker id>/<project name>:<tag>							*Port forwarding
-docker run -p <localhost port>:<container port> <image name>
-docker push <docker id>/<project name>:<tag>																*To push to Docker hub

DOCKERFILE

1.1	Specify a base image ---------------------------------------------------------------------> FROM <image name>:<tag>
1.2	Specify working directory to avoid conflicts---------------------------------------------->	WORKDIR <directory>
1.3	Copy files from local directory to container directory------------------------------------>	COPY ./ ./						*Note that this will copy from local to the working directory specified in 1.2
2. 	Run some commands to install additional programs------------------------------------------> RUN <command>
3. 	Specify a command to run on container startup---------------------------------------------> CMD ["...","...",...]
-Intermediate containers are created
-Cache will be used for rebuilds
-Tags can be used to specify diffeent versions. "alpine" tag means the most compact version of the image
-Files in the same directory are by default NOT included (due to containerization).
-We can use two COPY commands to avoid having rebuild the image from scratch everytime when a single change is made in one single file.


DOCKER-COMPOSE

-use .yml format
-services inside the .yml file have their own network, i.e., no need to do port mapping because a network is automatically created between the services by docker-compose version 3.
-restart policies: 'no' (must be in quotes), always, on-failure, unless-stopped. No need of quotes for the last three.
-docker-compose up -d																						*Similar to docker run <image>, -d means detached
-docker-compose up --build																					*Similar to docker build . then docker run <image>...remember to rebuild when a change is made
-docker-compose down																						*Stop and remove containers
-docker-compose ps																							*Must be in the same directory of docker-compose.yml


DOCKER VOLUME MAPPING
-docker run -it -p <localhost port>:<container port> -v <container dir> - v ${<dir referenced, usually pwd where Dockerfile exists>}:<dir which references> <image id>    *The first -v tells that container dir to not reference anything. ${pwd} in Windows (must run in powershell), must enable file sharing in docker desktop
-We can also create a docker-compose.yml file and simply run docker-compose up. This way we don't need to run it on powershell. Example docker-compose.yml file:
version: '3'
services:
    web:
        stdin_open: true
        build:
            context: .
            dockerfile: Dockerfile.dev
        ports:
            - "3000:3000"
        volumes: 
            - /app/node_modules
            - .:/app
NOTE THE "stdin_open: true" which currently solves a bug introduced by REACT
