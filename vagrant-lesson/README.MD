### Vagrant

* Vagrant is a popular tool used by development teams to quickly install and deploy one or several clusters of virtualized server environments on which to test and develop software and distributed systems.

* In the era of micro services, it is extremely important to be able to quickly setup and tear down a test environment that would closely mimic it’s cloud environment once a system is deployed in production.
* In this demo we will install and configure mysql server and ubuntu, and the steps on how to get it up and running quickly.

In this demo we will be doing: 
- virtualizing and automating the download, 
- setup of ubuntu where mysql server will be running 
- installation of the necessary dependencies, configurations required for mysql server to operate properly.

- Ubuntu is a linux based operating system and is one of the most popular among the open source operating systems.
- For vagrant to work, we also need a virtualization provider, for this we are going to use Oracle’s VirtualBox.
- We also need a GUI Admin interface to manage and configure mysql databases and its users. For this we are going to use the web based phpmyadmin, as well as the corresponding web tools apache2, and php7.

#### Install vagrant (on ubuntu linux)

```bash
sudo apt-get install virtualbox
sudo apt-get install vagrant
```
#### Configure Vagrantfile

Open Vagrantfile using your favorite editor. The initial content of the file will look like,

```bash
Vagrant.configure("2") do |config|
  config.vm.box = "base"
end
```
config.vm.box specifies the type of operating system that will be used as guest operating system running in virtualbox.
Now change “base” to “xenial64”, this will install ubuntu16.04 the 64bit version. The OS will be downloaded from Vagrant’s cloud OS box site. Also add line of codes indicated below.
```bash
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.define "db-server" do |db|
    db.vm.network "public_network", :bridge => "eth0", ip: "192.168.1.237"
    db.vm.network "forwarded_port", guest: 3306, host: 3306
    db.vm.network "forwarded_port", guest: 80, host: 8306
    db.vm.provision "shell", path: "bootstrap.sh"
  end
end
```
What each line is about,

config.vm.define, allows us to define a named virtual environment. We can define several virtual environments in a single Vagrantfile, as long as we provide a unique name for each. In this particular example, we named our environment to “db-server”.

db.vm.network “public_network”, defines the network interface to use. In this case we are using the host’s dhcp provider (as specified by “public_network”) to assign the ip via a bridge, using the shown IP. Note: the IP value may be changed to suit your network environment. Also make sure the IP is configured in the local network provider/router to lease the IP semi-permanently to this virtual machine as this may change on your next restart of the guest machine, and may have to be reconfigured to a new one.

db.vm.network “forwarded_port”, this will allow us to open a listening port in the host and guest OS. The host OS has the actual connection to the outside world, so it forwards all received packets to the port we specify for the guest OS.

For mysql, we can set 3306 for the guest and host OS.
For the web application phpmyadmin, guest is set to 80, for the host we set to 8306. NOTE: vagrant will complain about port 80 being a well known port and can not be used. Choose any number higher than 1024 and you should be ok.

db.vm.provision “shell”, this allows us to execute a shell script inside the guest OS right after it boots up.
bootstrap.sh, This is where we add additional commands to install and configure the guest OS to run mysql server and phpmyadmin. bootstrap.sh may be renamed to anything you fancy.

#### Bootstrap.sh
```bash
#!/usr/bin/env bash 


DBHOST=localhost 
DBNAME=dbname 
DBUSER=dbuser 
DBPASSWD=userpass 

apt-get update 
apt-get install vim curl build-essential python-software-properties git 

debconf-set-selections <<< "mysql-server mysql-server/root_password password $DBPASSWD" 
debconf-set-selections <<< "mysql-server mysql-server/root_password_again password $DBPASSWD" 
debconf-set-selections <<< "phpmyadmin phpmyadmin/dbconfig-install boolean true" 
debconf-set-selections <<< "phpmyadmin phpmyadmin/app-password-confirm password $DBPASSWD"
debconf-set-selections <<< "phpmyadmin phpmyadmin/mysql/admin-pass password $DBPASSWD"
debconf-set-selections <<< "phpmyadmin phpmyadmin/mysql/app-pass password  $DBPASSWD" 
debconf-set-selections <<< "phpmyadmin phpmyadmin/reconfigure-webserver multiselect  none" 




# install mysql and admin interface 

apt-get -y install mysql-server phpmyadmin 


mysql -uroot -p$DBPASSWD -e "CREATE DATABASE $DBNAME" 
mysql -uroot -p$DBPASSWD -e "grant all privileges on $DBNAME.* to '$DBUSER'@'%' identified by $DBPASSWD" 

cd /vagrant 

# update mysql conf file to allow remote access to the db 

sudo sed -i "s/.*bind-address.*/bind-address = 0.0.0.0/" 
/etc/mysql/mysql.conf.d/mysqld.cnf 

sudo service mysql restart 

# setup phpmyadmin 

apt-get -y install php apache2 libapache2-mod-php php-curl php-gd php-mysql php-gettext a2enmod rewrite 

sed -i "s/AllowOverride None/AllowOverride All/g" 
/etc/apache2/apache2.conf 

rm -rf /var/www/html 
ln -fs /vagrant/public /var/www/html 

sed -i "s/error_reporting = .*/error_reporting = E_ALL/" 
/etc/php/7.0/apache2/php.ini 
sed -i "s/display_errors = .*/display_ errors = On/" 
/etc/php/7.0/apache2/php.ini 

service apache2 restart 
```
Boot up db-server

