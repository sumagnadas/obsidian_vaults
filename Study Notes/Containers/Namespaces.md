Namespaces are one of the main building blocks of a container and they control the system view in many different ways
## PID
---
- This type of namespace is used to isolate the view of processes running in a system. 
- Processes belonging to the same PID namespace can see each other. 
- Processes have a PID for each namespace they belong to. 
- Normally the process that creates the PID namespace has the PID 1, which generally belongs to `init` normally for the kernel.
## UTS
---
- This namespace is used to isolate hostname/system name for the processes.
- This namespace can be used to make the group of processes think the system name is different altogether from the `root` namespace or the parent namespace (remember, NSes can be nested)
## Network
---
- Isolates `iptables` (firewall rules), custom routing rules, ports, addresses, etc.
- Physical network interfaces are always connected to the root namespace and cannot be connected to any child namespaces. To connect them, virtual network interfaces like `vethXXXX` are used like a bridge cable where on each side, there is a network interface.
## Mount
---
Isolates mount points and filesystem hierarchy. Everything in Linux is shown to be present under a single directory (`/`), even the mount points at some point of this directory tree. Mount namespaces are used to keep seperate copies of this tree. 
This can be used to make the root directory seperate. How, I am still a bit confused on that part so i have to use and find out.
>Basically using `chroot()` syscall changes the root directory location and not the whole directory structure, so even if the current `/` is going to be the `chroot`-ed directory, you can go up the tree from new `/` to the real root directory. This is because the directory tree is still the same. This requires an already open FD to a location outside the jail/`chroot`-ed `/` which can then be used to `fchdir` (a Linux syscall which uses a FD instead of string pathname to `cd` to the location). After `cd`, you can then go up to the previous root directory and change to it, gaining access to the host system. The open FD is required because a reference is needed outside the `chroot`-ed dir's subtree. The open FD can be used to go to it and access it. Normal `cd ..`  won't work because after reaching the currently defined root, the internal calls return the same changed `/` directory even if it has a parent. This does not mean that the whole mountpoint tree is gone; it's just unreachable. This makes it a very big vulnerability and so cannot be used for changing the root location for a container.
>To actually create an (atleast better) isolated directory structure, mount namespaces can be used to make the directory/mountpoint tree different so as to not hamper the host kernel and then use `pivot_root()` to point another **mounted** folder to the `/` directory mountpoint and move the whole host root directory subtree to somewhere under the new root dir. Then, unmount the host root and now, you got a fully different root directory which doesn't contain any references whatsoever to the actual root. 
>Some nuances:-
>1. The new folder needs to be a mountpoint and cannot be any arbitrary location.
>2. The current working directory needs to be changed to the new root or you will still be in the previous root.
>3. If there was already an FD opened or other references to inside the previous root location somewhere outside the jail, it can still be read. If those references go out, nothing can reach the previous root. Also, even if the FD is open, you can't traverse up. (more notes later with experimentation)
## User
---
Isolates UID and GID mapping in a system. Everything with permissions and what an user can do with resources is determined by capabilities of a process.
### Capabilities
Capabilities are fine-grained control parameters which delegate permissions and access control of a process with the system. 
The concept of `root` privilege and `root` access was previously binary; you either have privilege or you don't. Currently, the permissions are not binary(privileged or not privileged) and essentially a bitset, where the permissions are divided and individually given to processes/executables that might need it. For example, `ping` needs to able to communicate using raw sockets, so `CAP_NET_RAW`(capability reqd. for using raw/packet sockets) is given to it. You can give a file(probably only executables?) extra capabilities using `setcap` and get info about its capibilities using `getcap`
The capabilities of a process are defined in multiple sets - 
- Permitted   -> Capabilities it can move to effective set (basically usable caps)
- Effective     -> Capabilities that are actively enforced/being used
- Inheritable -> Capabilities that are inherited by processes summoned by `execve()` i.e. child processes
- Bounding   -> Capabilities that can be possessed by a specific process (and its child processes) in all of reality. You really can't get a capability which is not defined/permitted in this set. It can be modified.
### The namespace
An user namespace, when created, has all the UIDs and GIDs mapped to 65536 which is shown as `nobody` and `nogroup` respectively. To have the UIDs map properly to host users, one has to create an UID and GID mapping in the user namespace. You can map UID 0 to an unprivileged user outside the container.
In an user namespace, `root` user or UID 0 has privileges but with only resources owned by the namespace. Any other resources such as those owned by the parent, host are checked according to the UID mapping. If the mapped host user has the necessary permissions, then the resources can be accessed accordingly. The `root` user essentially has all privileges but only in that namespace on only its owned resources. These resources include its own net, mnt, pid, ipc namespaces.
Anything you do which touches something of any namespace that is not its child, the user's permissions are checked with respect to the mapped host user and does not work like `root` even if the user's UID is 0 in that namespace.
