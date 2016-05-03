#Docker PlayPen

A ready to go docker developer environment.

Built with vagrant using the ubuntu/trusty64 14.04.4 LTS Server distro.

Comes pre-installed with;

###Nginx
Acts as a reverse proxy from your public network to docker containers

###Mysql
Database server for docker containers.
I don't agree with using mysql in a microservices enviroment, in a production enviroment your always best having a backed DB instance only accessible on an internal network ip from your web servers.

###Docker
docker-engine for running docker containers

###Docker-compose
A tool for defining and running multi-container Docker applications

The Vagrant file should automagically create a virtual box machine with 4 interfaces

1 public network bridged interface
 - creates a bridged interface, sharing your host box on your network as a physical machine
 - IP assigned by network DHCP
 - ensure your bridged adapter interface name is correct
 - accessible to anyone on your netork

1 private network interface
 - Creates a private network between host and guest
 - Only accessible from your host
 - Default IP:192.168.33.10
  
1 NAT interface
  - doesnt really provide much functionality but why not have one?

1 docker interface
  - used by docker engine to to create an interface shared between docker containers and your guest interfaces

##Mysql Details
MySQL server version 5.5.49

Credentials are specified in the Vagrantfile as followed

user:root
pass:root

##Installation Instructions
### Prerequisites 
  - Latest Virtual box
  - Latest vagrant

####To install first clone this repository

git clone https://github.com/OpenSourceCartel/docker-playpen.git

####Provision the vagrant host

vagrant up --provision
