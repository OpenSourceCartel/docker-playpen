Vagrant.configure("2") do |config|

    # Global provisioning command copy your ssh key into all vms
    config.vm.provision "file", source: "~/.ssh", destination: "/tmp"

    # Setup loadbalancer/reverse proxy for containers
    config.vm.define :lb do |loadbalancer|
        loadbalancer.vm.define "loadbalancer"
        loadbalancer.vm.hostname = "loadbalancer"
        loadbalancer.vm.box = "minimal/trusty64"
        loadbalancer.vm.box_check_update = true
        loadbalancer.vm.network "private_network", ip:"192.168.33.10"
        loadbalancer.vm.network "public_network", bridge: "eth0"
        loadbalancer.vm.synced_folder "~/Sites", "/sites"

        # Configure VBox settings
        loadbalancer.vm.provider :virtualbox do |vb|
            vb.name = "loadbalancer"
            vb.gui = false
            vb.memory = 1024
            vb.cpus = 1
            vb.customize ["modifyvm", :id, "--cpuexecutioncap", "75"]
        end

        # Run some provisions on the vm
        loadbalancer.vm.provision "shell", inline: <<-SHELL
            sudo apt-get update
            sudo apt-get install -y linux-image-extra-$(uname -r) apt-transport-https ca-certificates build-essential nginx htop --fix-missing
            cp -Rf /tmp/.ssh/ /root/
        SHELL
    end

    # Setup our docker host
    config.vm.define :ds do |docker_srv|

        docker_srv.vm.provision "docker"
        docker_srv.vm.define "dockerhost"
        docker_srv.vm.hostname = "dockerhost"
        docker_srv.vm.box = "ubuntu/trusty64"
        docker_srv.vm.box_check_update = true
        docker_srv.vm.network "public_network", bridge: "eth0"
        docker_srv.vm.network "private_network", ip: "192.168.33.20"
        docker_srv.vm.synced_folder "~/Sites", "/sites"
        docker_srv.vm.provision "file", source: "~/.ssh", destination: "/tmp/"
        docker_srv.vm.post_up_message = "Welcome to Docker PlayPen"
        docker_srv.vm.network "forwarded_port", guest: 8080, host: 7666

        # Configure VBox settings
        docker_srv.vm.provider :virtualbox do |vb|
            vb.name = "dockerhost"
            vb.gui = false
            vb.memory = 2576
            vb.cpus = 2
            vb.customize ["modifyvm", :id, "--cpuexecutioncap", "75"]
        end

        # Provision our docker containers
        docker_srv.vm.define "ubuntuSrvBase" do |a|
            a.vm.provider "docker" do |d|
                d.vagrant_machine = "dockerhost"
                d.vagrant_vagrantfile = "../ubuntuSrvBase/Dockerfile"
            end
        end

        # Run some provisions on the vm
        docker_srv.vm.provision "shell", inline: <<-SHELL
            cp -Rf /tmp/.ssh/ /root/
        SHELL
    end

    # Setup our MySql server
    config.vm.define :db do |db_srv|
        db_srv.vm.define "mysqlhost"
        db_srv.vm.hostname = "mysqlhost"
        db_srv.vm.box = "minimal/trusty64"
        db_srv.vm.box_check_update = true
        db_srv.vm.network "private_network", ip:"192.168.33.20"
        db_srv.vm.network "public_network", bridge: "eth0"
        db_srv.vm.synced_folder "~/Sites", "/sites"
        db_srv.vm.provision "file", source: "~/.ssh", destination: "/root/"

        # Configure VBox settings
        db_srv.vm.provider :virtualbox do |vb|
            vb.name = "mysqlhost"
            vb.gui = false
            vb.memory = 1024
            vb.cpus = 1
            vb.customize ["modifyvm", :id, "--cpuexecutioncap", "75"]
        end

        # Run some provisions on the vm
        db_srv.vm.provision "shell", inline: <<-SHELL
            debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password password root'
            debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password_again password root'
            sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
            sudo apt-get update
            sudo apt-get install -y linux-image-extra-$(uname -r) apt-transport-https ca-certificates build-essential mysql-server htop language-pack-en-base
            cp -Rf /tmp/.ssh/ /root/
        SHELL
    end
end
