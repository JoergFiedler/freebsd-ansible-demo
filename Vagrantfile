# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "FreeBSD10"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "https://s3-eu-west-1.amazonaws.com/vastland.moumantai.de/public/FreeBSD/vagrant-box/FreeBSD-10-vagrant-base.box"
  config.vm.box_download_checksum = "3418526d3a67313d3763ac75eb5b46c2d5d90837342e9d4fdef46dc387128735"
  config.vm.box_download_checksum_type = "sha256"

  config.ssh.proxy_command = "/usr/bin/ssh -p 2222 -i ~/.vagrant.d/insecure_private_key  -o 'StrictHostKeyChecking no' -o 'UserKnownHostsFile=/dev/null' -q vagrant@localhost -W %h:22"
  config.ssh.host = "10.0.2.15"

  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false

    vb.memory = 2048
    vb.cpus = 2
    vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
    vb.customize ["modifyvm", :id, "--audio", "none"]
    vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
    vb.customize ["modifyvm", :id, "--nictype2", "virtio"]
  end

  # Use Ansible as provisioning tool
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "site.yml"
    ansible.raw_ssh_args = ["-o ProxyCommand='/usr/bin/ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -p 2222 -i ~/.vagrant.d/insecure_private_key -q vagrant@localhost -W %h:22'"]
    ansible.verbose = 'vvvv'
  end

end
