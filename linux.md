# LINUX CHEATSHEET

## General / Misc
- reboot: `shutdown -h now | +10 | 17:00 "optional message"`  
- concatenate; will output files to shell; read only: `cat some_file.txt`   
- piping to text files: `some_command > some_file.txt`  
- send output from command to a files: `some_command >> some_file.txt` # append output from command to a files  
- paginate using a pager: `some_command -options | less`  
- search for term within output (only bring up relevant lines): `some_command -options | grep "search_term"`
- view progress on a cp operation (using rsync): `rsync -avzP copy_src copy_dst` NOTE: assuming copy_src is a dir, there should be no trailing slash. Same for copy_dst  
- nohup [command to execute from ssh that will be orphaned and continue running even when session is closed]  
- create a soft link: `ln -s /target/file /my/link/file`
- `sudo apt-get update` updates all packages
- `sudo apt-get upgrade` upgrades all packages
- show all installed packages `apt list --installed`
- read mail for user logged in `sudo less /var/mail/$(whoami)`

## SYSTEM / FILE SYSTEM INFO
- `df -h` check disk space available on all mounted disks
- `uname -a` provides some basic system information
- `sudo lshw -html | -short` hardware information
- `sudo fdisk -l` take a look at all installed disks
- `sudo fdisk /dev/sb[x]` will open fdisk to work in sd[x]
- `sudo parted` a more powerful and advanced tool for managing partitions etc.
- `sudo mkfs.ext4 /dev/sd[x][#]` make a file system on a disk partition
- `du -sh [filelocation1] [filelocation2] ...| sort -h` view list of dirs and their sizes; sort by file size and summarize dirs
- `uname -m`

### TIME AND DATE
- `date` view sys date and time
- `timedatectl list-timezones` list available timezones and then set the timezone
- `sudo timedatectl set-timezone name_from_above_list` e.g. America/Moncton
- `sudo timedatectl set-ntp on` turn on timesyncd
- `sudo date mmddyyhhMM.ss` set sys date and time manually
- To set the timezone of your system clock do the following: `cp /usr/share/zoneinfo/Canada/Atlantic /etc/localtime`
- Set date from the command line: `date +%Y%m%d -s "20120418"`
- Set time from the command line: `date +%T -s "11:14:00"`

## FILE MANAGEMENT
- `sudo chmod 777 <filename>` change the permissions of a file or folder or 'chmod u=rwx,g=rx,o=r <myfile>'
- `sudo chown <username> <filename>`
- `sudo chgrp <groupname> <filename>`

## MOUNTING AND UNMOUNTING #
- `sudo fdisk -l` first, find the disk you are looking to mount
- `sudo mount /dev/sdb1 /mnt/sdb`  mount a file system to a location on the root disk; note: mnt folder must be created beforehand
- `sudo umount /dev/sdb1` # unmount a disk
- `sudo blkid`  get uuid of each drive
- To automount a drive
  - `sudo cp /etc/fstab /etc/fstab.old`
  - `sudo nano /etc/fstab`
  - insert this line into fstab: `UUID=<uuid> <pathtomount> <filesystem> defaults 0 0`
    - e.g., `UUID=fd812abd-359d-433e-a8ea-1367e312938f /mnt/backup ext4 defaults 0 0`

## Networking
- sudo netstat -tlnp #check to see which applicaiton is listening to which port
- `sudo arp-scan --interface=ens1 --localnet` scan of local network; be sure to first install arp-scan. use ifconfig to find out interface name

## Services

- show list of all services `service --status-all`

### Samba
- service samba status # check status
- script to add user to linux and samba together
```
USERNAME="" && sudo adduser $USERNAME && sudo smbpasswd -a $USERNAME
```
- sudo smbpasswd -a <username> # add a user (must already exist on server)
- sudo nano /etc/samba/smb.conf # edit the config file (and specify name, user access, etc)
- sudo service smbd restart # restart service
- `sudo pdbedit -L -v` list all samba users
- example samba share - any samba user will be able to access this:
```
[name_of_share_folder]
    comment = Samba on Ubuntu
    path = /home/username/sambashare
    read only = no
    browsable = yes
    guest ok = no
```
- another share config (restricted to list of users)
```
[Crab]
   path = /mnt/glfsci_inv/Crab
   browseable = no
   writeable = yes
   guest ok = no
   valid users = admin,wadee,surettet,boudreaus
```

### FTP
- install `sudo apt install vsftpd`
- config file `/etc/vsftpd.conf `



### Apache
- apache2 -v # determine version
- sudo service apache2 start
- sudo service apache2 stop
- sudo service apache2 restart

### MySQL
- systemctl status mysql.service # check status of server
- sudo service mysql start|stop|restart
- sudo mysqladmin [-p -u root version] # -p = prompt for password; -u use user = root; but you can also jsut run mysqladmin for list of commands]

## Users and Groups
- `who -H` see who is currently logged in
- `cat /etc/passwd | less` view all users on sys
- `sudo adduser <newusername>` add new user
- script to change username and home dir of existing user
```
newUsername="" && oldUsername="" && sudo usermod -l $newUsername $oldUsername && sudo usermod -d /home/$newHomeDir -m $newUsername
```
- `sudo passwd <username>` change user password
- `deluser <username>` delete a users
- `sudo adduser <username> <groupname>` add a user to a group
- `sudo deluser <username> <groupname>` del a user from a group
- `usermod -a -G <group1>,<group2> <username>` add a list of groups to a user

- `cat /etc/group | less` #view all groups in sys
- `groupmod <tab x 3>` # alternate way to view all groups
- `groups [username]` list all the groups that a particular user belongs to
- `sudo addgroup <groupname>` create a new group
- `sudo groupdel <groupname>` delete a group
- `su root` switch user to root (BE CAREFUL)

## Scheduling Processes
-`sudo crontab -l` view current crontab
-`sudo crontab -e` edit current crontab
