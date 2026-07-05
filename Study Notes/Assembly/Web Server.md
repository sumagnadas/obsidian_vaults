## Information
---
### Sockets
In Linux, everything is a file, or atleast you can assume it to be so. Sockets are the file-like concept in terms of networking. Using the syscall `socket` basically creates an endpoint for the program to interact with in terms of networking, be it via Internet  or via Bluetooth. This endpoint is a socket. We then use  the syscall `bind` to connect it to an IP address and port as `socket` just creates an endpoint for us to interact with; it does not connect anything to it.
## Challenges
---
### Exit
Just use `exit` syscall
### 
