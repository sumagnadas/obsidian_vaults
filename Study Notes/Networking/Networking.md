## Vault Structure
---
 [[Networking]]
- [[#Introduction]]
- [[TCP-IP Stack Deep Dive]]
- [[Packet Crafting]]
- [[Common Network Attacks]]
## Terminology
---
- **Internet** -> A worldwide *computer* network which connects many devices which are called hosts/end systems.
- **Transmission Rate** -> measures the data speed in data bits/second.
- **Packets** -> Packets are packages of data transmitted in a network. 
- **Packet switches** -> A device which receives packets of data from an incoming communication link and forwards it via an outgoing link. (similar to a transport network)
- **Route/Path** -> The path through which a packet traverses via packet switches before reaching the destination.
- **Sockets** -> (Basic idea) File-like concept meant to represent the link between two endpoints over a network.
## Introduction
---
In today's world, Internet (from the word *Inter-Networking*) has become an indispensable part of our life. It is a network where many networks consisting of end systems are connected together via communication links and packet switches. Different types of communication links transfer data at different transmission rates depending on the medium and radio spectrum. End systems access the internet through Internet Service Providers(ISPs). There are also different protocols in place governing how transmission of data occurs. 
Since the method of communication is important, everyone must agree on what each protocol does, so one guy understands what the other guy is trying to say. These standards are developed by the Internet Engineering Task Force and these standards documents are called RFC or Request For Comments. They were named as such because they were literally *request for comments* on network design problems that were being face during implementing the internet.
### Packet Switches
---
Packets are transferred to the destination via packet switches, which work very similar to a transportation network consisting of highways, roads (**communication links**) and intersections (**packet switches**) through which multiple transport trucks (**packets**) travel. They move a large cargo (**data**) after dividing it into smaller parts which are then loaded onto trucks and they independently reach the destination warehouse via different roads and intersections where it is unloaded and assembled into the cargo we wanted to move.
## Protocols
---
A protocol defines the format and order of messages exchanged between two or more communicating entities, (two end systems on a network/Internet in our case) and the corresponding actions taken on transmission &/or receipt of a message or other event.