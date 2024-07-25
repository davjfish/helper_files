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
  ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAqUBwlEMj1Pd6KPC2lW54jiFDrw4/paRpKlWCQuIP1rRG2GkbhKupCrKafZkk37yF0AWh6BztKpjMR3GLGsph89FKwg1m6AiMoyJ/x3QhlpLN6aZg6apM1X/56uENoF+Yu2OjeKQ0QsfIs3BTqljG8xChRquZAl2HcvXDaLlKSK0DDjeJ+6LZHxgCPkE30tYh5drbsHTJyqzM+1BQ3PAN60hfYGYh7qURngBgabJNhgJkEjhxGvu0C9TcAA0RoyNuqUgQhUJ2JJB278XMMwwarbs/+htw36gb/DqkuW7C8WwVEzPClqTCHnsLNdLnTFcOE/AYEMOmRJ76tIPOiv5Kxw== davids-rsa-key-20230307 
  ```
  This is the public key component of the ppk file saved in the above step. Make sure there are no breaklines or spaces in the key
- In general the format should be: `ssh-rsa [pubkey] name-of-key` (assuming RSA algorithm was used)
- save the file and now you should be able to log in without a password

Or in one shot:

``` bash
mkdir ~/.ssh && echo "ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAqUBwlEMj1Pd6KPC2lW54jiFDrw4/paRpKlWCQuIP1rRG2GkbhKupCrKafZkk37yF0AWh6BztKpjMR3GLGsph89FKwg1m6AiMoyJ/x3QhlpLN6aZg6apM1X/56uENoF+Yu2OjeKQ0QsfIs3BTqljG8xChRquZAl2HcvXDaLlKSK0DDjeJ+6LZHxgCPkE30tYh5drbsHTJyqzM+1BQ3PAN60hfYGYh7qURngBgabJNhgJkEjhxGvu0C9TcAA0RoyNuqUgQhUJ2JJB278XMMwwarbs/+htw36gb/DqkuW7C8WwVEzPClqTCHnsLNdLnTFcOE/AYEMOmRJ76tIPOiv5Kxw== davids-rsa-key-20230307" > ~/.ssh/authorized_keys
```
