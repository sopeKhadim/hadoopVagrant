# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "SopeKhadim/hadoopVM"
  config.vm.box_version = "2.0"
  #config.vm.network "forwarded_port", guest: 4040, host: 4040
  config.vm.network :public_network, :bridge =>  "wlo1 Wi-Fi (Mon wifi)"
  config.vm.synced_folder "/home/user/vagrantShareFolder", "/home/vagrant/shareFolder"
  
  config.vm.provider "virtualbox" do |vb|
    vb.cpus = "4" # you can increase you cpu to 4
    vb.memory = "6096"
  end
end
