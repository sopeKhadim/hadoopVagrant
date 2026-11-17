# Hadoop Vagrant Virtual Machine

* **Version** : 2.0 (box `SopeKhadim/hadoopVM`)
* **OS** : CentOS 7
* Last modified : 17/11/2026

## Description
Hadoop Vagrant Virtual Machine used in the course *Introduction to Big Data*.

## Components
This machine contains the following technologies :
* **Python** 2.7
* **Python** 3.6
* **Hadoop** 3.2.1
* **Spark** 2.4.5
* **Hive** 3.1.0
* **MySQL** 5.7.33
* **Sqoop** 1.4.7

## Requirements
* A 64-bit **x86** computer (Intel/AMD) with hardware virtualization (VT-x / AMD-V) enabled in the BIOS/UEFI.
  Apple Silicon Macs (M1/M2/M3…) cannot run this box.
* At least **8 GB of RAM** (16 GB recommended) and **~20 GB** of free disk space.
* The `Vagrantfile` gives the VM **6 CPUs and 6 GB of RAM**. On a smaller machine, lower
  `vb.cpus` and `vb.memory` (e.g. `2` and `4096`) before running `vagrant up`.

## 1. Install VirtualBox
Download and install VirtualBox for your system from the official page:
👉 https://www.virtualbox.org/wiki/Downloads

* **Windows** : run the `.exe` installer. If VirtualBox refuses to start the VM, disable
  *Hyper-V* / *Windows Hypervisor Platform* in "Turn Windows features on or off" and reboot.
* **Linux** : use the package for your distribution from the link above
  (or `sudo apt install virtualbox` on Ubuntu/Debian).
* **macOS (Intel)** : run the `.dmg` and allow the Oracle extension in
  *System Settings → Privacy & Security*.

## 2. Install Vagrant
Official download page: 👉 https://developer.hashicorp.com/vagrant/install

**Windows** : download the *AMD64* `.msi` installer from the page above, run it, then **reboot**.

**Ubuntu / Debian** :
```bash
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vagrant
```

**macOS (Intel)** :
```bash
brew tap hashicorp/tap
brew install hashicorp/tap/hashicorp-vagrant
```

Check the installation:
```bash
vagrant --version
VBoxManage --version
```

## 3. Start the virtual machine
```bash
git clone https://github.com/sopeKhadim/hadoopVagrant.git
cd hadoopVagrant
rm -rf .vagrant          # remove the VM state left by the author's machine (first time only)
vagrant up               # downloads the box (several GB) the first time, then boots the VM
vagrant ssh              # connect to the VM as user "vagrant"
```

* ``vagrant`` can sudo as ``root`` and can perform any task in the virtual machine.
* Stop the VM with ``vagrant halt`` (data is kept), restart it with ``vagrant up``.
* After editing the `Vagrantfile`, apply the changes with ``vagrant reload``.
* ``vagrant destroy`` deletes the VM completely (HDFS data included).

**Offline installation (USB key)** : if the teacher gives you the box as a file:
```bash
vagrant box add SopeKhadim/hadoopVM /path/to/hadoopVM.box
```
then comment the line `config.vm.box_version = "2.0"` in the `Vagrantfile` before `vagrant up`.

### Network
The `Vagrantfile` puts the VM on a private host-only network:

| Access | Address |
|---|---|
| VM from your computer | `192.168.56.10` (e.g. `ping 192.168.56.10`) |
| HDFS NameNode web UI | http://localhost:9870 or http://192.168.56.10:9870 |
| YARN ResourceManager web UI | http://localhost:8088 or http://192.168.56.10:8088 |

## 4. Share files between your computer and the VM
The `Vagrantfile` declares a synced folder:

```ruby
config.vm.synced_folder "./partage", "/home/vagrant/shareFolder", create: true
```

| On your computer (host) | Inside the VM (guest) |
|---|---|
| `hadoopVagrant/partage/` (created automatically) | `/home/vagrant/shareFolder` |
| `hadoopVagrant/` (the project folder itself) | `/vagrant` (Vagrant default) |

Both sides see the same files in real time: a file dropped in `partage/` on your computer
appears immediately in `~/shareFolder` in the VM, and vice versa.

Example: send a dataset from your computer to HDFS
```bash
# On your computer
cp ~/Downloads/data.csv hadoopVagrant/partage/

# In the VM (vagrant ssh)
ls ~/shareFolder
hdfs dfs -mkdir -p /user/vagrant/data
hdfs dfs -put ~/shareFolder/data.csv /user/vagrant/data/

# And back: get a result from HDFS to your computer
hdfs dfs -get /user/vagrant/output/part-r-00000 ~/shareFolder/
```

To share another folder, add a line to the `Vagrantfile` (host path first, VM path second),
then run `vagrant reload`:
```ruby
config.vm.synced_folder "C:/Users/me/datasets", "/home/vagrant/datasets", create: true   # Windows
config.vm.synced_folder "~/datasets", "/home/vagrant/datasets", create: true             # Linux / macOS
```

For a one-off copy without a shared folder:
```bash
vagrant upload ./file.csv /home/vagrant/file.csv
```

If `vagrant up` fails with an error about `vboxsf` / *Guest Additions*, install the plugin
below, then run `vagrant reload`:
```bash
vagrant plugin install vagrant-vbguest
```

## Get Started
You can start/stop the hadoop services by the following commands :
* HDFS - ``start-dfs.sh / stop-dfs.sh``
* YARN - ``start-yarn.sh /stop-yarn.sh``
* All Hadoop services - ``start-all.sh``

You can run ``jps`` to see all the relevant processes running.
You can also run ``hdfs dfs -ls /user/vagrant`` to confirm HDFS is up and running.
You can connect to MySQL as root with the password : ``Pa$$w0rd``

The `scripts/` folder (`retail_db.sql`, `sqoop_script.sh`) is available in the VM under
`/vagrant/scripts`. MySQL runs inside the VM, so in the Sqoop commands use
`jdbc:mysql://localhost:3306/...` instead of the old IP address `192.168.185.226`.

## License
. None
