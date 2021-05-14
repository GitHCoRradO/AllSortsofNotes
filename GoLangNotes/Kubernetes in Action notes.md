### Kubernetes in Action notes

#### Chapter 2, building the container image
+ when building the container image from a Dockerfile, Docker client uploads all the contents of the directory containing the Dockerfile to Docker daemon; and the image is built in the Docker daemon; the client and daemon can be on different machines.
+ tip: Don’t include any unnecessary files in the build directory, because they’ll slow down the build process—especially when the Docker daemon is on a remote machine.