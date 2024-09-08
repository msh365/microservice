https://hub.docker.com/repositories
===================================DOCKER=========================================

https://github.com/in28minutes/spring-microservices-v2/tree/main/04.docker

*************Docker Architecture*********

"Docker Client" is where we run command 
"Docker Daemon" is responsible for executing the task, mainly below three task
managing containers, local images and  image registry (creating new images and push it to the hub)


**************Docker Commands**************

most of the "docker container" command can be shotend to only "docker"

docker run  -p 8080:5000 in28min/todo-rest-api-h2:1.0.0.RELEASE       * downoads the image from docker repo hub if not found in local system
                                                                      * runs the image 
                                                                      * -p 8080:5000 maps inetrnal port 5000 to the host machine port 8080
 
http://localhost:8080/hello-world-bean     * access the application on the mapped port 8080 once container is running

docker run  -p 8080:5000 -d in28min/todo-rest-api-h2:1.0.0.RELEASE     * to run image in background( detached-mode)

docker run --restart=always in28min/todo-rest-api-h2:1.0.0.RELEASE     * --restart=always will ensure the container gets on running state after the restart of docker engine  

docker run -m 512m --cpu-quota 50000 in28min/todo-rest-api-h2:1.0.0.RELEASE  * -m 512m assigns memory for this container 
                                                                             * --cpu-quota 50000 assigns cpu quota to the process . total avaialble cpu quota is 100000

docker logs -f [containerId]  * to see and follow the logs 

docker container ls                   *shows all running container 

docker container ls -a                *shows all container irrespective of thier status

docker container stop [containerId]   *to stop a running container gracefully 

docker container kill [containerId]   *to imediately shutdown container

docker container pause [containerID]    * to pause an container 

docker container unpause [containerID]  * to unpause an container

docker container inspect  [containerID] * to see the complete details of the container

docker container prune                  *to delete all stopped containers

docker events                           *to see events executed by Docker daemon 

docker top [containerId]                *shows the process running inside the container

docker stats                            *to see statistics of all runing containers

docker system df                        *to see the details of things managed by docker daemon  

docker images (or) docker image ls    *lists all local images 

docker tag [image repository]:[old tag] [image repository]:[new tag]   *to give a new tag to a image
docker tag in28min/todo-rest-api-h2:1.0.0.RELEASE in28min/todo-rest-api-h2:latest
 
docker pull [image name]          *to pull a repository from docker registry to the local system
 
docker image history  [imageId]   *to see the history of steps in creating the image

docker image inspect  [imageId]   *to see the full details of the image 

docker search [image name]        *to search an image 
                                  *an official image is managed by docker team 
								  
docker image remove [imageId]     *to remove image from local


==================to create docker image===========================

go to pom add below code in build tag as per requirement. mention image name in image tag as your docker hub id / repo name: tag
spring-boot-maven-plugin will help in building docker image

<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<configuration>
					<image>
						<name>msh365/ms-${project.artifactId}:${project.version}</name>
					</image>
					<pullPolicy>IF_NOT_PRESENT</pullPolicy>
				</configuration>				
			</plugin>
		</plugins>
	</build>
	

click on project -> run as maven -> build -> Goals -> spring-boot:build-image -DskipTests -> run


docker run -p 8000:8000 msh365/ms-currency-exchange-microservice:0.0.1-SNAPSHOT

http://localhost:8000/currency-exchange/from/USD/to/INR




===========================Automate docker image run by docker-compose ===========================

create a docker-compose.yaml file and configure the image to run like below 

---------------------
version: '3.7'

services:
  currency-exchange:
    image: msh365/ms-currency-exchange-microservice:0.0.1-SNAPSHOT
    mem_limit: 700m
    ports:
      - "8000:8000"
----------------------	  
	  
Now cd to the directory containing this yaml file from shell and run below command to start execution

docker-compose up      *this will look for a docker-compose.yaml file in the current directory and runs the image mentioned in the file 

by default the application will run in default network created by docker-compose. to create a network add below code in yaml file

-------------------
networks:
  currency-network:
------------------- 

to use this network, add below line in the service defination 

-------------------------
    networks:
      - currency-network
-------------------------


docker application cannot communicate through localhost, pass the URL of the application as an environment variable. 
It will override the property of application.property file of that application.

    environment:
      eureka.client.serviceUrl.defaultZone: http://naming-server:8761/eureka

if an application has dependencuy on any other application then add below line in the service defination 

    depends_on:
      - naming-server
      - rabbitmq

-----------------------------------------------

docker run -p 9411:9411 -d openzipkin/zipkin:2.23

