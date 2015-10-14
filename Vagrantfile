# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
PRIVATE_IP = "10.0.2.15"
PROXY_COMMAND = "/usr/bin/ssh -p 2222 " +
                "-i ~/.vagrant.d/insecure_private_key " +
                "-o StrictHostKeyChecking=no " +
                "-o UserKnownHostsFile=/dev/null " +
                "-q " +
                "vagrant@localhost " +
                "-W %h:22"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "FreeBSD10"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "https://s3-eu-west-1.amazonaws.com/vastland.moumantai.de/public/FreeBSD/vagrant-box/FreeBSD-10.2-pf-enabled-vagrant-base.box"
  config.vm.box_download_checksum = "100e0cb5a6c58d197e0e95013bf80b189356ae0f75af2f8e6089d880d8d3cb55"
  config.vm.box_download_checksum_type = "sha256"

  config.vm.network "private_network", ip: PRIVATE_IP, auto_config: false

  config.ssh.proxy_command = PROXY_COMMAND
  config.ssh.host = PRIVATE_IP
  config.ssh.insert_key = false

  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false

    vb.memory = 2048
    vb.cpus = 2
    vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
    vb.customize ["modifyvm", :id, "--audio", "none"]
    vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
  end

  # Use Ansible as provisioning tool
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "site.yml"
    ansible.tags = ENV['CMD_ANSIBLE_TAGS']
    ansible.raw_ssh_args = ["-o ProxyCommand='" + PROXY_COMMAND + "'"]
    ansible.verbose = ENV['CMD_ANSIBLE_VERBOSE']
  end

end
