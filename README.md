# Hadoop Vagrant Virtual Machine
* **Version** : 1.0
* **OS** : Centos 7
* Last modified : 26/03/2021 
## Description
This Hadoop Vagrant Virtual Machine ...
..
..  

## Components
This machine contains the following technologies :
* **Python** 2.7
* **Python** 3.6
* **Hadoop** 3.2.1
* **Spark** 2.4.5
* **Hive** 3.1.0
* **MySQL** 5.7.33 

## Installation
* Install **Virtual box** from https://www.virtualbox.org/
* Install **Vagrant** setup from https://www.vagrantup.com/
* Clone this repo to your local machine
* Move to the folder where the Vagrantfile is located.
* Run ``vagrant up`` to bring up the Virtual Machine.
* Connect to your Virtual Machine by saying ``vagrant ssh``.
* Login as user vagrant.
* ``vagrant`` can sudo as ``root`` and can perform any task in the virtual machine.
* Stop Vagrant VM with ``vagrant halt``

## Get Started
You can start/stop the hadoop services by the following commands : 
* HDFS - ``start-dfs.sh / stop-dfs.sh``
* YARN - ``start-yarn.sh /stop-yarn.sh``  
* All Hadoop services - ``start-all.sh``  
You can run ``jps`` to see all the relevant processes running.  
You can also run ``hdfs dfs -ls /user/vagrant`` to confirm HDFS is up and running.  
You can connect to mysql with as root with the password : ``Pa$$w0rd``  

## License
. None