In step 2, We named the virtual machine “db-server”, we use this name to refer to the guest server when sending commands to it.

To start db-server
```bash
mysql-env$> vagrant up db-server
```
To check status
```bash
vagrant status
```
To ssh to guest
```bash
vagrant ssh db-server
```

The last command should now bring you inside the db-server. Type cd /vagrant, this will bring you to the base path of the guest machine, which is the same path as your host machine, in mysql-env.

#### Test connections

Load mysqladmin. Use any browser in the host, or any system in the same network. Login with the username and password you assigned in bootstrap.sh

On your browser, go to: http://192.168.1.237:8306/phpmyadmin

Connect to mysql server in the host terminal, or any system in the same network. Login with the username and password you assigned in bootstrap.sh
```bash
mysql -u dbuser -p userpass -h 192.168.1.237
```

#### Installing Ubuntu VM on VirtualBox using Vagrant CLI

- In order to install Ubuntu or any other OS VM for that reason we need an installation image from somewhere.
- Here is the Vagrant boxes repository hosted on Vagrant cloud - app.vagrantup.com/boxes/search
- I will be using __ubuntu/bionic64__ image which is an official Ubuntu 18.04 LTS release build.


__Step 1__ -> Create a local directory and Add the Vagrant box to it

```bash
$ mkdir LCO_linux_install_demo
$ cd LCO_linux_install_demo/
$ mkdir shared_resources
$ ls shared_resources/

```
__Step 2__ -> Create Vagrantfile and add the Vagrant box

```bash

$ touch Vagrantfile
$ vagrant box add ubuntu/bionic64
==> box: Loading metadata for box 'ubuntu/bionic64'
    box: URL: https://vagrantcloud.com/ubuntu/bionic64
==> box: Adding box 'ubuntu/bionic64' (v20210315.1.0) for provider: virtualbox
    box: Downloading: https://vagrantcloud.com/ubuntu/boxes/bionic64/versions/20210315.1.0/providers/virtualbox.box
Download redirected to host: cloud-images.ubuntu.com
    box:
==> box: Successfully added box 'ubuntu/bionic64' (v20210315.1.0) for 'virtualbox'!

```

You can view the list of all vagrant boxes using following command ->
```bash
$ vagrant box list | grep bionic64
ubuntu/bionic64     (virtualbox, 20210315.1.0)
```
#### Initialize the Vagrant environment 
__Init__

- Command: vagrant init [name [url]]
- This initializes the current directory to be a Vagrant environment by creating an initial Vagrantfile if one does not already exist.
- If a first argument is given, it will prepopulate the config.vm.box setting in the created Vagrantfile.
- If a second argument is given, it will prepopulate the config.vm.box_url setting in the created Vagrantfile.

__Options__

--box-version - (Optional) The box version or box version constraint to add to the Vagrantfile.
--force - If specified, this command will overwrite any existing Vagrantfile.
--minimal - If specified, a minimal Vagrantfile will be created. This Vagrantfile does not contain the instructional comments that the normal Vagrantfile contains.
--output FILE - This will output the Vagrantfile to the given file. If this is "-", the Vagrantfile will be sent to stdout.
--template FILE - Provide a custom ERB template for generating the Vagrantfile.
- The following command will initialize the Vagrant environment in current working directory.
```bash
$ vagrant init ubuntu/bionic64 --force
or you can create a minimal Vagrantfile (no comments or helpers):
$ vagrant init -m hashicorp/bionic64

A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
```
Other options
```bash
Create a new Vagrantfile, overwriting the one at the current path:
$ vagrant init -f hashicorp/bionic64

Create a Vagrantfile with the specific box, from the specific box URL:
$ vagrant init my-company-box https://example.com/my-company.box

Create a Vagrantfile, locking the box to a version constraint:
$ vagrant init --box-version '> 0.1.5' hashicorp/bionic64
```
Have a look at Vagrantfile content
```bash
$ grep -v '^ *#' Vagrantfile

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/bionic64"

end
```

__Step 3__ -> Bring up the virtual machine by using ``` vagrant up```

