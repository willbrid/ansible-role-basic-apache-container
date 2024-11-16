# Sandbox

Il s'agit ici de monter un environnement sandbox via **vagrant** pour tester et valider les rôles Ansible dans une configuration virtualisée locale. Nous utilisons ici **virtualbox 7.0** couplé à **vagrant**.

```
mkdir $HOME/ansible-multi-platform && cd $HOME/ansible-multi-platform
```

```
wget https://download.virtualbox.org/virtualbox/7.0.20/VBoxGuestAdditions_7.0.20.iso
```

```
vi Vagrantfile
```

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vbguest.auto_update = false
  config.vbguest.no_remote = true
  config.vbguest.iso_path = "./VBoxGuestAdditions_7.0.20.iso"

  # General Vagrant VM configuration
  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.provider :virtualbox do |v|
    v.memory = 2048
    v.cpus = 2
    v.linked_clone = true
  end
  
  # Rocky Server
  config.vm.define "rocky-server" do |srv|
    srv.vm.box = "willbrid/rockylinux8"
    srv.vm.box_version = "0.0.3"
    srv.vm.hostname = "rocky-server"
    srv.vm.network :private_network, ip: "192.168.56.117"
  end

  # Ubuntu Server
  config.vm.define "ubuntu-server" do |us|
    us.vm.box = "bento/ubuntu-22.04"
    us.vm.box_version = "202407.23.0"
    us.vm.hostname = "ubuntu-server"
    us.vm.network :private_network, ip: "192.168.56.118"
  end
end
```

```
vagrant up
```