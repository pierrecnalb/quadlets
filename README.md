# Podman quadlets

This is a personnal collection of Podman Quadlets hardened for self hosted services.

From my own experience, it has been quite difficult to find proper quadlet configurations on the web.  
They are very few, and most of them are either not working properly or have several drawbacks.  
They do not take advantage of advanced capabilities of podman, such as pods for example, and they are not using security features.  

As such, the quadlets in this repository are specially crafted to resolve those issues, when possible.  

The quadlets are designed to be used as root with the following features:
- pods are used to host related containers
- containers from different pods are properly isolated from each other within their own user namespace
- containers are running with a dedicated user
- unnecessary capabilities are dropped
- the least amount of privileges are used

Technically speaking, the above can be translated with the following podman options:
- `UserNS=auto`
- `User=<uid>:<gid>`
- `ReadOnly=true`
- `NoNewPrivileges=true`
- `DropCapability=...`

## Prerequesites

All the files must be copied to `/etc/containers/systemd/`.  
In order to make `UserNS=auto` working properly, you need to first allocate the UIDs and GIDs to be used for these containers.  
Podman recommends that you allocate the highest 2 billion UIDs for your containers.   
You can do this by adding the following containers line to your `/etc/subuid` and `/etc/subgid` file.  
```sh
# cat /etc/subuid
user:100000:65536
containers:2147483647:2147483648 
# cat /etc/subgid
user:100000:65536
containers:2147483647:214748364
```



## Why not rootless?

According to [this conversation](https://github.com/containers/podman/discussions/13728), Daniel Walsh (one of the developer of Podman) mentioned that _"Rootless containers are great for containers run by users on a system, but if you are just running containers on a server, then --userns=auto is a more secure solution"_.  
As he explained, when you run multiple containers as a rootless user, they run in the same user namespace so they can attack each other from a User Namespace point of view.  
Instead, using rootful containers with the `--userns=auto` option, then those containers are running in unique user namespace and are isolated.  
In addition to that, using a dedicated user in the container and dropping capabilities/privileges, should provide way more security than most docker compose files found on the internet.
 
