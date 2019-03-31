https://github.com/mikegcoleman/docker101/blob/master/Docker_eBook_Jan_2017.pdf


Docker for Mac Commands for Getting Into The Local Docker VM
https://www.bretfisher.com/docker-for-mac-commands-for-getting-into-local-docker-vm/


Package Management Basics: apt, yum, dnf, pkg
https://www.digitalocean.com/community/tutorials/package-management-basics-apt-yum-dnf-pkg


Format command and log output
https://docs.docker.com/config/formatting/


DNS
https://howdns.works/
https://dyn.com/blog/dns-why-its-important-how-it-works/

Docker Networks: DNS
* Containers shouldn't rely on IP's for inter-communication
* DNS for friendly names is built-in if you use custom networks
* There create custom networks rather than the link command 


Install 'ps aux' capability on linux
``` bash
apt-get update
apt-get install -y procps
```

Install 'ping' capability on linx
``` bash
apt-get update
apt-get install iputils-ping
```

Alphine use apk for package management

Docker networks
-p 8080:80 -> means open port 8080 on host route traffic to open port 80 on container

However on your host you cannot have multiple containers listening on the same port

## **Image**  
App binaries and dependencies
Metadata about the image data and how to run the image
Not a complete OS, No kernel, kernel modules (e.g drivers)

https://github.com/moby/moby/blob/master/image/spec/v1.md

https://github.com/docker-library/official-images/tree/master/library

https://docs.docker.com/storage/storagedriver/


## **Docker file basic**
Sequence is important

FROM -> file should start with from - usually the basic os
ENV -> variables used in file
RUN -> exec shell commands, use "&&" such that they all fit in one layer
EXPOSE -> by default no ports are exposed to vn. Exposing them only open ports in the container, therefore, we still need to run -p commands to route localhost interface host to the ones we have exposed here
CMD -> required param - it is the final command that will be run everything we run a new container from the image 
WORKDIR ->change directory - using workdir is preferred to using 'RUN cd /some/path'
COPY -> copy command

## **Container lifetime & persistence**
https://www.oreilly.com/ideas/an-introduction-to-immutable-infrastructure

https://12factor.net/

https://medium.com/@kelseyhightower/12-fractured-apps-1080c73d481c

https://docs.docker.com/storage/

Persistent data - 2 ways in docker
1) Volumes - make special location outside of container UFS
2) Bind Mounts - link container path to host path

Data Volumes
* need manual deletion 

running inspect will show
"Mounts" -> 
``` text
...
 "Mounts": [
            {
                "Type": "volume",
                "Name": "673e303f9b0badb966a91ae1f6e44c7b325b9954f2ce3f547e7171a3317f30f8",
                "Source": "/var/lib/docker/volumes/673e303f9b0badb966a91ae1f6e44c7b325b9954f2ce3f547e7171a3317f30f8/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
...
```
"Volume" -> 
``` text
...
 "Volumes": {
                "/var/lib/mysql": {}
            },
...
```

Bind Mounting
 Can't use in Dockerfile, must be at container run

___


## **Docker Compose and The docker-compose.yml file**

Comprised of 2 separate but related things
1) YAML-formatted file that describes our solution options for:
* containers
* networks
* volumes

2) A CLI tool docker-compose used for local dev/test automation with those YAML files

___

YAML file
See ../compose-sample-1/template.yml
https://yaml.org/start.html
https://yaml.org/refcard.html
https://docs.docker.com/compose/compose-file/compose-versioning/
https://github.com/docker/compose/releases


CLI tool
``` bash
docker-compose up  # setup volumes/networks and start all containers
docker-compose down # stop all containers and remove cont/vol/net
```
___





