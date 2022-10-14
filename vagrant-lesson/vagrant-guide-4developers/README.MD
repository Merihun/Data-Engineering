## Vagrant

- Vagrant is a virtualization tool used to create development environments that can be consistently and easily re-produced among team members.
- Vagrant builds a VM based on the specifications in the Vagrantfile. 
- Vagrant builds a VM based on the specifications in the Vagrantfile. 
- It creates a shared folder in the VM under ```“/vagrant”``` that automatically syncs the files in your project directory.
- There is no need to worry about mounts/ volumes for data accessibility/ persistence.

#### Vagrantfile

```bash
# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
    config.vm.network "forwarded_port", guest: 80, host: 8000
  config.vm.provision :shell, path: "install.sh", privileged: false

end
```




NOTE: This article is adopted and narreted from:  https://towardsdatascience.com/fall-in-love-with-your-environment-setup-vagrant-edition-40ee9313291b
