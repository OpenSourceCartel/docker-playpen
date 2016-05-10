Vagrant.configure("2") do |config|

    config.vm.define :lb do |loadbalancer|
        loadbalancer.vm.define "loadbalancer"
        loadbalancer.vm.hostname = "loadbalancer"
        loadbalancer.vm.box = "ubuntu/trusty64"
        loadbalancer.vm.box_check_update = true
        loadbalancer.vm.network "private_network", ip:"192.168.33.10"
        loadbalancer.vm.network "public_network", bridge: "eth0"
        loadbalancer.vm.synced_folder "~/Sites", "/sites"
        loadbalancer.vm.provision "file", source: "~/.ssh", destination: "/root/"

        loadbalancer.vm.provider :virtualbox do |vb|
            vb.name = "loadbalancer"
            vb.gui = false
            vb.memory = 1024
            vb.cpus = 1
            vb.customize ["modifyvm", :id, "--cpuexecutioncap", "75"]
        end
    end

    config.vm.define :ds do |docker_srv|

        docker_srv.vm.provision "docker"
        docker_srv.vm.define "dockerhost"
        docker_srv.vm.hostname = "dockerhost"
        docker_srv.vm.box = "ubuntu/trusty64"
        docker_srv.vm.box_check_update = true
        docker_srv.vm.network "public_network", bridge: "eth0"
        docker_srv.vm.network "private_network", ip: "192.168.33.20"
        docker_srv.vm.synced_folder "~/Sites", "/sites"
        docker_srv.vm.provision "file", source: "~/.ssh", destination: "/root/"
        docker_srv.vm.post_up_message = "Welcome to Docker PlayPen"
        docker_srv.vm.network "forwarded_port", guest: 8080, host: 7666

        docker_srv.vm.provider :virtualbox do |vb|
            vb.name = "dockerhost"
            vb.gui = false
            vb.memory = 2576
            vb.cpus = 2
            vb.customize ["modifyvm", :id, "--cpuexecutioncap", "75"]
        end

        docker_srv.vm.define "ubuntuSrvBase" do |a|
            a.vm.provider "docker" do |d|
                d.vagrant_machine = "dockerhost"
                d.vagrant_vagrantfile = "./ubuntuSrvBase/Dockerfile"
            end
        end
    end

  config.vm.define :db do |db_srv|
        db_srv.vm.define "mysqlhost"
        db_srv.vm.hostname = "mysqlhost"
        db_srv.vm.box = "ubuntu/trusty64"
        db_srv.vm.box_check_update = true
        db_srv.vm.network "private_network", ip:"192.168.33.20"
        db_srv.vm.network "public_network", bridge: "eth0"
        db_srv.vm.synced_folder "~/Sites", "/sites"
        db_srv.vm.provision "file", source: "~/.ssh", destination: "/root/"

        db_srv.vm.provider :virtualbox do |vb|
            vb.name = "mysqlhost"
            vb.gui = false
            vb.memory = 1024
            vb.cpus = 1
            vb.customize ["modifyvm", :id, "--cpuexecutioncap", "75"]
        end
  end

end
