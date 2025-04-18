# LINUX CHEATSHEET

## General / Misc

- reboot: `shutdown -h now | +10 | 17:00 "optional message"`; another "more serious" way to do this is described here https://www.linuxjournal.com/content/rebooting-magic-way
- concatenate; will output files to shell; read only: `cat some_file.txt`
- piping to text files: `some_command > some_file.txt`
- send output from command to a files: `some_command >> some_file.txt` # append output from command to a files
- paginate using a pager: `some_command -options | less`
- search for term within output (only bring up relevant lines): `some_command -options | grep "search_term"`
- nohup [command to execute from ssh that will be orphaned and continue running even when session is closed]
- `sudo apt-get update` updates all packages
- `sudo apt-get upgrade` upgrades all packages
- show all installed packages `apt list --installed`
- read mail for user logged in `sudo less /var/mail/$(whoami)`
- find a program: `whereis git`

- Change the machine's hostname `hostnamectl set-hostname my-new-hostname`

### File Transfers and manipulations
- empty the contents of a large file: `> error.log` or `: > error.log` or `true > error.log` or `cat /dev/null > error.log` or `cp /dev/null error.log`
  or `echo "" > error.log`
- create a soft link: `ln -s /target/file /my/link/file`
- view files using RSYNC utility
  - Basic usage: `rsync -a copy_src copy_dst` NOTE: assuming copy_src is a dir, there should be **no trailing slash**. Same for copy_dst
  - use the `--progress` option to view progress on individual files
  - use the `-z` option to compress files before transferring (don't use this if transferring many files)

## SYSTEM / FILE SYSTEM INFO

- `df -h` check disk space available on all mounted disks
- `uname -a` provides some basic system information
- `sudo lshw -html | -short` hardware information
- `lsblk` a list of storage devices and their partitions
- `sudo fdisk -l` take a look at all installed disks
- `sudo fdisk /dev/sb[x]` will open fdisk to work in sd[x]
- `sudo parted` a more powerful and advanced tool for managing partitions etc.
- `sudo mkfs.ext4 /dev/sd[x][#]` make a file system on a disk partition
- `du -sh [filelocation1] [filelocation2] ...| sort -h` view list of dirs and their sizes; sort by file size and summarize dirs
- `uname -m`
- `free -m` check memory usage
- `lscpu` view cpu detail
- `find DIR_NAME -type f | wc -l` count all the files in a directory, recursively

### File Compression, decompression and tarballs

(inspired by: [https://www.howtogeek.com/248780/how-to-compress-and-extract-files-using-the-tar-command-on-linux/](https://www.howtogeek.com/248780/how-to-compress-and-extract-files-using-the-tar-command-on-linux/))

- Compress an Entire Directory or a Single File: `tar -czvf name-of-archive.tar.gz /path/to/directory-or-file` or no compression `tar -cvf name-of-archive.tar /path/to/directory-or-file`
- Compress Multiple Directories or Files at Once: `tar -czvf archive.tar.gz /home/ubuntu/Downloads /usr/local/stuff /home/ubuntu/Documents/notes.txt`
- Exclude Directories and Files: `tar -czvf archive.tar.gz /home/ubuntu --exclude=/home/ubuntu/Downloads --exclude=/home/ubuntu/.cache`
- Extract an Archive: `tar -xzvf archive.tar.gz` or `tar -xzvf archive.tar.gz -C /tmp`

There is a great tool call fast-jar which can be install with `sudo apt install fastjar`. This seems to work really well for corrupted zips as well. Does not work for tar.gz
- extract a file: `jar xvf myfile.zip`
- use jar to store many files in a single `.jar` file: `jar cvf myfile.jar.zip /path/to/directory-or-file` or no compression `jar cvf0 myfile.jar /path/to/directory-or-file`




### Steps to reformat a flash drive [https://www.wikihow.com/Format-a-USB-Flash-Drive-in-Ubuntu](https://www.wikihow.com/Format-a-USB-Flash-Drive-in-Ubuntu)

- this is assuming your device is called `sdc`
- make sure the drive is unmounted; it should not show up in `df -lh`
- if you like, you can erase all the data on the drive `sudo dd if=/dev/zero of=/dev/sdc bs=4k status=progress && sync`. `dd` stands for `disk destroyer`. This
  operation will spend some time writing millions and millions of zeros over every nook and cranny of the /dev/sda1
  partition [https://opensource.com/article/18/7/how-use-dd-linux](https://opensource.com/article/18/7/how-use-dd-linux)
- `sudo fdisk /dev/sdc` followed by: `N` for new then `W` for write.
- `lsblk` to confirm the partition is created
- `sudo mkfs.vfat /dev/sdc1` to create a new filesystem on the partition
- to eject the disk `sudo eject /dev/sdc`
- or, to mount the partition `sudo mount /dev/sdc1 /mnt/my_sexy_folder`

### TIME AND DATE

- `date` view sys date and time
- Using `timedatectl` (preferred):
  - `timedatectl list-timezones` list available timezones and then set the timezone
  - `sudo timedatectl set-timezone name_from_above_list` e.g. America/Moncton
  - `sudo timedatectl set-ntp on` turn on timesyncd
  - `sudo timedatectl set-time '2015-11-23 08:10:40'` set date / time using timedatectl.. NOTE: you first have to turn off synchro
  - set ntp server address --> first open  `sudo nano /etc/systemd/timesyncd.conf` then add the following line: `NTP=your.ntp.server.address  # DFNSB2YwpDCP001.ENT.dfo-mpo.ca (DFO), time.google.com, ...`
  - check the status of timesyncd `timedatectl show-timesync` and `timedatectl timesync-status`
 

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
        - e.g., `UUID=fd812abd-359d-433e-a8ea-1367e312938f /mnt/backup ext4 rw,user,exec,umask=000 0 0`  --> This will mount with 777 permissions
- mount a Windows share: `sudo mount.cifs //Windowshost_or_IP/Share /home/geek/Desktop/Windows-Share -o user=geek`. Just make sure you have this
  installed `sudo apt install cifs-utils`

## Networking

- sudo netstat -tlnp #check to see which applicaiton is listening to which port
- `sudo arp-scan --interface=ens1 --localnet` scan of local network; be sure to first install arp-scan. use ifconfig to find out interface name
- you can also try this usage: `sudo arp-scan [default gateway ip]/CIDR`
- NOTE: `nmap` might be an interesting tool to explore for future use.
- `sudo nmap -sP -PI -PT 192.168.x.x/24` is an alternative to the above. replace x.x. with default gateway - router ip / netmask cidr
- `ip route` to see the DHCP server info. In general the `ip` can be a useful tool e.g., `id addr`
- `nmcli dev show [optional dev name]` to see all interesting details about network devices including mac addresses, ip address, dns

### Set a static ip

- source is
  from [here](https://linuxize.com/post/how-to-configure-static-ip-address-on-ubuntu-20-04/#:~:text=Configuring%20Static%20IP%20address%20on%20Ubuntu%20Desktop,-Setting%20up%20a&text=Depending%20on%20the%20interface%20you,IP%20address%2C%20Netmask%20and%20Gateway.)
- make sure this file is set to disabled `sudo nano /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg`
  ```
  network: {config: disabled}
  ```
- edit this file: `sudo nano /etc/netplan/01-netcfg.yaml` and under the NIC that you want to set a static ip:
  ```
  dhcp4: no
      addresses:
        - 192.168.121.221/24  # your requested IP and subnet mask
      gateway4: 192.168.121.1  # this is your default gateway
      nameservers:
          addresses: [8.8.8.8, 1.1.1.1]
  ```
- apply the changes `sudo netplan apply`

## Services

- show list of all services `service --status-all`

### Samba

- connect to an smb share (one-off) `smbclient \\\\host\\share\\ -U "DOMAIN\myuser"`
- service samba status # check status
- script to add user to linux and samba together

```
USERNAME="" && sudo adduser $USERNAME && sudo smbpasswd -a $USERNAME
```

- `sudo smbpasswd -a <username>` # add a user (must already exist on server)
- `sudo nano /etc/samba/smb.conf` # edit the config file (and specify name, user access, etc)
- `sudo service smbd restart` # restart service
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

- finally, make sure the folder you are trying to share is opened! I have been granting 777 permissions but I am not sure if there is not a better, safer way to
  do this



### FTP

- install `sudo apt install vsftpd`
- config file `/etc/vsftpd.conf `
- to upload a file to an ftp url `curl -T localfile.txt ftp://192.168.0.19/path/to/remotefile.txt --user my_user:my_pw` (might have to apt install curl first)

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

- `who -H` see who is currently logged in, or just `w`
- `cat /etc/passwd | less` view all users on sys
- `sudo adduser <newusername>` add new user
- `sudo usermod -aG sudo <username>` add user to the sudo group
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
- logout a user `sudo pkill -KILL -u MYSUSER`


## Scheduling Processes

-`sudo crontab -l` view current crontab -`sudo crontab -e` edit current crontab

- in crontab file:
    - do something every day (at midnight) --> ```0 0 *  * * bash /usr/local/bin/my_daily_script_to_run.sh```
    - do something every hour --> ```0 * *  * * bash /usr/local/bin/my_hourly_script_to_run.sh```
- see below for a good autoupdate script and related crontab entries
- check logs from running crontab: `sudo grep CRON /var/log/syslog`


## Printing

- make sure you have cups and cups-client installed on the machine: `sudo apt install cups cups-client`
- view list of printers by device `lpstat -p -d`. The `-d` arg is to see what the current system default is set to, `-p` is for the list of printers
- check out the man for lpstat: [https://www.cups.org/doc/man-lpstat.html](https://www.cups.org/doc/man-lpstat.html)

- check out the man for lpoptions: [https://www.cups.org/doc/man-lpoptions.html](https://www.cups.org/doc/man-lpoptions.html)
- and check out this page on cups options from the cups man: [https://www.cups.org/doc/options.html](https://www.cups.org/doc/options.html)
- set a default printer `lpoptions -d [printer_name_from_above_list]`
- look at the options of a specific printer `lpoptions -p [printer_name_from_above_list] -l`
- set a default options for a specific printer `lpoptions -p [printer_name_from_above_list] -o [prop_name]=[value]` e.g.:
    - lpoptions -p [printer_name_from_above_list] -o media=62x100mm`

- check out the man for lp: [https://www.cups.org/doc/man-lp.html](https://www.cups.org/doc/man-lp.html)
- here are some useful examples for printing:
    - `lp my_test_file`
    - `lp my_test_file -d PRINER_NAME`
    - `lp my_test_file -o landscape`
    - `lp my_test_file -o landscape -o media=62x100mm`

- check the status of active jobs: `lpstat -R` or `lpstat -W all|not-completed|completed`
- cancel a job: `cancel [name_of_job]`
- to check if cups browsing is on:
  ```
  sudo service cups-browsed status
  sudo service cups-browsed start
  # etc...
  ```


### use the python wrapper for CUPS

- install the following: `sudo apt-get install libcups2-dev`
- install: `pip install pycups`
- Example:

```
# from https://pypi.org/project/pycups/
import cups
conn = cups.Connection()
# look at list of printers
conn.getPrinters()
# look at list of jobs
conn.getJobs()
# print something
conn.printFile("PRINTER_NAME", PATH_TO_FILE, "NAME OF JOB",{"media":"62x100mm", "option1":"value1",})
```

### Add a startup script

from [https://transang.me/create-startup-scripts-in-ubuntu/](https://transang.me/create-startup-scripts-in-ubuntu/)

1. create a `.service` file: `sudo nano /etc/systemd/system/my-service.service`
2. add the following lines inside the file:

  ```
  [Unit]
  Description=My custom startup script
  
  [Service]
  ExecStart=/home/transang/startup.sh start
  
  [Install]
  WantedBy=multi-user.target
  ```

3. `systemctl start my-service.service` to start the service
4. `systemctl enable my-service.service` to enable the service on startup
5. make sure the permissions and modes of the script file are set correctly (i.e. can be executed) and that the script file's first line is `#!/bin/bash`

### Aliasing

- edit the `.bashrc` file: `nano ~/.bashrc`
- at the bottom, add the alias: `alias rm='rm -i'`
- execute the bashrc file: `. ~/.bashrc`
- [reference](https://askubuntu.com/questions/17536/how-do-i-create-a-permanent-bash-alias)

## Environment Variables (from [https://linuxize.com/post/how-to-set-and-list-environment-variables-in-linux/](https://linuxize.com/post/how-to-set-and-list-environment-variables-in-linux/))

- list variables `printenv`
- get value for specific variable `printenv Var1 var2 var3... `
- create an environmental var for current session: `export MY_NEW_VAR="My New Var"`
- add variable to system-wide variable: in `/etc/environment` add your variables:
    ```
    FOO=bar
    VAR_TEST="Test Var"
    ```

### firewall

- good
  resource: [https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-18-04](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-18-04)
- reset to default config:

```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

- allow incoming ssh: `sudo ufw allow ssh` or `sudo ufw allow 22`
- allow incoming range port: `sudo ufw allow 6000:6007/tcp`
- allow incoming from a specific ip address: `sudo ufw allow from 203.0.113.4`
- allow incoming traffic from a local network: `sudo ufw allow from 192.168.0.0/24`  where 192.168.0.0 is the pattern of the local network.
- allow incoming from a specific ip address to a specific port: `sudo ufw allow from 203.0.113.0/24 to any port 22`
- allow incoming from a specific ip address to a specific port on a specific NIC: `sudo ufw allow in on eth0 to any port 80`
- you can replace `allow` with `deny` for any of the above
- get a list of rules with numbers: `sudo ufw status numbered`
- delete a rule by its number: `sudo ufw delete 2`
- checking the status of rules: `sudo ufw status verbose`
- enable rules: `sudo ufw enable` (note: for each new rule added, you will have to enable)
- allow samba: `sudo ufw allow samba`
- reset all rules: `sudo ufw --force dis able` then `sudo ufw --force reset` then `sudo ufw default deny incoming` then `sudo ufw default allow outgoing`
- show pending rules: `sudo ufw show added`
- allow all and any incoming traffic on a certain port: `sudo ufw allow from any to any port 19132 proto tcp`

### I/O

- display output from serial port: `screen /dev/ttyS0/ 4800`
- listen to a UPD port: `netcat -l -u -p 4001`


# random jewels

- how to adjust behavior when closing the lid of a
  laptop: [https://askubuntu.com/questions/141866/keep-ubuntu-server-running-on-a-laptop-with-the-lid-closed](https://askubuntu.com/questions/141866/keep-ubuntu-server-running-on-a-laptop-with-the-lid-closed)

# crontab entries for autoupdate / autoupgrade

- open the file: `sudo nano /usr/local/bin/autoupdate.sh`:

```
#!/bin/bash

apt-get update
apt-get -y upgrade
apt-get -y dist-upgrade
apt-get clean
apt-get -y autoremove
```

- make sure it is executable: `sudo chmod 744 /usr/local/bin/autoupdate.sh`
- go into your crontab: `sudo crontab -e`

```
# run the autoupdate script at 12:00am
0 0   *   *   *    /usr/local/bin/autoupdate.sh

# reboot your system every day at 4:05am
0 4   *   *   *    /sbin/shutdown -r +5
```

# Disable Shutdown, Restart, Suspend, etc., for all users in GUI

https://askubuntu.com/questions/93542/how-to-disable-shutdown-reboot-suspend-hibernate/93956#93956

- `sudo nano /etc/polkit-1/localauthority/50-local.d/restrict-login-powermgmt.pkla`
- add the following lines:
  ```
  [Disable lightdm PowerMgmt]
  Identity=unix-user:*
  Action=org.freedesktop.login1.reboot;org.freedesktop.login1.reboot-multiple-sessions;org.freedesktop.login1.power-off;org.freedesktop.login1.power-off-multiple-sessions;org.freedesktop.login1.suspend;org.freedesktop.login1.suspend-multiple-sessions;org.freedesktop.login1.hibernate;org.freedesktop.login1.hibernate-multiple-sessions
  ResultAny=no
  ResultInactive=no
  ResultActive=no
  ```
  

### Create a bootable startup disk

(from https://www.tecmint.com/create-bootable-ubuntu-usb-disk/)

- install gddrescue: `sudo apt install gddrescue`
- use `lsblk` to identify usb key, e.g., `/dev/sdb`
- then using ddrescue: `sudo ddrescue ubuntu-20.04-beta-desktop-amd64.iso /dev/sdb --force -D`
- if using desktop, sometime to create the disk you have to be in sudo mode. Therefore, fom cmd terminal run: `sudo usb-creator-gtk` instead of opening the app via GUI

### Logical Volume Manager

- How to expand a volume
  - Described here: https://packetpushers.net/blog/ubuntu-extend-your-default-lvm-space/  
  - `sudo vgdisplay` to see how much the current volume can be expanded
  - `sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv` to expand the volume; confirm with `vgdisplay`
  - `sudo resize2fs /dev/mapper/ubuntu–vg-ubuntu–lv` to extend the actual filesystem


# Sync with google drive (Rclone)

 - install with apt
 - will need to run `rclone config` to link to google account, see docs: https://rclone.org/drive/
 - Need to put in the google drive location "hash" as the "root_folder_id", this lets rclone know which folder to link to
 - then can copy files over with:  
 
``` bash
rclone copy rclone_test: /path/to/local/dir --bwlimit=8.5M --progress
# 'rclone_test:' is the name of the config / drive directory to copy
# rclone copy will only add, rclone sync will also delete things
```
