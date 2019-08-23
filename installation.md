Getting Set Up <br>
=== 

### What Is Docker <br> 

Docker is a way to run and deploy containers running software on a virtualized operating system without also virtualizing the hardware. 
As a result, Docker containers are more lightweight and use less resources
than traditional Virtual Machines. 
They can be run on any operating system. <br>

Here is a much better description: <br> 

[Explanation Of Docker](https://www.docker.com/resources/what-container)

### Docker Setup <br>

#### Windows <br>

Windows has issues that other *nix* operating systems do not have. 
In this case there are issues with virtualization. 
Hyper-V must be enabled in order to work, but this causes a loss of functionality with virtual machines managed by Virtualbox, VMware, and the like. 
If this affects you, you may use Docker toolbox to circumvent this.
Otherwise, Docker Desktop is the best way to go. <br>

[Windows Docker Installation Instructions](https://docs.docker.com/docker-for-windows/install/)

#### Mac <br> 

With OSx there are not the same virtualization issues that are in Windows, so you can install Docker Desktop like normal. <br>

[Mac Docker Installation Instructions](https://hub.docker.com/editions/community/docker-ce-desktop-mac)

#### Linux <br> 

Linux also does not face the same virtualization issues as Windows, but Docker Desktop is not available for it. 
You will just need to use the regular Docker server. 
Scroll about 3/4 through and choose your distro. 
It will then give you step by step instructions. 

[Linux Docker Installation Instructions](https://docs.docker.com/install/)

### But I Want A GUI! <br> 

There are many GUI options available for Docker. 
I will not go in depth, but will list a few here: <br>

[Kinematic](https://github.com/docker/kitematic) (Linux, Mac, and Windows)

[Portainer](https://www.portainer.io) (Linux, Mac, and Windows)

[Docker Desktop](https://www.docker.com/products/docker-desktop) (Mac and Windows)

[Rancher](https://rancher.com/products/rancher) (Linux, Mac, and Windows)
> Note Rancher is made more specifically for Kubernetes, a Docker container deployment, scaling, and management system.
> Kubernetes is much more complex than vanilla Docker.
> Rancher is included in this list due to how simple it makes spinning up containers. 
