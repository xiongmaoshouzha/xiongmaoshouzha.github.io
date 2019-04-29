# xiongmaoshouzha.github.io
my technology blog

using [ink](https://github.com/InkProject/ink) build source. 

``` sh
# /bin/bash

sudo rm -rf blog/public/*
./ink build
cd blog
docker kill myblog
docker rm myblog
docker rmi -f ink:v1.0
docker build -t ink:v1.0 .
docker run -d -p 80:80 -p 443:443  --name myblog ink:v1.0
```
dockerfile:
```sh
FROM nginx
WORKDIR /usr/share/nginx/html
RUN mkdir -p /etc/nginx/cert
COPY cert* /etc/nginx/cert/
COPY public/ .
COPY nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
EXPOSE 443
```

nginx config:
```
user  nginx;
worker_processes  2;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  512;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;
    server_tokens off;
    #gzip  on;
    server {
        listen 80;  
        server_name www.igtx.top;      
        rewrite ^/(.*) https://www.igtx.top/$1 permanent;
		server_tokens off;		
    }
    server {
        listen 443 ssl http2;
        server_name www.igtx.top;
        ssl on;

        root /usr/share/nginx/html;
        access_log /dev/stdout;
        error_log /dev/stderr;
        index index.xml index.html index.htm;
		add_header X-Frame-Options SAMEORIGIN;
      

        ssl_certificate   cert/214688181830451.pem;
        ssl_certificate_key  cert/214688181830451.key;
        ssl_session_timeout 5m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
    }

}
```
