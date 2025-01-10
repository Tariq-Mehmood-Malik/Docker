# Docker Networking Overview

The beauty of docker container is isolation not only on process / services level also on network level as well. Docker allows communication between containers and with external networks. It utilizes software-defined networking (SDN) to create virtual networks, providing connectivity while maintaining isolation. This allows users to define custom networks, connect containers to these networks, and establish network policies.         
 
Basic network /driver types in docker:

### 1. Bridge: 
The default network type. It creates a virtual bridge on the host machine, connecting containers to the bridge. Containers on a bridge network can communicate with each other and, by default, access the host's network.        

### 2. Host: 
Containers on the host network share the host's network namespace. They have the same IP address as the host and can directly access the host's network interfaces.    

### 3. None:
This network type disables the container's network stack, isolating it from any network access.   

### 4. Overlay: 
This network type allows communication between containers running on different Docker hosts. It creates a virtual network that spans multiple hosts, enabling inter-host container communication.  

### Docker connect 
It is a command used to add an existing Docker container to a specific network (on same host). By connecting a container to a an other network, it gains the ability to communicate with other containers within that network.

---
Some important concepts:

**Veth (Virtual Ethernet) Pair:**             
  A veth (Virtual Ethernet) pair is a set of two virtual network interfaces. One end resides inside a container's network namespace. The other end is typically connected to the host’s network namespace or a bridge (docker0).
  
**Linux Namespaces:**   
  A namespace in Linux is a feature that isolates system resources such as processes, network interfaces, file systems, and more, allowing them to operate independently within separate environments. Each namespace provides an isolated view of a specific resource, ensuring that processes in different namespaces do not interfere with each other. This isolation is crucial for containerization technologies like Docker, where each containers can have their own independent resources (e.g., network, filesystem, process IDs) while running on the same host system.   
  
**NAT (Network Address Translation):**    
  NAT is used to allow devices to access external networks (like the internet). When a container tries to access an external service (such as a website), the host will perform source NAT (SNAT) to translate the container’s private IP address to the host’s public or routable IP address. The reverse process, called DNAT (Destination NAT), happens when the response returns to the host and is forwarded to the container
  
**Bridge network:**    
  A bridge network in Linux connects multiple network interfaces (like Ethernet or virtual interfaces) to allow them to communicate as if they were on the same physical network. It operates at the data link layer (Layer 2) and is commonly used in virtualization to connect virtual machines to the host network. A bridge works like a software switch, forwarding packets based on MAC addresses. In docker `docker0` is default bridge network for containers.

---
# Docker Networking Expalined

## Host Driver in Docker

![dh01](images/dh01.webp)


The Host network driver in Docker is a networking mode that allows containers to directly use the networking stack of the Docker host machine, bypassing Docker’s network abstraction layer. In this mode, the container shares the same network namespace as the host, utilizing the host’s IP address and network configuration. This means the container does not get its own IP address and can directly access the host’s network interfaces, routing table, and ports. For example, if a container binds to port 80 and uses host networking, its application will be available on port 80 on the host's IP address.

Using the host network mode offers several advantages. It provides better performance due to reduced overhead and faster networking, simplifies configuration by eliminating the need for port mapping and enabling a unified network setup, and allows containers to directly access host resources like storage and hardware. Additionally, it avoids the complexity of virtual network overhead. However, this approach also has notable downsides. There are security risks due to the lack of isolation between containers and the host, which can lead to potential vulnerabilities. Containers directly sharing the host's network can result in port conflicts, especially when multiple containers try to bind to the same port. Furthermore, the setup has limited scalability, making it harder to scale across multiple hosts. Resource contention may arise as containers compete with the host for system resources. Finally, because it is tied to the host’s environment, this mode reduces portability and makes it harder to move containers between different systems.



<br>   

## Bridge Driver in Docker   

Docker's Bridge driver uses several key concepts to manage container networking and enable communication both within the container environment and with external networks. The main components involved are `Veth pairs`, `Linux namespaces`, `NAT`, and the `Bridge network`.

![db01](images/db01.png)

# Docker Networking Overview

One end of the **veth pair** resides within the container's network namespace and acts as the container’s primary network interface (e.g., `eth0`). The other end is connected to the Docker bridge network (`docker0`), which allows the container to communicate with other containers and the host. The veth pair serves as a virtual cable connecting the isolated container network to the host’s network stack.

Each container has its own **network namespace**, which includes its own IP address, routing table, and network interfaces. This isolation ensures that containers do not interfere with each other’s network configurations, maintaining a secure and independent networking environment. The container’s network namespace is connected to the host's network via the Docker bridge (`docker0`) and its veth pair.

When a container sends outbound traffic (e.g., to access the internet), Docker performs **SNAT**. This translates the container’s private IP to the host’s public IP. **DNAT** is performed when a response comes back to the host, mapping the response to the appropriate container by converting the public IP back to the container’s private IP address.

This ensures that the container can access external services while remaining secure behind the host's IP address.

The **bridge network** in Docker connects containers to the host’s network, allowing them to communicate both with each other and with the outside world. The bridge operates at the **data link layer** (Layer 2) and Layer 3, forwarding packets based on both **MAC addresses** and **IP addresses**. Containers on the same bridge network can communicate directly with each other by sending packets to the bridge. The bridge also connects to the host’s network interface, enabling containers to access external resources via NAT. 

# Docker Networking Overview

One end of the **veth pair** resides within the container's network namespace and acts as the container’s primary network interface (e.g., `eth0`). The other end is connected to the Docker bridge network (`docker0`), which allows the container to communicate with other containers and the host. The veth pair serves as a virtual cable connecting the isolated container network to the host’s network stack.

Each container has its own **network namespace**, which includes its own IP address, routing table, and network interfaces. This isolation ensures that containers do not interfere with each other’s network configurations, maintaining a secure and independent networking environment. The container’s network namespace is connected to the host's network via the Docker bridge (`docker0`) and its veth pair.

When a container sends outbound traffic (e.g., to access the internet), Docker performs **SNAT**. This translates the container’s private IP to the host’s public IP. **DNAT** is performed when a response comes back to the host, mapping the response to the appropriate container by converting the public IP back to the container’s private IP address.

This ensures that the container can access external services while remaining secure behind the host's IP address.

The **bridge network** in Docker connects containers to the host’s network, allowing them to communicate both with each other and with the outside world. The bridge operates at the Layer 2 and Layer 3, forwarding packets based on both **MAC addresses** and **IP addresses**, depending on the specific communication (between containers or the host). Containers on the same bridge network can communicate directly with each other by sending packets to the bridge. The bridge also connects to the host’s network interface, enabling containers to access external resources via NAT. 

---

## Docker Network Commands

To list all docker networks
```bash
docker network ls
```
<br>

To create new network of driver bridge
```bash
docker network create --driver bridge <netwrok-name>
```
<br>

To inspect network
```bash
docker network inspect <network-name>
```
<br>

To create conatiner in custom network
```bash
docker run -d --net <network-name> nginx
```
<br>

To connect container running in 1 network to an other network also
```bash
docker network connect <new-network-name> <container-name>
```
<br>
