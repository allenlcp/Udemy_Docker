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