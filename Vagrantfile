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
  
  # create a bridged interface adapter, this will make your vm look like another physical host on your network, should get your ip from DHCP
  Vagrant.configure("2") do |config|
    config.vm.network "public_network", bridge: "en1"
  end

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "~/Sites", "/Sites"

   config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
     vb.gui = false
   end
   
   # set to 4Gb of ram
   config.vm.customize ["modifyvm", :id, "--memory", 4096]
   
  # set number of cpus to 2 
  config.vm.customize ["modifyvm", :id, "--cpus", 2]

  # uncomment to set the number of cpus to half of what you have avalible
  #config.vm.customize ["modifyvm", :id, "--cpus", `awk "/^processor/ {++n} END {print n}" /proc/cpuinfo 2> /dev/null || sh -c 'sysctl hw.logicalcpu 2> /dev/null || echo ": 2"' | awk \'{print \$2}\' `.chomp ]

  # fire the build commands for our new guest
   config.vm.provision "shell", inline: <<-SHELL
     debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password password root'
     debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password_again password root'
     sudo apt-get update
     sudo apt-get install -y apt-transport-https ca-certificates
     sudo apt-get update
     sudo apt-get install -y build-essential nginx mysql-server htop language-pack-en-base git curl wget apparmor
     sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
     sudo touch /etc/apt/sources.list.d/docker.list
     sudo echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" >> /etc/apt/sources.list.d/docker.list
     sudo apt-get update
     sudo apt-get purge lxc-docker
     sudo apt-cache policy docker-engine
     sudo apt-get install linux-image-extra-$(uname -r)
     sudo apt-get update
     sudo apt-get install docker-engine
     sudo service docker start
   SHELL
end
