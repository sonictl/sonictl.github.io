---
layout: post
title:  "Fixing certbot does not know where nginx is located in your server"
date: 2022-03-30 10:19:54 +0800
categories: jekyll
slug: p20220330101954
---
# Fixing certbot does not know where nginx is located in your server

If your certbot cannot auto configure ssl for your host:

## The cause of this problem

  Mine is because the installation path of nginx is not the default one， such as /etc/nginx or /usr/bin/nginx


## When Certbot is unable to automatically configure your web:

  Follow the GPT's answer:

```
ME: 
  Certbot doesn't know how to automatically configure the web server on this system.

GPT:
  If Certbot is unable to automatically configure your web server, it usually means that it couldn't identify the web server software and configuration on your system. To address this issue, you may need to specify the web server and its configuration manually when running Certbot.
  command:
    certbot --apache
  or:
    certbot --nginx

ME:
  The requested nginx plugin does not appear to be installed.

GPT:
  Install Certbot and the Nginx plugin using the following command:
    apt install certbot python3-certbot-nginx
    certbot --nginx

```

## solution 2: create a link to your path of nginx, manually assign path of conf files.

  - The systemd service file `/etc/systemd/system/nginx.service` may need change.
  - Carefully change the path of /etc/nginx/nginx.conf and the `include path` in it.
  - use `ln -s` command to create link from certbot known location to real location of conf file.

  #### An Example:

  The executable binary of nginx is: `/etc/nginx/sbin/nginx` and the real conf path is `/etc/nginx/conf/`

  ```
    $ ln -s /etc/nginx/sbin/nginx /usr/bin/nginx
    $ ln -s /etc/nginx/conf/ /etc/nginx
  ```
you'll see  `/etc/nginx/sbin/nginx -c /etc/nginx/conf/nginx.conf` in `nginx.service` file.

`vim /etc/systemd/system/nginx.service ` 

modify:` /etc/nginx/conf/nginx.conf -> /etc/nginx/nginx.conf`

if the .service file contains `nginx -t` modify it into `nginx -t -c <path of nginx conf file>`



## solution 3: Use `certbot certonly` and conf file for nginx server node:

  cmd: `certbot certonly`

  answer the prompts given by certbot. If you meet `could not bind port 80` error, stop the nginx service that using 80 port.

  - Congratulations! 
    Your certificate and chain have been saved at:
    `/etc/letsencrypt/live/<your_domain_name>/fullchain.pem`
    Your key file has been saved at:
    `/etc/letsencrypt/live/<your_domain_name>/privkey.pem`


## A conf file for nginx web server

  replace `<your_domain_name>` with your domain name
  refer the sample below for your nginx config file such as ` <your_domain_name>.conf`

```
server {
    server_name <your_domain_name>;

    location / {
      proxy_pass http://localhost:3000;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection 'upgrade';
      proxy_set_header Host $host;
      proxy_cache_bypass $http_upgrade;
    }

    listen 443 ssl; 
    ssl_certificate     /etc/letsencrypt/live/<your_domain_name>/fullchain.pem; 

    ssl_certificate_key /etc/letsencrypt/live/<your_domain_name>/privkey.pem; 
    #include /etc/letsencrypt/options-ssl-nginx.conf; 
    #ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; 

}

server {
    if ($host = <your_domain_name>) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    server_name <your_domain_name>;
    listen 80;
    return 404; # managed by Certbot
}

```

and 

```
	server {
	  listen       80;
	  listen       443 ssl;
	  server_name  <your domain name>;

	  ssl_certificate      /etc/letsencrypt/live/<your_domain_name>/fullchain.pem; 
	  ssl_certificate_key  /etc/letsencrypt/live/<your_domain_name>/privkey.pem; 

	  location / {
		proxy_pass http://localhost:2000;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection 'upgrade';
		proxy_set_header Host $host;
		proxy_cache_bypass $http_upgrade;
	  }
	}
```


### For solution 3: Renewal the cert for a spec domain

  cmd: ` certbot certonly -d <your_domain_name> `
  follow the prompt and renewal.




