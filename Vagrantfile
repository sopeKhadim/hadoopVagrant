# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Vagrantfile for the "Introduction to Big Data" course (IPSL), adapted from
# https://github.com/sopeKhadim/hadoopVagrant (box SopeKhadim/hadoopVM 2.0:
# CentOS 7, Hadoop 3.2.1, Hive 3.1.0, Spark 2.4.5, MySQL 5.7, Sqoop 1.4.7).
#
# Differences from the original Vagrantfile:
#  - private host <-> VM network instead of a bridge on the author's Wi-Fi interface
#    ("wlo1 Wi-Fi (Mon wifi)" does not exist on any other machine);
#  - shared folder relative to the project (./partage), created automatically,
#    instead of /home/user/vagrantShareFolder;
#  - 6 vCPUs and 6 GB of RAM (lower to 2 and 4096 if the host only has 8 GB).
#
# Usage: put this file in place of the Vagrantfile of the cloned repo,
# delete the repo's .vagrant folder, then run "vagrant up".

Vagrant.configure("2") do |config|
  config.vm.box = "SopeKhadim/hadoopVM"
  # If the box was added from a local file (USB key), comment out the next line:
  # a local box without metadata has version "0".
  config.vm.box_version = "2.0"

  # Private network: the VM is reachable from the host at 192.168.56.10
  # (range allowed by default by VirtualBox for host-only networks).
  config.vm.network "private_network", ip: "192.168.56.10"

  # Hadoop web interfaces, also reachable via localhost.
  config.vm.network "forwarded_port", guest: 9870, host: 9870, auto_correct: true  # HDFS NameNode
  config.vm.network "forwarded_port", guest: 8088, host: 8088, auto_correct: true  # YARN ResourceManager

  # Host <-> VM exchange folder (created if it does not exist).
  config.vm.synced_folder "./partage", "/home/vagrant/shareFolder", create: true

  config.vm.provider "virtualbox" do |vb|
    vb.name = "hadoop-ipsl"
    vb.cpus = 6
    vb.memory = 6144   # lower to 4096 if the computer only has 8 GB of RAM
  end
end
