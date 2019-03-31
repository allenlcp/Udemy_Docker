# Container Images, Where To Find Them and How To Build Them

## The Mighty Hub: Using Docker Hub Registry Images

http://hub.docker.com


``` bash
docker image ls
```

``` bash
docker pull nginx
```

``` bash
docker pull nginx:1.11.9
```

``` bash
docker pull nginx:1.11
```

``` bash
docker pull nginx:1.11.9-alpine
```

``` bash
docker image ls
```

## Images and Their Layers: Discover the Image Cache

``` bash
docker image ls
```

``` bash
docker history nginx:latest
```

``` bash
docker history mysql
```

``` bash
docker image inspect nginx
```

## Image Tagging and Pushing to Docker Hub

``` bash
docker image tag -- help
```

``` bash
docker image ls
```

Pulling mysql-server image from the mysql organisation repo
``` bash
docker pull mysql/mysql-server
```

``` bash
docker image ls
```

``` bash
docker pull nginx:mainline
```

``` bash
docker image ls
```

``` bash
docker image tag nginx bretfisher/nginx
```

``` bash
docker image tag --help
```

``` bash
docker image ls
```

Returns - denied: requested access to the resource is denied
``` bash
docker image push bretfisher/nginx
```

``` bash
docker --help
```

Command to login
``` bash
docker login
```

Location where login config is stored
``` bash
cat .docker/config.json
```


``` bash
docker image push bretfisher/nginx
```

Create another tag for bretfisher/nginx
``` bash
docker image tag bretfisher/nginx bretfisher/nginx:testing
```
``` bash
mysql                latest              7bb2586065cd        4 days ago          477MB
httpd                latest              5eace252f2f2        4 days ago          132MB
allenlcp/nginx       latest              2bcb04bdb83f        4 days ago          109MB
allenlcp/nginx       testing             2bcb04bdb83f        4 days ago          109MB
nginx                1.15.10             2bcb04bdb83f        4 days ago          109MB
```

``` bash
docker image ls
```

``` bash
docker image push bretfisher/nginx:testing
```
Check whether layers already exist or not
``` bash
The push refers to repository [docker.io/allenlcp/nginx]
7e274c0effe8: Layer already exists 
dd0338cdfab3: Layer already exists 
5dacd731af1b: Layer already exists 
testing: digest: sha256:dabecc7dece2fff98fb00add2f0b525... size: 948
```

``` bash
docker image ls
```
___


## Building Images: The Dockerfile Basics

cd dockerfile-sample-1

vim Dockerfile

## Building Images: Running Docker Builds
Building Dockfile in the current directory
-t -> since we will only be using it locally, we don't need to prefix it with the user... therefore simply calling it customnginx
``` bash
docker image build -t customnginx .
```

``` bash
docker image ls
```

Make a change in the Docketfile
Run below command again and the build is super fast. Docker is re-using cached that has not changed.
Note: we need to put the least changing things at the start of the file otherwise, docker will rebuild everything after that things that have not been cached
``` bash
docker image build -t customnginx .
```

## Building Images: Extending Official Images

cd dockerfile-sample-2

vim Dockerfile

Note we did not need to specify EXPOSE or CMD - that's because we are inheriting from the nginx dockerfile defaults

``` bash
docker container run -p 80:80 --rm nginx
```

``` bash
docker image build -t nginx-with-html .
```

``` bash
docker container run -p 80:80 --rm nginx-with-html
```

``` bash
docker image ls
```

``` bash
docker image tag --help
```

``` bash
docker image tag nginx-with-html:latest bretfisher/nginx-with-html:latest
```

``` bash
docker image ls
```

``` bash
docker push
```

## Assignment Answers: Build Your Own Dockerfile and Run Containers From It

cd dockerfile-assignment-1

vim Dockerfile

``` bash
docker build cmd
```

``` bash
docker build -t testnode .
```

``` bash
docker container run --rm -p 80:3000 testnode
```

``` bash
docker images
```

``` bash
docker tag --help
```

``` bash
docker tag testnode bretfisher/testing-node
```

``` bash
docker push --help
```

``` bash
docker push bretfisher/testing-node
```

``` bash
docker image ls
```

``` bash
docker image rm bretfisher/testing-node
```

``` bash
docker container run --rm -p 80:3000 bretfisher/testing-node
```
