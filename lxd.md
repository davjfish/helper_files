# LXD Container Notes

### General
- list containers `lxc ls`
- list clusters `lxc cluster ls`
- create a new container: `lxc launch ubuntu:22.04 mycontainer`
  - specify a profile: `... -p myprofile` (see below for more about profile management)
  - specify a config variable: `... -c limits.cpu=4 -c limits.memory=4GiB`
  - use VM instead of container: `... --vm`
- show the config for a container: `lxc config show mycontainer`
- push file from host to container: `lxc file push ./myfile mycontainer /full/path/`
- mount a folder from host in container: `lxc config device add mycontainer data disk source=/path/from/host path=/mnt/mounted_host_folder`
- start a containter: `lxc start mycontainer`
- stop a containter: `lxc stop mycontainer`
- delete a containter (must be stopped): `lxc delete mycontainer`

### Storage
- list the storage pools: `lxc storage list`
- more info about a storage pool: `lxc storage into mypool`
- see the sizes of the different VMs and containers `zfs list | grep containers`

### networking
- see of list of networks `lxc network list`

### Profile Management:
- show a list of profiles: `lxc profile list`
- edit an LXD profile: `lxs profile edit myprofile`