- This command creates and configures guest machines according to your Vagrantfile.


__Step 4__ -> Log in to the guest Ubuntu VM
- In order to login to Ubuntu Guest VM run the ```vagrant ssh``` command.
- That's all. Now Ubuntu Server installed using Vagrant and running through VirtualBox on your computer.
- To exit from the Ubuntu console type ```exit``` to exit out of the server, and then you can run ```vagrant halt``` to shut it down.

#### Customizing Vagrantfile

In this section we will put our own LCO Linux cluster's (1 master node, 2 worker nodes) environment's Vagrant file which we can leverage to create our own cluster.
```bash
$ cat Vagrantfile
# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure(2) do |config|


  # Linux Master Server
  config.vm.synced_folder "shared_resources/", "/home/vagrant/shared_resources"
  config.vm.define "lco-linux-test-master" do |lcolinuxmaster|
    lcolinuxmaster.vm.box = "bento/ubuntu-18.04"
    lcolinuxmaster.vm.hostname = "lco-linux-test-master.example.com"
    lcolinuxmaster.vm.network "private_network", ip: "172.82.82.100"
    lcolinuxmaster.vm.provider "virtualbox" do |v|
      v.name = "lco-linux-test-master"
      v.memory = 4096
      v.cpus = 2
        end
  end

  NodeCount = 2

  # Linux Worker Nodes
  (1..NodeCount).each do |i|
    config.vm.synced_folder "shared_resources/", "/home/vagrant/shared_resources"
    config.vm.provision :shell, path: "bootstrap.sh"
    config.vm.define "lco-linux-test-worker#{i}" do |workernode|
      workernode.vm.box = "bento/ubuntu-18.04"
      workernode.vm.hostname = "lco-linux-test-worker#{i}.example.com"
      workernode.vm.network "private_network", ip: "172.82.82.10#{i}"
      workernode.vm.provider "virtualbox" do |v|
        v.name = "lco-linux-test-worker#{i}"
        v.memory = 2048
        v.cpus = 2
          end

    end
  end

end
```
Highlights from the above file -

config.vm.network - Configures networks on the machine. Follow here for VirtualBox provider specific networking options.
config.vm.provider - Configures provider-specific configuration, which is used to modify settings which are specific to a certain provider.
config.vm.synced_folder - Configures synced folders on the machine, so that folders on your host machine can be synced to and from the guest machine.
config.vm.box - It is a shorthand to a box in HashiCorp's Vagrant Cloud then this value does not need to be specified.
config.vm.hostname - The hostname the machine should have. Defaults to nil. If nil, Vagrant will not manage the hostname. If set to a string, the hostname will be set on boot. If set, Vagrant will update /etc/hosts on the guest with the configured hostname.
Here is the bootstrap.sh file which will be executed once the systems are booted and become ready to use.

```bash
$ cat bootstrap.sh
#!/bin/bash
echo ""
echo ""
echo "Checking your Linux release:"
echo ""
cat /etc/os-release
echo ""
echo ""
echo "BOOTSTRAP:"
echo "----------------------------------------"
echo "update ssh config"
sudo su -
sed -i 's/#\?\(PermitRootLogin\s*\).*$/\1 yes/' /etc/ssh/sshd_config
systemctl restart sshd
apt-get update -y
apt-get install vim lshw nmap telnet gcc perl kernel-headers kernel-devel -y
/sbin/rcvboxadd setup

```
Here I am changing some ssh configurations and installing basic packages.

You can modify both the files Vagrantfile and bootstrap.sh as per your requirements. They both should be present in the same folder where we are initiating vagrant.
```bash
$ ls
bootstrap.sh*  shared_resources/  Vagrantfile
```
It's time to provision the guest virtual machines.

```bash
$ vagrant.exe up
Bringing machine 'lco-linux-test-master' up with 'virtualbox' provider...
Bringing machine 'lco-linux-test-worker1' up with 'virtualbox' provider...
Bringing machine 'lco-linux-test-worker2' up with 'virtualbox' provider...

...
...
```
This may take a few minutes to complete but it should finish without any errors.

All our machines are up and running. Verify their status.
```bash
$ vagrant status

```

To destroy the test environment you created run the following command -
```bash
$ vagrant destroy
    lco-linux-test-worker2: Are you sure you want to destroy the 'lco-linux-test-worker2' VM? [y/N] y
==> lco-linux-test-worker2: Destroying VM and associated drives...
    lco-linux-test-worker1: Are you sure you want to destroy the 'lco-linux-test-worker1' VM? [y/N] y
==> lco-linux-test-worker1: Destroying VM and associated drives...
    lco-linux-test-master: Are you sure you want to destroy the 'lco-linux-test-master' VM? [y/N] y
==> lco-linux-test-master: Destroying VM and associated drives...
```
