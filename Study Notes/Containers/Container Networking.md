Containers have their own network namespace, which isolates them from the host machine and cannot communicate online via any means until an interface is connected to it. This makes it easier to distinguish as well as separate rules for routing, firewall, etc from the host.
## Concepts
---
- **Interfaces**: Networking for a Unix-based system happens through network interfaces. This can be physical i.e. hardware or virtual i.e. connecting one thing to another on the same system via software.
## Making connections
---
Due to having their own isolated network space, containers can't connect with anything. A new network namespace only has a loopback interface `lo`, which is basically the localhost and routes all connection from the interface to the system itself. This interface is present in a `down` state, and has to be enabled before anything can be done. 
To connect with anything outside the container, a virtual ethernet interface has to be added to the network namespace. Virtual Ethernet interfaces are virtual network interface which exist in pairs and used to connect two different namespace. Each end of the interface stays in different namespaces and, once enabled, facilitates network traffic between namespaces where each packet sent through one end of the interface is received at the other end immediately.
