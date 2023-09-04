# [Docker: An Introduction](https://www.youtube.com/watch?v=pg19Z8LL06w&ab_channel=TechWorldwithNana)

- [Docker: An Introduction](#docker-an-introduction)
  - [Key Linux Technologies Powering Docker](#key-linux-technologies-powering-docker)
  - [Comparison between VMs and Docker Containers](#comparison-between-vms-and-docker-containers)
    - [References](#references)

Docker is an open-source platform that automates the deployment, scaling, and management of applications using containerization.

`A Docker container is a lightweight, standalone, executable package` that includes everything needed to run a piece of software, including the code, runtime, libraries, environment variables, and config files.

Containers are designed to be portable and efficient, running on any system that supports Docker and providing consistent operation across different environments.

Containers are far more lightweight than traditional VMs, have near-instant spin up times, and can be easily scaled up or down based on demand.

## Key Linux Technologies Powering Docker

Docker leverages several key Linux technologies to provide its powerful, flexible containerization capabilities. These technologies include:

**1. Control Groups (cgroups):** This Linux kernel feature limits, accounts for, and isolates the resource usage (CPU, memory, disk I/O, network, etc.) of process groups. Cgroups ensure that each Docker container only uses its allotted resources.

**2. Namespaces:** Namespaces provide a layer of isolation between containers. Each container runs in its own namespace, separate from other containers. This allows each container to operate as if it's the only process running on the host system.

**3. Chroot:** Chroot helps create an isolated filesystem for each container. Each container has its own filesystem that doesn't interfere with others, contributing to the security and isolation Docker provides.

**4. Union File Systems (UnionFS):** UnionFS allows Docker to build and layer containers, sharing common files between them to save space and increase container spin-up speed. This layering also enables Docker's image versioning and distribution.

**5. Security Modules (SELinux, AppArmor):** Docker can leverage Linux security modules to provide an additional layer of security to its containers. These modules help define access controls and ensure processes inside a container cannot interfere with the host system.

In conclusion, Docker has leveraged a variety of existing Linux technologies to revolutionize how we develop, deploy, and scale applications. These technologies have come together to enable Docker's lightweight, isolated containers that can be consistently run on any system with Docker support.

## Comparison between VMs and Docker Containers

| Attribute  | Virtual Machines (VMs) | Docker Containers |
|---|---|---|
| **System Overhead**  | VMs require a full copy of an operating system, along with the application and its dependencies. This leads to significant system overhead. | Containers share the host system's kernel and only require the application and its dependencies, leading to less system overhead.  |
| **Boot-Up Time**  | VMs generally have slower boot-up times because they need to start up a full operating system.  | Containers can start almost instantly, as they only need to start the application processes.  |
| **Performance**  | VM performance can be near-native, but it's generally less efficient due to the overhead of running multiple full operating systems. | Containers have less overhead and can therefore offer better performance, especially in terms of startup time, scalability, and density. |
| **Isolation**  | VMs provide strong isolation as each VM operates on separate underlying OS instances.  | Containers also provide isolation, but it's a little less rigid than VMs, as they share the host kernel. |
| **Portability**  | VMs are less portable due to host-guest OS dependencies. | Containers are highly portable; a containerized application can run on any system that has Docker installed, irrespective of the underlying host OS. |
| **Security**  | VMs provide robust security due to the strong isolation of resources. They are less susceptible to kernel vulnerabilities. | Containers are generally secure, but they share the host kernel and therefore are more susceptible to kernel-level exploits. Docker does have security features like SELinux and AppArmor to mitigate these risks. |
| **Disk Space**  | VMs require more disk space, as they need to store the entire operating system along with the application. | Containers require less disk space, as they only store the application and its dependencies. |
| **Use Case**  | VMs are good for running applications that require all of the operating system's resources and functionality. | Containers are great for deploying microservices, as you can easily scale up or down the individual components of an application. |

### References

- [Docker Explained](https://www.youtube.com/watch?v=pg19Z8LL06w&ab_channel=TechWorldwithNana)
- [Docker in 100s](https://www.youtube.com/watch?v=Gjnup-PuquQ&ab_channel=Fireship)
