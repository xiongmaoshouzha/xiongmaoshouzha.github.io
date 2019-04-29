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
``
