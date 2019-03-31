# Container Lifetime & Persistent Data: Volumes, Volumes, Volumes

## Persistent Data: Data Volumes


``` bash
docker pull mysql
```

``` bash
docker image inspect mysql
```

``` bash
docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True mysql
```

``` bash
docker container ls
```

``` bash
docker container inspect mysql
```

``` bash
docker volume ls
```

``` bash
docker volume inspect TAB COMPLETION
```

``` bash
docker container run -d --name mysql2 -e MYSQL_ALLOW_EMPTY_PASSWORD=True mysql
```

``` bash
docker volume ls
```

``` bash
docker container stop mysql
```

``` bash
docker container stop mysql2
```

``` bash
docker container ls
```

``` bash
docker container ls -a
```

``` bash
docker volume ls
```

``` bash
docker container rm mysql mysql2
```

Show volumes are here even though we deleted the containers
``` bash
docker volume ls
```

-v mysql-db: -> is the name of the volume to be created with this container
``` bash
docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql
```

``` bash
docker volume ls
```
Returns below:
``` text
workspace@workspace-VirtualBox:~/udemy_Docker/Workspace/tutorials$ docker volume ls
DRIVER              VOLUME NAME
local               673e303f9b0badb966a91ae1f6e44c7b325b9954f2ce3f547e7171a3317f30f8
local               fd5c161acd68a3b1a1630bafa0fb805761db5a8ed55940c6ccef7edc33ea2fd1
local               mysql-db

```

``` bash
docker volume inspect mysql-db
```

Even after deleting the container the volume mysql-db is still there
``` bash
docker container rm -f mysql
```

Running new container that points to the same volume as the previous container
``` bash
docker container run -d --name mysql3 -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql
```

``` bash
docker volume ls
```

``` bash
docker container inspect mysql3
```

``` bash
docker volume create --help
```

## Persistent Data: Bind Mounting

cd dockerfile-sample-2

pcat Dockerfile

Linking local directory ${pwd} to container directory
``` bash
docker container run -d --name nginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx
```

``` bash
docker container run -d --name nginx2 -p 8080:80 nginx
```

``` bash
docker container exec -it nginx bash
```

## Assignment Answers: Edit Code Running In Containers With Bind Mounts

``` bash
docker run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve
```
