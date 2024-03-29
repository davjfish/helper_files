# Nginx Notes

### General
- Config file: `/etc/nginx/nginx.conf`
- Need to select which site configs to use with
> include /etc/nginx/conf.d/*.conf; 
> 
> include /etc/nginx/sites-enabled/*;

- also need to increase the max file size:
> # allow larger file sizes
> client_max_body_size 5M;
  

- sample site config to proxy for lxc container (/etc/nginx/conf.d/mysite.conf):
```
server {
        listen 80 default_server; 
        # [::] is for ip6
        listen [::]:80 default_server; 

        # this is the DNS hostname of the server, set in /etc/hostname + /etc/hosts
        server_name glf-science-0;

        # redirect incoming traffic on port 80 to lxc container ip
        location / {
                proxy_pass http://240.217.0.190/;
                include proxy_params;
        }
        
        # the trailing '/' will strip off the location string
        location /preprod {
                proxy_pass http://240.217.0.190/;
                include proxy_params;
        }
}
```
  
- example config to forward ssh access through a port
```
upstream my-config-name {
      server 240.217.0.190:22;
}

server {
  listen 22018;
  proxy_pass my-config-name;
}
```




