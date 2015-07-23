# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

box = "phusion/ubuntu-14.04-amd64"
box_url = "https://atlas.hashicorp.com/phusion/boxes/ubuntu-14.04-amd64/versions/2014.04.30/providers/virtualbox.box"

shared_path = "."

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = box
  config.vm.box_url = box_url

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network "forwarded_port", guest: 8080, host: 8900

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder shared_path, "/home/vagrant/kube/"

  # config.vm.synced_folder shared_path, "/home/vagrant/shared/", :mount_options => [ "dmode=777", "fmode=777" ]

  config.vm.provision "shell", inline: $SHELL
  # or use separated #bin/bash sh script by defining the file path
  # config.vm.provision :shell, path: "bootstrap.sh"

  config.vm.provider "virtualbox" do |vb|
    # Boot with headless mode or GUI mode
    vb.gui = false

    # Use VBoxManage to customize the VM. change memory:
    vb.customize ["modifyvm", :id, "--memory", "2048"]

    vb.destroy_unused_network_interfaces = true
  end
end

$SHELL = <<-CONTENTS

###
# Install required softwares and binaries
###

sed -i 's/# \(.*multiverse$\)/\1/g' /etc/apt/sources.list
apt-get -yqq update
apt-get install -y wget curl


###
# Instal Docker (https://docs.docker.com/installation/ubuntulinux/)
###

wget -qO- https://get.docker.com/ | sh
usermod -aG docker vagrant
service docker restart


###
# Install docker-compose (https://github.com/docker/compose/releases)
###

curl -L https://github.com/docker/compose/releases/download/1.4.0rc2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose


###
# Setup dockercfg auth key
###

if test -e /home/vagrant/kube/shared/dockercfg ; then
  cp -r /home/vagrant/kube/shared/dockercfg /home/vagrant/.dockercfg
  chown -R vagrant /home/vagrant/.dockercfg
fi


###
# Clean up: symlinked files
###

rm -rf /home/vagrant/.bashrc
rm -rf /home/vagrant/.gitconfig
rm -rf /home/vagrant/.scripts

ln -s /home/vagrant/kube/shared/boilerplate/root/.bashrc /home/vagrant/
ln -s /home/vagrant/kube/shared/boilerplate/root/.scripts /home/vagrant/
ln -s /home/vagrant/kube/shared/boilerplate/root/.gitconfig /home/vagrant/

CONTENTS
