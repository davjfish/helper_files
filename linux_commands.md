#########################
# BASIC LINUX COMMANDS
#########################

shutdown -h now | +10 | 17:00 "optional message" # shutdown
shutdown -r now | +10 | 17:00 "optional message" # reboot
# concatenate
cat some_file.txt # will output files to shell; read only
# piping to text files
some_command > some_file.txt # send output from command to a files
some_command >> some_file.txt # append output from command to a files
# paginate using a pager
some_command -options | less
# search for term within output (only bring up relevant lines)
some_command -options | grep "search_term"
# view progress on a cp operation
rsync -avzP copy_src copy_dst
nohup [command to execute from ssh that will be orphaned and continue running even when session is closed]
ln -s /opt/foo /usr/bin/bar # create a soft link

##############
# SYSTEM INFO
##############
df -h # check disk space available on all mounted disks
fdisk -l # take a look at all installed disks
uname -a # provides some basic system information
sudo lshw -html | -short

##############
# FILE SYSTEM
##############
df -h # check disk space available on all mounted disks
fdisk -l # take a look at all installed disks
sudo fdisk /dev/sb[x] # will open fdisk to work in sd[x]
sudo fdisk /dev/sb[x] # will open fdisk to work in sd[x]
sudo parted # a more powerful and advanced tool for managing partions etc.
sudo mkfs.ext4 /dev/sd[x][#] # make a file system on a disk partition

###########################
# MOUNTING AND UNMOUNTING #
###########################
sudo mount /dev/sdb1 /mnt/sdb # mount a file system to a location on the root disk; note: mnt folder must be created beforehand
sudo umount /dev/sdb1 # unmount a disk
sudo blkid # get uuid of each drive
# to automount
  sudo cp /etc/fstab /etc/fstab.old
  sudo nano /etc/fstab
  #insert this line into fstab
  UUID=<uuid> <pathtomount> <filesystem> defaults 0 0
  # e.g.: UUID=fd812abd-359d-433e-a8ea-1367e312938f /mnt/backup ext4 defaults 0 0

################
# TIME AND DATE
################
#view sys date and time
date
timedatectl list-timezones # list available timezones and then set the timezone
sudo timedatectl set-timezone name_from_above_list # e.g. America/Moncton
sudo timedatectl set-ntp on # turn on timesyncd
sudo date mmddyyhhMM.ss #set sys date and time manually

############
# SAMBA
############

service samba status # check status
sudo smbpasswd -a <username> # add a user (must already exist on server)
sudo nano /etc/samba/smb.conf # edit the config file (and specify name, user access, etc)
sudo service smbd restart # restart service

##############
# APACHE
#############
# determine version
apache2 -v
#start and stop the service
sudo service apache2 start
sudo service apache2 stop
sudo service apache2 restart


###################
# MySQL
###################
systemctl status mysql.service # check status of server
sudo service mysql start|stop|restart
sudo mysqladmin [-p -u root version] # -p = prompt for password; -u use user = root; but you can also jsut run mysqladmin for list of commands]

###################
# Networking
###################
sudo netstat -tlnp #check to see which applicaiton is listening to which port



#########
# USERS
#########
cat /etc/passwd | less # view all users on sys
sudo adduser <newusername> # add new user
sudo passwd <username> # change user password
deluser <username> # delete a users
sudo adduser <username> <groupname> # add a user to a group
sudo deluser <username> <groupname> # del a user from a group
usermod -a -G <group1>,<group2> <username> # add a list of groups to a user

#########
# GROUPS
#########
cat /etc/group | less #view all groups in sys
groupmod <tab x 3> # altnerate way to view all groups
groups [username]# list all the groups that a particular user belongs to
sudo addgroup <groupname> # create a new group
sudo groupdel <groupname> # delete a group


###################
# FILE MANAGENENT
###################
sudo chmod 777 <filename> # change the permissions of a file or folder
sudo chown <username> <filename>
sudo chgrp <groupname> <filename>
