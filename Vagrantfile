# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.define "network-server" do |machine|
        machine.vm.hostname = "network-server"
        machine.vm.box = "debian/testing64"
        machine.vm.network "private_network", ip: "192.168.23.1", virtualbox__intnet: "network1"
        machine.vm.provider "virtualbox" do |vbox|
            vbox.name = "network-server"
            vbox.cpus = 1
            vbox.memory = 512
        end
        machine.vm.provision "shell", path: "dhcp/script"
        machine.vm.provision "shell", path: "dns"
    end

    config.vm.define "linux-client" do |machine|
        machine.vm.hostname = "linux-client"
        machine.vm.box = "generic/fedora27"
        machine.vm.network "private_network", type: "dhcp", virtualbox__intnet: "network1"
        machine.vm.provider "virtualbox" do |vbox|
            vbox.name = "linux-client"
            vbox.cpus = 2
            vbox.memory = 1024
            vbox.gui = true
        end
        machine.vm.synced_folder '.', '/vagrant', type: "rsync", disabled: false
        machine.vm.provision "shell", path: "client/script"
    end

    config.vm.define "web-server" do |machine|
        machine.vm.hostname = "web-server"
        machine.vm.box = "debian/testing64"
        machine.vm.network "private_network", ip: "192.168.23.2", virtualbox__intnet: "network1"
        machine.vm.provider "virtualbox" do |vbox|
            vbox.name = "web-server"
            vbox.cpus = 1
            vbox.memory = 512
        end
        machine.vm.provision "ansible_local" do |ansible|
            ansible.playbook = "web/playbook.yml"
            ansible.verbose = false
        end
    end

end
