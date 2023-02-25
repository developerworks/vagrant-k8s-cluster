Vagrant.configure("2") do |config|
    config.proxy.http     = "http://192.168.0.103:1087"
    config.proxy.https    = "http://192.168.0.103:1087"
    config.vm.box_check_update = false

    config.vm.provision "shell", inline: <<-SHELL
        sudo apt update -y
        echo "192.168.0.200  master" >> /etc/hosts
        echo "192.168.0.201  worker1" >> /etc/hosts
        echo "192.168.0.202  worker2" >> /etc/hosts
    SHELL
    
    config.vm.define "master" do |master|
        master.vm.box = "ubuntu/jammy64"
        master.vm.hostname = "master"
        master.vm.network "public_network", bridge: "enp8s0", ip: "192.168.0.200"
        master.vm.provider "virtualbox" do |vb|
            vb.memory = 4096
            vb.cpus = 2
        end
        master.vm.provision "shell", path: "scripts/common.sh"
        master.vm.provision "shell", path: "scripts/master.sh"
    end

    (1..2).each do |i|
        config.vm.define "worker#{i}" do |worker|
            worker.vm.box = "ubuntu/jammy64"
            worker.vm.hostname = "worker#{i}"
            worker.vm.network "public_network", bridge: "enp8s0", ip: "192.168.0.20#{i}"
            worker.vm.provider "virtualbox" do |vb|
                vb.memory = 2048
                vb.cpus = 4
            end
            worker.vm.provision "shell", path: "scripts/common.sh"
            worker.vm.provision "shell", path: "scripts/worker.sh"
        end
    end
end