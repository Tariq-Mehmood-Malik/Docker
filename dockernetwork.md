# Docker Network / Networking
---

The beauty of docker container is isolation not only on process / services level also on network level as well. By default docker creates `veth` for each container (virtual ethernet interface) that is seprate from host which is mostly `172.17.0.0/16` all containers are created in this network unless defined by Docker Client. All containers in veth netwotk can communicate with each others and also with host system as well.      
The veth is linked to the host's `docker0` bridge interface creating veth pair. The `docker0` bridge is the default virtual bridge network created by Docker when it starts. It acts as a central point for containers to communicate with each other and the host system (Like Router at home).


**Network Address Translation (NAT)**               
NAT is used to allow containers to access external networks (like the internet) even though they are on a private, isolated network (e.g., 172.17.0.0/16). When a container tries to access an external service (such as a website), the host will perform source NAT (SNAT) to translate the container’s private IP address to the host’s public or routable IP address. The reverse process, called DNAT (Destination NAT), happens when the response returns to the host and is forwarded to the container.
Docker configures iptables rules on the host to implement NAT. 


A **veth pair** (short for **virtual Ethernet pair**) is a pair of virtual network interfaces that are connected to each other. You can think of it like a **virtual cable** that connects two "places" on a computer's network. 

Here's an easy-to-understand breakdown of how it works:

### Think of It Like a Tunnel

Imagine you have two rooms in a building, and there’s a **tunnel** that connects the two rooms. In the world of computers, these "rooms" are called **network namespaces** (which are like separate network environments). The **tunnel** is the **veth pair**.

- One end of the tunnel is inside a **container** (which has its own virtual room, or network namespace).
- The other end of the tunnel is on the **host machine** (your main computer that runs Docker, for example).

### How It Works

1. **Container Side**: 
   - The container has a network interface, like `eth0`, that is connected to one end of the veth pair. This gives the container access to the network, but only through the veth interface.
   
2. **Host Side**: 
   - The other end of the veth pair connects to the host’s network (like the `docker0` bridge in Docker). This means the container can send and receive network traffic through the host.

### Key Points:

- **Communication**: The two ends of the veth pair can send and receive data between each other. If data (like a network request) is sent through one end of the veth pair, it automatically comes out of the other end.
- **Virtual Cable**: It's like a **virtual cable** because there's no physical wire involved; it's all handled by software.
- **Isolation**: Each container gets its own side of the veth pair, making sure the container's network traffic is isolated from other containers unless configured otherwise.

### Example:
- When a **Docker container** is started, Docker automatically creates a veth pair. The container’s network interface (like `eth0`) is connected to one side of the veth pair. The other side is connected to the `docker0` bridge on the host machine. This allows the container to communicate with the host and even with other containers.

### Summary:
A **veth pair** is just a way to connect two network environments (like a container and the host) so they can communicate with each other. It acts like a virtual wire between the container and the host, letting them send data back and forth.





















# Network Types Inside Container




## The Flow of Communication:
### Container to Host Communication:         
When a container wants to communicate with the host, the container sends packets through its veth interface to docker0. Since docker0 is connected to the host network, the host receives the packets and routes them appropriately.

### Container to External Network (via NAT):     
If a container wants to access the external network (e.g., the internet), it sends packets to its veth interface, which forwards them to the docker0 bridge on the host. The host applies NAT to the packet. The source IP of the container (e.g., 172.17.0.2) is replaced with the host's IP address. The packet is then sent out from the host's network interface (such as eth0 or wlan0) to the external network.


### Return Traffic (from External Network):    
When the external network (e.g., a website) sends a response back to the host, the host uses its DNAT rules to forward the response to the correct container. It looks up the original source IP and port, then forwards it to the appropriate container via the veth pair.
Container-to-Container Communication:

Containers connected to the same bridge (docker0) can communicate with each other using their internal IPs (e.g., 172.17.0.x addresses).
This communication does not require NAT, as both containers are in the same private network.
Detailed Example of Docker's Networking Flow:
Setting up the Veth Pair: When a container is launched, Docker creates a veth pair. For example, the container might get the eth0 interface with the IP 172.17.0.2 in its network namespace. On the host side, a corresponding interface like vethX (e.g., veth6c2fd9) is created, which is connected to the docker0 bridge.

Communication to Host: If the container wants to communicate with the host (say, ping the host's docker0 IP 172.17.0.1), it sends the packet through its veth interface. The packet is passed to docker0 and then forwarded to the host’s network stack.

Container to External Network:

When the container wants to access an external website, it sends a packet with a destination IP (e.g., 8.8.8.8 for Google's DNS) to the docker0 bridge.
Docker’s iptables rules will ensure that the container's IP (172.17.0.2) is replaced with the host's IP address in the outgoing packet, and this is done via NAT (masquerading).
The host then routes the packet to the external network.
Return Traffic:

When the external website responds, the response is sent to the host's public IP. Docker's iptables DNAT rules will identify the response and forward it to the correct container's IP (172.17.0.2).
