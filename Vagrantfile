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
        machine.vm.provision "file", source: "dhcp/files", destination: "$HOME/dhcp"
        machine.vm.provision "shell", path: "dhcp/script"
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
        machine.vm.provision "shell", inline: <<-EOF
            dnf --assumeyes update
            dnf --assumeyes groups install "Fedora Workstation"
            systemctl enable gdm.service
            systemctl set-default graphical.target
            systemctl default
        EOF
    end

end
