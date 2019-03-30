# Creating and Using Containers Like a Boss

## Check Our Docker Install and Config
``` bash
docker version

docker info

docker

docker container run

docker run
```

## Starting a Nginx Web Server
run nginx container - if image not found - will go download it from repository (default - dockerHub)
``` bash
docker container run --publish 80:80 nginx
```

--detach -> means that nginx will run in the backround
It will then output the container uniqueid after the command is run / container started
``` bash
docker container run --publish 80:80 --detach nginx
```

``` bash
docker container ls
```

Can type the first 3 or so digits - as long as it it unique
``` bash
docker container stop 690
```

docker container ls

Returns multiple lines - because it list all the containers that were ran.
Names - are unique - randomly generated from open source list of adjective and surnames of notable hacker or scientist
``` bash
docker container ls -a
```

-- name -> used to specify name of container, in this case it's webhost
``` bash
docker container run --publish 80:80 --detach --name webhost nginx
```

docker container ls -a

Since the container is running in the background now - we run the following command to view the logs
``` bash
docker container logs webhost
```

``` bash
docker container top
```

It shows the processes running in the container
``` bash
docker container top webhost
```

``` bash
docker container --help
```

docker container ls -a

Command to remove containers - append uniqueid to the command - here we are removing 3 containers
However, it will not remove running containers - safety measure
``` bash
docker container rm 63f 690 ode
```

docker container ls


Command to remove containers by force - even if it is running
``` bash
docker container rm -f 63f
```

docker container ls -a

___

## Container VS. VM: It's Just a Process
-d means we are running it in the background
``` bash
docker run --name mongo -d mongo
```

List the container running - similar to docker container ls
``` bash
docker ps  -> old way
docker container ls -> new way
```

``` bash
docker top mongo

> run 'ps aux' linux command - we see the process running on the host not in vm - however it has it's process id for security reasons
> run 'ps aux | grep mongo - to search mongo process
```

``` bash
docker stop mongo
```

docker ps

docker top mongo

``` bash
docker start mongo
```

docker ps

docker top mongo

___

## Assignment Answers: Manage Multiple Containers

docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql

docker container logs db

docker container run -d --name webserver -p 8080:80 httpd

docker ps

docker container run -d --name proxy -p 80:80 nginx

docker ps

docker container ls

Takes the name or uniqueid as parameter
``` bash
docker container stop TAB COMPLETION
```

docker ps -a

docker container ls -a

docker container rm

docker ps -a


``` bash
docker image ls
```
___

## What's Going On In Containers: CLI Process Monitoring

docker container run -d --name nginx nginx

docker container run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=true mysql

docker container ls

docker container top mysql

docker container top nginx

Get back json array on how the container was started
``` bash
docker container inspect mysql
```

docker container stats --help

Returns live stats of running container
``` bash
docker container stats
```

docker container ls

___

## Getting a Shell Inside Containers: No Need for SSH
Start new container interactively (i and t are sperate commands)
-t -> pseudo=tty - simulates a real terminal like ssh does
-i -> keep session open to receive terminal command
bash -> is additinal command - passing bash as argument
``` bash
docker container run -it --name proxy nginx bash
```
Enter exit to exit container - however it will stop the container - this is because it exited the command parsed in - in this case bash command

docker container ls

docker container ls -a

``` bash
docker container run -it --name ubuntu ubuntu
```

docker container ls

docker container ls -a

docker container start --help

-ai -> Starting existing container in interactive mode
``` bash
docker container start -ai ubuntu
```

docker container exec --help

exec -it -> run additional command in existing container
``` bash
docker container exec -it mysql bash
```

docker container ls

pull -> will pull latest image from repository
``` java
docker pull alpine
```

docker image ls

Starting an alpine container in interactive mode by passing bash command
``` bash
docker container run -it alpine bash
```
Returns an error because image/container does not have "bash" installed
``` text
docker: Error response from daemon: OCI runtime create failed: container_linux.go:345: starting container process caused "exec: \"bash\": executable file not found in $PATH": unknown.
```

However, the container/image has "sh"
``` bash
docker container run -it alpine sh
```

___

## Docker Networks: Concepts for Private and Public Comms in Containers

