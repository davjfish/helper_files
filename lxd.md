# LXD Container Notes

### General
- get the high level info about lxc/lxd installation `lxc info`
- list containers `lxc ls`
- create a new container: `lxc launch ubuntu:22.04 mycontainer`
  - specify a profile: `... -p myprofile` (see below for more about profile management)
  - specify a config variable: `... -c limits.cpu=4 -c limits.memory=4GiB`
  - use VM instead of container: `... --vm`
- show the container info: `lxc info mycontainer`
- show the config for a container: `lxc config show mycontainer`


### container operations
- start a container: `lxc start mycontainer`
- stop a container: `lxc stop mycontainer`
- delete a container (must be stopped): `lxc delete mycontainer`
- rename a container (must be stopped): `lxc move mycontainer mynewcontainer`
- create a new container from an old one (must be stopped): `lxc copy mycontainer mynewcontainer`
- run a command from the inside of a container: `lxc exec mycontainer -- service --status-all`
- access bash shell in a container: `lxc exec mycontainer -- bash` or `lxc shell mycontainer`
- push file from host to container: `lxc file push ./myfile mycontainer /full/path/`


### Devices
- mount a folder from host in container: `lxc config device add mycontainer data disk source=/path/from/host path=/mnt/mounted_host_folder`
- mount a usb device in a container: `lxc config device add mycontainer ttyACM0 unix-char mode=0666 gid=20 path=/dev/ttyACM0`

### Snapshots
- create a snapshot of a container: `lxc snapshot mycontainer snapshot.mycontainer.0` use the `--resuse` flag to overwrite the file
- restore a snapshot: `lxc restore mycontainer snapshot.mycontainer.0`
- re-instantiate a snapshot as a new container: `lxc copy mycontainer/mysnapshot.0 mynewcontainer`
- restore a snapshot: `lxc delete mycontainer/snapshot.mycontainer.0`
- schedule the collection of snapshot: `lxc config set <instance_name> snapshots.schedule @daily` or `lxc config set <instance_name> snapshots.schedule "0 6 * * *"` to collect the snapshot at 6 am. 

### Images
- NOTE: when you create a new container from a remote image, you download a copy locally
- list all images you have in the system: `lxc image ls`
- assign an alias to an image `lxc image alias create myalias 884a62161ef5` where the last arg is the image hash
- rename an alias of an image `lxc image alias rename myalias myaliasNEW`
- remove an alias of an image `lxc image alias delete myalias`
- create a new image from a container `lxc publish mycontainer/mysnapshot --alias mycontainerimage`
  - NOTE: best to publish from a snapshot otherwise you need to stop the container
- export an image to file `lxc image export mycontainerimage ~/myfolder`
- import an image from file `lxc image import 725a02bf5e68.tar.gz` then `lxc image alias create mynewimagealias 725a02bf5e68`


### clusters
- **for clustering to work, all versions of lxd must be the same version!!**
- Make sure that all members of the cluster have the same system time.
- list all nodes in cluster `lxc cluster ls`
- add a new node to the cluster: `lxc cluster add <FQDN of new host>`. This should be run on from within the cluster. Then run `sudo lxd init` on the new node and copy the token.
- move an instance from one node to another: 
  ```
  lxc stop c1
  lxc move c1 --target node1
  lxc start c1
  ``` 
- remove a cluster from a node: `lxc cluster remove <mycluster>`. If server is dead, you might need to use the `--force` flag.
- You may also need to remove the old trusts on the cluster: `lxc config trust list`
- It may also be necessary to reset the cluster db leader:  `sudo lxd cluster recover-from-quorum-loss`
- **NOTE ABOUT SUBNETS** If the different nodes are on different networks, you will have to configure the `fan.underlay_subnet` in the `lxc network edit ldxfan0` to something appropriate for all networks. For example, if the inital network is `142.130.6.0/24` and the other network is `142.130.4.0/24`, you will have to set the fan.underlay subnet to `142.130.0.0/16`
- **NOTE ABOUT UPDATES** Snap periodically runs `snap refresh` which can cause some havoc in the cluster if it upgrades lxd. When this happens, cluster nodes will become "blocked" and go offline (the containers stay up though).  The strong indicator is that the lxd versions will be out of sync and lxc commands will stop working on upgraded nodes. To upgrade the remaining nodes, run `sudo snap stop lxd`, `sudo snap refresh lxd` and then restart any containers that were stopped.   
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
