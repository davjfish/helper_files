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
- find a program: `whereis git`
- empty the contents of a large file: `> error.log` or `: > error.log` or `true > error.log` or `cat /dev/null > error.log` or `cp /dev/null error.log` or `echo "" > error.log`

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
- `find DIR_NAME -type f | wc -l` count all the files in a directory, recursively

### Steps to reformat a flash drive [https://www.wikihow.com/Format-a-USB-Flash-Drive-in-Ubuntu](https://www.wikihow.com/Format-a-USB-Flash-Drive-in-Ubuntu)
- this is assuming your device is called `sdc`
- make sure the drive is unmounted; it should not show up in `df -lh`
- if you like, you can erase all the data on the drive `sudo dd if=/dev/zero of=/dev/sdc bs=4k status=progress && sync`. `dd` stands for `disk destroyer`. This operation will  spend some time writing millions and millions of zeros over every nook and cranny of the /dev/sda1 partition [https://opensource.com/article/18/7/how-use-dd-linux](https://opensource.com/article/18/7/how-use-dd-linux)
- `sudo fdisk /dev/sdc` followed by: `N` for new then `W` for write. 
- `lsblk` to confirm the partition is created
- `sudo mkfs.vfat /dev/sdc1` to create a new filesystem on the partition
- to eject the disk `sudo eject /dev/sdc`
- or, to mount the partition `sudo mount /dev/sdc1 /mnt/my_sexy_folder`


### TIME AND DATE
- `date` view sys date and time
- `timedatectl list-timezones` list available timezones and then set the timezone
- `sudo timedatectl set-timezone name_from_above_list` e.g. America/Moncton
- `sudo timedatectl set-ntp on` turn on timesyncd
- `sudo date mmddyyhhMM.ss` set sys date and time manually
- To set the timezone of your system clock do the following: `cp /usr/share/zoneinfo/Canada/Atlantic /etc/localtime`
- Set date from the command line: `date +%Y%m%d -s "20120418"`
- Set time from the command line: `date +%T -s "11:14:00"`
- Sync the system with a network time protocol (NTP) server: `sudo ntpdate DFNSB2YwpDCP001.ENT.dfo-mpo.ca`. Common time servers include: `time.nrc.ca`

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
 - mount a Windows share: `sudo mount.cifs //Windowshost_or_IP/Share /home/geek/Desktop/Windows-Share -o user=geek`. Just make sure you have 
this installed `sudo apt install cifs-utils`

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
- finally, make sure the folder you are trying to share is opened! I have been granting 777 permissions but I am not sure if there is not a better, safer way to do this

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
- `who -H` see who is currently logged in
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

## Scheduling Processes
-`sudo crontab -l` view current crontab
-`sudo crontab -e` edit current crontab
- in crontab file: 
    - do something every day (at midnight) --> ```0 0 *  * * bash /usr/local/bin/my_daily_script_to_run.sh```
    - do something every hour --> ```0 * *  * * bash /usr/local/bin/my_hourly_script_to_run.sh```


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