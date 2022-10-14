## Vagrant

- Vagrant is a virtualization tool used to create development environments that can be consistently and easily re-produced among team members.
- Vagrant builds a VM based on the specifications in the Vagrantfile. 
- Vagrant builds a VM based on the specifications in the Vagrantfile. 
- It creates a shared folder in the VM under ```“/vagrant”``` that automatically syncs the files in your project directory.
- There is no need to worry about mounts/ volumes for data accessibility/ persistence.

#### Vagrantfile

```bash
1 # -*- mode: ruby -*-
2 # vi: set ft=ruby :
3 Vagrant.configure("2") do |config|
4   config.vm.box = "hashicorp/bionic64"
5     config.vm.network "forwarded_port", guest: 80, host: 8000
5   config.vm.provision :shell, path: "install.sh", privileged: false
7 
8 end
```
Line 4: VirtualBox base image to use - “hashicorp/bionic64”.
Line 5: forwards port 80 on the VM to local port 8000.
Line 6 runs the ```install.sh``` script to provision the VM. 
- privileged is set to ```false``` so that the script is not run as the root user. 
- If the install script is run as the root user, things installed like poetry will not be accessible to the vagrant user.
- You can still specify sudo in front of specific commands in the install script that require root level access.

#### install.sh script

```bash

#!/usr/bin/env bash

sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
sudo apt-get install -y zlib1g-dev build-essential libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev python-dev python-setuptools python-pip python-smbus openssl libffi-dev

# Install python 3.8.3
cd /opt
sudo wget https://www.python.org/ftp/python/3.8.3/Python-3.8.3.tgz

sudo tar xzf Python-3.8.3.tgz
cd Python-3.8.3
sudo ./configure --enable-optimizations
sudo make altinstall

sudo apt-get install -y libssl-dev curl python3-pip

# Install poetry pre-reqs
sudo apt-get install python3-distutils python3-lib2to3

# Install poetry
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -

# activate poetry (since didn't restart bash)
source $HOME/.poetry/env

# terminal formatting
# automatically activate the venv at a project's root
cd /vagrant
echo >> ~/.bashrc
cat auto_activate_.venv.txt >>  ~/.bashrc

echo >> ~/.bashrc
echo "# load terminal prompt formatting from .bash_prompt" >> ~/.bashrc
echo "source ~/.bash_prompt" >> ~/.bashrc
sudo ln -s /vagrant/.bash_prompt ~/.bash_prompt

echo >> ~/.bashrc
echo "# automatically cd to /vagrant folder when vagrant ssh" >> ~/.bashrc
echo "cd /vagrant" >> ~/.bashrc

# Install project dependencies
poetry env use python3.8
poetry install
```




NOTE: This article is adopted and narreted from:  https://towardsdatascience.com/fall-in-love-with-your-environment-setup-vagrant-edition-40ee9313291b
