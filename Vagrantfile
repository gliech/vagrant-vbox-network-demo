# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.define "network-server" do |machine|
        machine.vm.hostname = "network-server"
        machine.vm.box = "debian/testing64"
        machine.vm.network "private_network",
            ip: "192.168.23.1",
            netmask: "24",
            virtualbox__intnet: "network1"
        machine.vm.provider "virtualbox" do |vbox|
            vbox.name = "network-server"
            vbox.cpus = 1
            vbox.memory = 512
        end
        machine.vm.provision "shell", path: "dhcp/script"
        machine.vm.provision "shell", path: "dns"
        machine.vm.provision "shell", path: "routes/static-network1"
    end

    config.vm.define "web-server" do |machine|
        machine.vm.hostname = "web-server"
        machine.vm.box = "debian/testing64"
        machine.vm.network "private_network",
            ip: "192.168.23.2",
            netmask: "24",
            virtualbox__intnet: "network1"
        machine.vm.provider "virtualbox" do |vbox|
            vbox.name = "web-server"
            vbox.cpus = 1
            vbox.memory = 512
        end
        machine.vm.provision "ansible_local" do |ansible|
            ansible.playbook = "web/playbook.yml"
            ansible.verbose = false
        end
        machine.vm.provision "shell", path: "routes/static-network1"
    end

    config.vm.define "linux-client" do |machine|
        machine.vm.hostname = "linux-client"
        machine.vm.box = "ubuntu/xenial64"
        machine.vm.network "private_network",
            type: "dhcp",
            virtualbox__intnet: "network1"
        machine.vm.provider "virtualbox" do |vbox|
            vbox.name = "linux-client"
            vbox.cpus = 2
            vbox.memory = 1024
            vbox.gui = true
        end
        machine.vm.synced_folder '.', '/vagrant', type: "rsync", disabled: false
#       machine.vm.provision "shell", path: "client/script"
    end

    config.vm.define "router-one" do |machine|
        machine.vm.hostname = "router-one"
        machine.vm.box = "debian/testing64"
        machine.vm.network "private_network",
            ip: "192.168.23.254",
            netmask: "24",
            virtualbox__intnet: "network1"
        machine.vm.network "private_network",
            ip: "10.13.24.1",
            netmask: "24",
            virtualbox__intnet: "network2"
        machine.vm.provider "virtualbox" do |vbox|
            vbox.name = "router-one"
            vbox.cpus = 1
            vbox.memory = 256
        end
        machine.vm.provision "shell", path: "router"
        machine.vm.provision "shell", path: "routes/static-router1"
    end

    config.vm.define "router-two" do |machine|
        machine.vm.hostname = "router-two"
        machine.vm.box = "debian/testing64"
        machine.vm.network "private_network",
            ip: "10.13.24.2",
            netmask: "24",
            virtualbox__intnet: "network2"
        machine.vm.network "private_network",
            ip: "172.18.5.254",
            netmask: "24",
            virtualbox__intnet: "network3"
        machine.vm.network "private_network",
            ip: "192.168.17.254",
            netmask: "24",
            virtualbox__intnet: "network4"
        machine.vm.provider "virtualbox" do |vbox|
            vbox.name = "router-two"
            vbox.cpus = 1
            vbox.memory = 256
        end
        machine.vm.provision "shell", path: "router"
        machine.vm.provision "shell", path: "routes/static-router2"
        machine.vm.provision "shell", path: "dhcrelay/script"
    end

    config.vm.define "client-three" do |machine|
        machine.vm.hostname = "client-three"
        machine.vm.box = "generic/fedora27"
        machine.vm.network "private_network",
            type: "dhcp",
            virtualbox__intnet: "network3"
        machine.vm.provider "virtualbox" do |vbox|
            vbox.name = "client-three"
            vbox.cpus = 2
            vbox.memory = 1024
            vbox.gui = true
        end
        machine.vm.synced_folder '.', '/vagrant', type: "rsync", disabled: false
        machine.vm.provision "shell", path: "client/script"
    end

end
