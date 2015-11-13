# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
VB_IP = "10.0.2.15"
VB_SSH_USER = "vagrant"
EC2_SSH_USER = "ec2-user"
PROXY_COMMAND = "/usr/bin/ssh -p %p " +
                "-i ~/.vagrant.d/insecure_private_key " +
                "-o StrictHostKeyChecking=no " +
                "-o UserKnownHostsFile=/dev/null " +
                "-q " +
                "-W %h:22"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "JoergFiedler/freebsd-box"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |vb, override|
    proxy_command = "#{PROXY_COMMAND} #{VB_SSH_USER}@localhost"

    override.vm.network "private_network", ip: VB_IP, auto_config: false

    override.ssh.proxy_command = proxy_command
    override.ssh.host = VB_IP

    override.vm.provision "ansible" do |ansible|
      ansible.playbook = "site.yml"
      ansible.tags = ENV['CMD_ANSIBLE_TAGS']
      ansible.verbose = ENV['CMD_ANSIBLE_VERBOSE']
      ansible.raw_ssh_args = ["-o ProxyCommand='#{proxy_command}'"]
    end

    vb.gui = false
    vb.memory = 2048
    vb.cpus = 2
    vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
    vb.customize ["modifyvm", :id, "--audio", "none"]
  end

  config.vm.provider :aws do |aws, override|
    proxy_command = "#{PROXY_COMMAND} -o ServerAliveInterval=5 -o ExitOnForwardFailure=yes -o ConnectTimeout=5 #{EC2_SSH_USER}@%h"

    override.ssh.proxy_command = proxy_command
    override.ssh.username = "ec2-user"
    override.ssh.private_key_path = "~/.vagrant.d/insecure_private_key"

    override.vm.provision "ansible" do |ansible|
      ansible.playbook = "site.yml"
      ansible.extra_vars = "./ec2.yml"
      ansible.tags = ENV['CMD_ANSIBLE_TAGS']
      ansible.verbose = ENV['CMD_ANSIBLE_VERBOSE']
      ansible.raw_ssh_args = ["-o ProxyCommand='#{proxy_command}'"]
    end

    aws.access_key_id = ENV['ACCESS_KEY']
    aws.secret_access_key = ENV['SECRET_ACCESS_KEY']
    aws.ssh_host_attribute = :dns_name
    aws.keypair_name = "ec2-user"
    aws.region = "eu-west-1"
    aws.user_data = "#!/bin/sh\necho 'pass all keep state' >> /etc/pf.conf\necho pf_enable=YES >> /etc/rc.conf\necho pflog_enable=YES >> /etc/rc.conf\necho 'firstboot_pkgs_list=\"awscli sudo bash python27\"' >> /etc/rc.conf\nmkdir -p /usr/local/etc/sudoers.d\necho 'ec2-user ALL=(ALL) NOPASSWD: ALL' >> /usr/local/etc/sudoers.d/ec2-user"
    aws.block_device_mapping = [
      { 'DeviceName' => '/dev/sda1',
        'Ebs.VolumeSize' => 10,
        "Ebs.VolumeType" => "gp2",
        "Ebs.DeleteOnTermination" => true },
      { 'DeviceName' => '/dev/sdf',
        'Ebs.VolumeSize' => 10,
        "Ebs.VolumeType" => "gp2",
        "Ebs.DeleteOnTermination" => true }
    ]
    aws.terminate_on_shutdown = true
  end

end
