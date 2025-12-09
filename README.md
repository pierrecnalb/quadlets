# Podman quadlets

This is a personnal collection of Podman Quadlets hardened to properly secure self hosted services.

From my own experience, it has been quite difficult to find proper quadlet configurations on the web.  
They are very few, and most of them are either not working properly or have several drawbacks.  
They do not take advantage of advanced capabilities of podman, such as pods or `userns=auto` for example, and they are not using security features.  

As such, the quadlets in this repository are specially crafted to resolve those issues, when possible.  

The quadlets are designed to be used with the following features:
- pods are used to host related containers
- containers from different pods are properly isolated from each other within their own user namespace
- containers are running with a dedicated user
- the least amount of privileges are used
- unnecessary capabilities are dropped (when possible)

Technically speaking, the above can be translated with the following podman options:
- `UserNS=auto`
- `User=<uid>:<gid>`
- `ReadOnly=true`
- `NoNewPrivileges=true`
- `DropCapability=...`

With those options, the provded quadlets offer way more security than most of the docker compose found on the internet.


## Rootful/Rootless considerations

Below are some important considerations mostly taken from [this podman conversation](https://github.com/containers/podman/discussions/13728):

- Both rootful and rootless podman can be used with those quadlets.  
- From a security point of view, they behave in the same way _at runtime_, since both rootful (with `userns=auto`) and rootless containers do not have root capabilities outside the containers. This is because all container users (including root) are mapped to unprivileged UIDs in the host namespace.  
- One of the most important option for a server running all containers is `UserNS=auto`, as it allows to run all containers in a separate User Namespace with isolated UIDs [see](https://github.com/containers/podman/discussions/13728#discussioncomment-11051798)  
  (In fact, using rootless podman _without_ `UserNS=auto` would run multiple containers in the same user namespace, so they could attack each other!)
- The only difference between rootful podman --userns=auto and rootless podman --userns=auto, is what the podman command itself can do, or be tricked to be done.
- Rootless with `UserNS=auto` is in overall more secure, but more complicated to setup.


## Rootful usage

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

## Rootless usage

TODO:  
_As explained in the above section, those quadlets offer significant security enhancements *in both rootless and rootful cases* compared to most docker-compose, but rootless is harder to manage, so currently I am only testing and documenting rootful usage._
