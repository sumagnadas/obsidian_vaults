A log for how i proceeded with making a container runtime from scratch in Go. 
## Goals
---
- Basic container runtime
- Commands available (first iteration)
	- `docker run (-d)`
	- `docker ps`
	- `docker freeze`
	- `docker unfreeze`
	- `docker exec`
- Future goals 
	- Multi container management system.
## Notes
---
- For using user namespaces properly with respect to the host, you need to do UID mapping. You can't do that without `sudo`. If you use `sudo`, your EUID is 0 and kernel doesn't allow you to assign UID and GID mapping to something which you are not
  For example, you can map the root UID and GID to container UID, GID 0 if you execute using `sudo` but you cannot map host UID 1000 to container UID 0, as `root`. You need to be UID 1000 to map it to some UID  in a container/user namespace.
- For `pivot_root`, the root directory or `/` needs to be a mountpoint, be it bind (for a directory mount) or an actual mount.
- Creation of unprivileged user namespaces are restricted from creation in Ubuntu, due to the large amount of attack surface it creates. Because of this, Ubuntu, specifically Canonical, implemented a restriction in apparmor for unprivileged user namespaces to not be able to create them.
- Networking and containers together are BS. 0/10, Not at all recommended apart from learning.
- For moving a process from one cgroup to another, you require write access to the common ancestor cgroup (cgroups v2) for the cgroup the PID is currently in and the one you want to move it into. To do so without privileges, there is a delegation system in place by `systemd`, where an user is given access to its own cgroup hierarchy for use without any privileges. This is normally present at `/sys/fs/cgroup/user.slice/user-<user_id>.slice/user@<user_id>.service/`