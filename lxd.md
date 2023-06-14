# LXD Container Notes

### General
- get the high level info about lxc/lxd installation `lxc info`
- list containers `lxc ls`
- list clusters `lxc cluster ls`
- create a new container: `lxc launch ubuntu:22.04 mycontainer`
  - specify a profile: `... -p myprofile` (see below for more about profile management)
  - specify a config variable: `... -c limits.cpu=4 -c limits.memory=4GiB`
  - use VM instead of container: `... --vm`
- show the container info: `lxc info mycontainer`
- show the config for a container: `lxc config show mycontainer`
- push file from host to container: `lxc file push ./myfile mycontainer /full/path/`
- mount a folder from host in container: `lxc config device add mycontainer data disk source=/path/from/host path=/mnt/mounted_host_folder`

### container operations
- start a containter: `lxc start mycontainer`
- stop a containter: `lxc stop mycontainer`
- delete a containter (must be stopped): `lxc delete mycontainer`
- rename a containter (must be stopped): `lxc move mycontainer mynewcontainer`
- create a new containter from an old one (must be stopped): `lxc copy mycontainer mynewcontainer`
- run a command from the inside of a container: `lxc exec mycontainer -- service --status-all`
- access bash shell in a container: `lxc exec mycontainer -- bash` or `lxc shell mycontainer`

### Snapshots
- create a snapshot of a container: `lxc snapshot mycontainer snapshot.mycontainer.0`
- restore a snapshot: `lxc restore mycontainer snapshot.mycontainer.0`
- re-instantiate a snapshot as a new container: `lxc copy mycontainer/mysnapshot.0 mynewcontainer`
- restore a snapshot: `lxc delete mycontainer/snapshot.mycontainer.0`

### Images
- NOTE: when you create a new container from a remote image, you download a copy locally
- list all images you have in the system: `lxc image ls`
- assign an alias to an image `lxc image alias create myalias 884a62161ef5` where the last arg is the image hash
- rename an alias of an image `lxc image alias create myalias myaliasNEW`
- remove an alias of an image `lxc image alias delete myalias`
- create a new image from a container `lxc publish mycontainer/mysnapshot --alias mycontainerimage`
- export an image `lxc image export mycontainerimage ~/myfolder`



### ssh
- create a user on the container: `lxc exec mycontainer -- adduser me`
- enable password authentication: `lxc exec mycontainer -- nano /etc/ssh/sshd_config` --> `PasswordAuthentication yes`. Save and exit.

### Storage
- list the storage pools: `lxc storage list`
- more info about a storage pool: `lxc storage into mypool`
- see the sizes of the different VMs and containers `zfs list | grep containers`

### networking
- see of list of networks `lxc network list`
- add a container's ip to as an entry in the `/etc/hosts` file in order to avoid using ip: `240.217.0.70 mycontainer`. Then you can do things like: `ssh root@mycontainer`


### Profile Management:
- show a list of profiles: `lxc profile list`
- edit an LXD profile: `lxs profile edit myprofile`
- How to reassign profile: `lxc profile assign mycontainer newprofile`

