### Kubernetes in Action notes

#### Chapter 2, building the container image
+ when building the container image from a Dockerfile, Docker client uploads all the contents of the directory containing the Dockerfile to Docker daemon; and the image is built in the Docker daemon; the client and daemon can be on different machines.
+ tip: Don’t include any unnecessary files in the build directory, because they’ll slow down the build process—especially when the Docker daemon is on a remote machine.

#### Chapter 2, commands to run and check out images
+ list all locally stored images:
	``` docker images ```
+ build kubia image. kubia is the image name built with.
	```docker build -t kubia .```
+ to run a new container called kubia-container from the kubia image.
	+ The container will be detached from the console (-d flag), which means it will run in the background.
	+ Port 8081 on the local machine will be mapped to port 8080 inside the container (-p 8081:8080 option)
		``` docker run --name kubia-container -p 8081:8080 -d kubia```
+ list all running containers
	``` docker ps```
	``` output sample
		CONTAINER ID   IMAGE         COMMAND         CREATED          STATUS         PORTS                                       NAMES
		8cbb6aa46a5a   luksa/kubia   "node app.js"   17 seconds ago   Up 6 seconds   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   recursing_meninsky
	```
+ print out a long JSON containing low-level information about the container
	``` docker inspect kubia-container ```
+ stop a container. This will stop the main process running in the container and consequently stop the container, because no other processes are running inside the container.
	``` docker stop kubia-container ```
+ the stopped container itself still exists and you can see it with ```docker ps -a```. The ```-a``` option prints out all the containers, those running and those that have been stopped.
+ to truly remove a container
	``` docker rm kubia-container ```

#### registry url for China mainland  
+ ``` minukube start --image-mirror-country='cn' --image-repository='registry.cn-hangzhou.aliyuncs.com/google_containers'```

#### Chapter 3, understanding containers and pods
+ Containers are designed to run only a single process per container (unless the process itself spawns child processes).
+ A pod of containers allows you to run closely related processes together and provide them with (almost) the same environment as if they were all running in a single container, while keeping them somewhat isolated.