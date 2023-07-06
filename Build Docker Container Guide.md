# Project Overview
- What will be containerized 
	- A static webpage
- Pros of containerizing?
	- Portability between different platforms and clouds
	- Efficiency through using far fewer resources than VMs and delivering higher utilization of compute resources
	- Agility that allows developers to integrate with their existing DevOps environment.
	- Higher speed in the delivery of enhancements. Containerizing monolithic applications using microservices helps development teams create functionality with its own life cycle and scaling policies.
	- Improved security by isolating applications from the host system and from each other.
	- Faster app start-up and easier scaling.
	- Flexibility to work on virtualized infrastructures or on bare metal servers
	- Easier management since install, upgrade, and rollback processes are built into the Kubernetes platform.
	- [IBM's take on container benefits and more details about containers](https://www.ibm.com/cloud/blog/the-benefits-of-containerization-and-what-it-means-for-you)
	
## Documentation for building container images: 
### Installed docker + dependencies (WSL2, for example)
- Download and install Terminal (On windows 10 machines) [Microsoft Terminal Download Webpage](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701)
- Download and install your preferred OS flavor e.g. Ubuntu, Debian, Kali [Download Windows Ubuntu](https://apps.microsoft.com/store/detail/ubuntu-on-windows/9NBLGGH4MSV6?hl=en-us&gl=US)
- Download and install Docker [Docker Install Download Link](https://www.docker.com/products/docker-desktop/)
### Build the container
- [Docker Build Documentation](https://docs.docker.com/engine/reference/commandline/build/)
- Ensure all of your dependencies and your Dockerfile are part of your current directory
- Use this method to build your container, swapping the "\[OPTIONS\]", PATH , URL appropriate to your solution
```
docker build [OPTIONS] PATH | URL | -
```
- [Docker Build OPTIONS Link](https://docs.docker.com/engine/reference/commandline/build/#options)
### Run the container
- To run a container use 
```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```
- [Docker Run OPTIONS Link](https://docs.docker.com/engine/reference/commandline/run/)
### View the project (open a browser...go to ip and port...)
- Once the container is running, get an instance of your choice web browser up
- Type your ip address and verify your webpage is running
### Troubleshooting 
- Ensure your sockets are given the correct permissions 
- Verify Docker is running 
- Validate your Docker is working, by running an included sample project : 
```
docker run hello-world
```
### Sources : 
- https://www.ibm.com/cloud/blog/the-benefits-of-containerization-and-what-it-means-for-you
- https://apps.microsoft.com/store/
- https://www.docker.com/products/docker-desktop/
- https://docs.docker.com/
- https://www.docker.com/wp-content/uploads/2022/03/docker-cheat-sheet.pdf