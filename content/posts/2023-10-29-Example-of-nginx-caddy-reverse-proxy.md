---
layout: post
title:  "Example of Reverse Proxy Configuration - Nginx & Caddy"
date: 2023-10-29 10:19:54 +0800
categories: embedded
slug: p20231029101954
---

### Caddy2 Reverse Proxy Configuration for vue+nodejs web app
```
yourdomain.com {
    # if the nodejs root is: /var/www/yourdomain.com/backend
    # if the vue root is: /var/www/yourdomain.com/frontend
    root * /var/www/yourdomain.com/frontend/dist
    encode gzip zstd

    # handle req for yourdomain.com/api/*
    handle /api/* {
        # configure for the backend nodejs serve on port 2001
        reverse_proxy 127.0.0.1:2001
    }

    handle {
        try_files {path} /index.html
        file_server
    }
}
```

### Example of Caddy Reverse Proxy Configuration

```
# This is a comment
yourdomain.com {
  # This is also a comment
  reverse_proxy /myroute localhost:2024

  # May take option1/option2 either:

  #  option1: a static web server for `index.html` in /var/www/mysite
  root * /var/www/mysite
  file_server

  #  option2:
  reverse_proxy https://www.abc.com {
        header_up Host {upstream_hostport}
  }
}
```

### Example of Nginx Reverse Proxy Configuration

```
server {
    listen          80;
    listen          [::]:80;

    server_name     <your_server_name>;
    rewrite ^(.*)$  https://$host$1 permanent;
}

map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}

server {
    listen  443       ssl http2;
    listen  [::]:443  ssl http2;

    server_name         <your_server_name>;

    ssl_certificate     /path/to/ssl_cert;
    ssl_certificate_key /path/to/ssl_cert_key;

    location / {
        proxy_set_header    Host                $host;
        proxy_set_header    X-Real-IP           $remote_addr;
        proxy_set_header    X-Forwarded-For     $proxy_add_x_forwarded_for;
        proxy_set_header    X-Forwarded-Proto   $scheme;
        proxy_http_version  1.1;
        proxy_set_header    Upgrade             $http_upgrade;
        proxy_set_header    Connection          $connection_upgrade;
        proxy_pass          http://127.0.0.1:9000/;
    }
}
```


### Reference:

##### [Nginx UI](https://github.com/0xJacky/nginx-ui#nginx-ui)
