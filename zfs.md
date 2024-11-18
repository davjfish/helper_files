- show a list of snapshots:  `zfs list -t snapshot | grep -i boot`
- cleanup snapshots: 

  ```
  sudo rm /tmp/cleanup.sh
  sudo touch /tmp/cleanup.sh
  sudo chmod 744 /tmp/cleanup.sh
  sudo zfs list -t snapshot | grep -i boot | awk '{print "zfs destroy "$1}' > /tmp/cleanup.sh 
  sudo bash -x /tmp/cleanup.sh
  
  ```

- https://docs.oracle.com/cd/E19253-01/819-5461/gbiqe/index.html
- destroy a pool `sudo zfs destroy -r rpool/incus`