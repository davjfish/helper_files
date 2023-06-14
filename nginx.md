# Nginx Notes

### General
- Config file: `/etc/nginx/nginx.conf`
- Need to select which site configs to use with
> include /etc/nginx/conf.d/*.conf; 
> 
> include /etc/nginx/sites-enabled/*;
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
  


