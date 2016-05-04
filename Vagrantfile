# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = true
  
  # creates a bridged interface adapter, this will make your vm look like another physical host on your network
  # should get your ip from DHCP, very helpfull for sharing/testing over a local netowkr
  # very nicley if you host interface name isnt 'eth0' vahrant knows and gives you a prompt of avalible interfaces to bridge too
  # how clever
  config.vm.network "public_network", bridge: "eth0"
  
  # creates a private network interface accessible from your host to the gest vagrant box created 
  config.vm.network "private_network", ip: "192.168.33.10"
  
  # define our hostname
  config.vm.hostname = "docker-playpen"
  
  # side not you will also get a NAT interface created as well by vagrant, and also a docker interface from when we install docker later

  # Share an additional folder to the guest VM. First argument is the host folder the second is the guest path to be mounted
  config.vm.synced_folder "~/Sites", "/Sites"
  
  # configure the guest settings
  config.vm.provider "virtualbox" do |vb|
      # Set the name
      vb.name = "docker-playpen"
    
      # Display the VirtualBox GUI when booting the machine
      vb.gui = false
     
      # set to 4Gb of ram
      vb.memory = 4096
     
      # set number of cpus to 2
      vb.cpus = 2
     
      # set execution cap
      vb.customize ["modifyvm", :id, "--cpuexecutioncap", "75"]
    end
   
   # copy ssh keys to our vm tmp, cp them in the inline commands
   config.vm.provision "file", source: "~/.ssh", destination: "/tmp/.ssh"
   
   # set a welcome banner when logging in to the vagrant host
   config.vm.post_up_message = "Welcome to Docker PlayPen"
   
  # fire the build commands for our new guest
   config.vm.provision "shell", inline: <<-SHELL
     debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password password root'
     debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password_again password root'
     sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
     sudo touch /etc/apt/sources.list.d/docker.list
     sudo echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" >> /etc/apt/sources.list.d/docker.list
     sudo apt-get purge lxc-docker
     sudo apt-get update
     sudo apt-cache policy docker-engine
     sudo apt-get install -y linux-image-extra-$(uname -r) apt-transport-https ca-certificates build-essential nginx mysql-server htop language-pack-en-base git curl wget apparmor docker-engine python-pip
     sudo pip install docker-compose
     sudo cp -Rf /tmp/.ssh /root/.ssh
   SHELL
end
