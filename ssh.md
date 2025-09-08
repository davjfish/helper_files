# SSH token authentication

## Using putty

- Go to puTTy Key Generator
  - RSA --> Generate 
  - once generated, click on "save private key" --> `\user\.keys\id_rsa.ppk`
![img.png](/screenshots/puttykeygen1.png)
- In puTTY, under a given preset, go to the `connection` group, select data and enter your "auto-login username"
- Connection > SSH > Auth > Credentials to authenticate with: put the filepath to the key e.g., `\user\.keys\id_rsa.ppk`
- on the server side, create the following file: `nano ~/.ssh/authorized_keys` and add the following:
  ``` 
  ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILvweZYEWpjXV7yti88KVYMR1GF+VcLkNMTCLaxQQocR 
  ```
  This is the public key component of the ppk file saved in the above step. Make sure there are no breaklines or spaces in the key
- In general the format should be: `ssh-rsa [pubkey] name-of-key` (assuming RSA algorithm was used)
- save the file and now you should be able to log in without a password

Or in one shot:

``` bash
mkdir ~/.ssh && echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILvweZYEWpjXV7yti88KVYMR1GF+VcLkNMTCLaxQQocR" > ~/.ssh/authorized_keys
```


## Secure file transfer

From https://unix.stackexchange.com/questions/106480/how-to-copy-files-from-one-machine-to-another-using-ssh

Syntax:
`scp <source> <destination>`

To copy a file from B to A while logged into B:
`scp /path/to/file username@192.168.0.100:/path/to/destination`

To copy a file from B to A while logged into A:
`scp username@192.168.0.100:/path/to/file /path/to/destination`