docker container run -p 80:80 --name webhost -d nginx

Show the ports being used and forwarded
``` bash
docker container port webhost
```

Show the ipaddress of the container - different from host
--format -> used here instead of grep (similar) as cleaner and more consistent
``` bash
docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost
```
___

## Docker Networks: CLI Management of Virtual Networks
Returns list of virtual Networks
bridge/docker0 -> default network that bridges through the NAT firewall
host -> it skips virtual network and attach container to host interface - prevent security bondaries but gain in performance
none -> removes eth0 and only leaves you with localhost inteface in container
``` bash
docker network ls
```


``` bash
docker network inspect bridge
```
Returns below - see webhost container attached 
``` text
...
 "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "05842fd421cd06952bc96df8907f9a8da355c3907f13dd4332c97234453b8d60": {
                "Name": "webhost",
                "EndpointID": "e7441fe75e9b746d7872d3587c64f7e20329cd13d35bfba6539c3eee5c2ebd1c",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
....
```

docker network ls

create - spawns a new virtual network for you to attach containers to
Hovwever it create vn based on the bridge drvier by default
``` bash
docker network create my_app_net
```

docker network ls

docker network create --help

Running new container in new virtual network setup above
``` bash
docker container run -d --name new_nginx --network my_app_net nginx
```

docker network inspect my_app_net

docker network --help

Connect container to the new network
arg1 - point to vn
arg2 - point to container
``` bash
docker network connect TAB_COMPLETION TAB_COMPLETION
```

Note: container will now be listening on two networks 
``` bash
docker container inspect TAB COMPLETION
```
``` text
...
"MacAddress": "02:42:ac:11:00:02",
    "Networks": {
        "bridge": {
            "IPAMConfig": null,
            "Links": null,
            "Aliases": null,
            "NetworkID": "835e50e2f416264e6d9ccb8895d7c6b1582b35c5550d7a643adffff0ac6958e3",
            "EndpointID": "e7441fe75e9b746d7872d3587c64f7e20329cd13d35bfba6539c3eee5c2ebd1c",
            "Gateway": "172.17.0.1",
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "MacAddress": "02:42:ac:11:00:02",
            "DriverOpts": null
        },
        "my_app_net": {
            "IPAMConfig": {},
            "Links": null,
            "Aliases": [
                "05842fd421cd"
            ],
            "NetworkID": "9b3cc637336891394467d7a78ebb9ba997d7952d8062116088814c937270b32c",
            "EndpointID": "af4451b81c6cd6484b9bd78e964ac63759fdecfd39673f81c8ef20beb70cf060",
            "Gateway": "172.18.0.1",
            "IPAddress": "172.18.0.3",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "MacAddress": "02:42:ac:12:00:03",
            "DriverOpts": null
        }
    }
...
```


Disconnect container from vn
``` bash
docker container disconnect TAB COMPLETION
```

docker container inspect

___

## Docker Networks: DNS and How Containers Find Each Other
Docker DNS - docker daemon has built-in DNS server that containers use by default
docker container ls
DNS default names - docker defaults the hostname to the container's name but you can also set alias

docker network inspect TAB COMPLETION

docker container run -d --name my_nginx --network my_app_net nginx

docker container inspect TAB COMPLETION

docker container exec -it my_nginx ping new_nginx

docker container exec -it new_nginx ping my_nginx

docker network ls

docker container create --help

___

## Assignment Answers: Using Containers for CLI Testing
--rm -> Docker will automatically clean up the container and remove the file system when the container exits
``` docker
docker container run --rm -it centos:7 bash
```

docker ps -a

docker container run --rm -it ubuntu:14.04 bash

docker ps -a

___

## Assignment Answers: DNS Round Robin Testing

Creates a new virtual network based on default bridge network settings
``` bash
docker network create dude
```

--net and --network (alis) both work
``` bash
docker container run -d --net dude --net-alias search elasticsearch:2
```

docker container ls 

Run container, run command "nslookup search", finaly exits and cleanup
``` bash
docker container run --rm --net dude alpine nslookup search
```

``` bash
docker container run --rm --net dude centos curl -s search:9200
```

docker container ls

docker container rm -f TAB COMPLETION
