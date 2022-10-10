

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

- The following command will initialize the Vagrant environment in current working directory.
```bash
$ vagrant init ubuntu/bionic64 --force

A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
```
