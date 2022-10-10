

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
