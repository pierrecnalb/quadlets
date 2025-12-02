# Podman quadlets

This is a personnal collection of Podman Quadlets hardened for self hosted services.

From my own experience, it has been quite difficult to find proper quadlet configurations on the web.
They are very few, and most of them are either not working properly or have several drawbacks.
They do not take advantage of advanced capabilities of podman, such as pods for example, and they are not using security features.

As such, the quadlets in this repository are specially crafted to resolve those issues, when possible.

The quadlets are designed to be used as root with the following features:
- pods are used to host related containers
- containers are properly isolated from each other within their own user namespace
- containers are running with a dedicated user
- unnecessary capabilities are dropped
- the least amount of privileges are used

Technically speaking, the above can be translated with the following podman options:
- `UserNS=auto`
- `User=<uid>:<gid>`
- `ReadOnly=true`
- `NoNewPrivileges=true`
- `DropCapability=...`

## Why not rootless?

According to [this conversation](https://github.com/containers/podman/discussions/13728), Daniel Walsh (one of the developer of Podman) mentioned that "Rootless containers are great for containers run by users on a system, but if you are just running containers on a server, then --userns=auto is a more secure solution".
As he explained, when you run multiple containers as a rootless user, they run in the same user namespace so they can attack each other from a User Namespace point of view.
Instead, using rootful containers with the --userns=auto option, then those containers are running in unigue user namespace and are isolated.
In addition to that, using a dedicated user in the container and dropping capabilities/priviliges, should provide way more security than most docker compose files found on the internet.
 
