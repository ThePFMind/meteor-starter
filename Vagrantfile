# -*- mode: ruby -*-
# vi: set ft=ruby :

ROOT = File.dirname(File.expand_path(__FILE__))

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = '2'

$mongo_config = <<SCRIPT

# sudo rm -rf /var/lib/mongodb

sudo sed -i 's/dbpath=\\/var\\/lib\\/mongodb/dbpath=\\/data\\/db/' /etc/mongod.conf

sudo service mongod start

SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.env.enable
  config.vm.box = "thepfmind/meteor-base"

  config.vm.network "forwarded_port", guest: 3000, host: 3000
  config.vm.network "forwarded_port", guest: 27017, host: 27017

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.network "private_network", ip: "192.168.33.10"

  config.vm.synced_folder "./data", "/data/db",
    owner: "vagrant",
    mount_options: ["dmode=775,fmode=664"]

  config.vm.provision :shell, :inline => $mongo_config

  config.vm.provider :virtualbox do |vb|
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "2048", "--cpus", "2"]
  end

end
