## Introduction
Containers are built up of many component. Namely, they are :-
- Cgroups -> Resource limit as to what amount of what that container can use
- Namespaces -> Limit what the container sees in the system.
These two system primitives are responsible for OS-level virtualization without the use of hypervisor. This is not true virtualization and the processes that run actually run on the host kernel, just with more isolation from the host. The host IS able to see all the processes that are running inside or outside the containers but the processes running inside the container cannot see outside it.
## Cgroups
---
Cgroups are a system primitive/component which helps in fine-grained control of resources for a process or group of processes. The subsytems a cgroup can control are as follows :-
- Memory, hugeTLB
- CPU and CPUset 
- Network (net)
- Block I/O (blkio)
- Freeze
These cgroups can be used to limit the resources a group of processes can use. Initially all the processes belong to the root cgroup which has no limits or anything set. For more info, [[CGroups]]
## Namespaces
---
Namespaces are a system primitive/component to limit the system view of a process. It can only see the system items defined by the rules of its namespaces, which are divided as follows :-
- PID
- UTS
- Mount
- User
- IPC/Inter Process Communication
- Network
- Control group(cgroups)(??)
Initially a process belongs to the `root`/`default`/`init` namespace of each kind. A namespace can be nested and only the `root` namespace doesn't have a parent. For more info, [[Containers/Namespaces|Namespaces]]