# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.define "control" do |control|
    control.vm.box = "bento/ubuntu-16.04"
    control.vm.synced_folder "./repo", "/vagrant"
    control.vm.provision "file", source: "./private/id_rsa", destination: "/home/vagrant/.ssh/id_rsa"
    control.vm.provision "shell", inline: <<-SHELL
      chown vagrant:vagrant /home/vagrant/.ssh/id_rsa
      chmod 0600 /home/vagrant/.ssh/id_rsa
    SHELL

    control.vm.network "private_network", ip: "192.168.33.10"
  end

  config.vm.define "slave" do |slave|
    slave.vm.box = "bento/centos-7.2"
    slave.vm.provision "file", source: "./private/id_rsa.pub", destination: "/tmp/pubkey"
    slave.vm.provision "shell", inline: <<-SHELL
      cat /tmp/pubkey >> /home/vagrant/.ssh/authorized_keys
      chmod 0600 /home/vagrant/.ssh/authorized_keys
      rm /tmp/pubkey
    SHELL

    slave.vm.network "private_network", ip: "192.168.33.11"
  end
end
