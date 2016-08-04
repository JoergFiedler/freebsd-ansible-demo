# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = '2'
PROXY_COMMAND = '/usr/bin/ssh -p %p ' +
                '-i ~/.vagrant.d/insecure_private_key ' +
                '-o StrictHostKeyChecking=no ' +
                '-o UserKnownHostsFile=/dev/null ' +
                '-q ' +
                '-W %h:22'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = 'JoergFiedler/freebsd-box'
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.ssh.insert_key = false

  config.vm.define 'btsync' do |btsync|
    btsync.vm.provision 'ansible', type: 'ansible'  do |ansible|
      ansible.playbook = './playbook/btsync.yml'
    end
  end

  config.vm.define 'nginx-single-host' do |single|
    single.vm.provision 'ansible', type: 'ansible'  do |ansible|
      ansible.playbook = './playbook/nginx-single-host.yml'
    end
  end

  config.vm.define 'nginx-multi-host' do |multi|
    multi.vm.provision 'ansible', type: 'ansible'  do |ansible|
      ansible.playbook = './playbook/nginx-multi-host.yml'
    end
  end

  config.vm.define 'joomla' do |btsync|
    btsync.vm.provision 'ansible', type: 'ansible'  do |ansible|
      ansible.playbook = './playbook/joomla.yml'
    end
  end

  config.vm.provision 'ansible', type: 'ansible' do |ansible|
      ansible.galaxy_roles_path = ENV['ANSIBLE_ROLES_PATH'] || './roles'
      ansible.galaxy_role_file = './roles.yml'
      ansible.galaxy_command = 'ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path}'
      ansible.tags = ENV['ANSIBLE_TAGS']
      ansible.skip_tags = ENV['ANSIBLE_SKIP_TAGS']
      ansible.verbose = ENV['ANSIBLE_VERBOSE']
  end

  config.vm.provider 'virtualbox' do |vb, config|
    proxy_command = "#{PROXY_COMMAND} vagrant@%h"
    config.ssh.proxy_command = proxy_command

    config.vm.provision 'ansible', type: 'ansible' do |ansible|
      ansible.raw_ssh_args = ["-o ProxyCommand='#{proxy_command}'"]
    end

    config.vm.network 'private_network', type: 'dhcp', auto_config: false
    config.vm.network 'forwarded_port', guest: 80, host: 2080
    config.vm.network 'forwarded_port', guest: 443, host: 20443
    config.vm.network 'forwarded_port', guest: 10022, host: 10022
    config.vm.network 'forwarded_port', guest: 10023, host: 10023

    vb.gui = false
    vb.memory = 4096
    vb.cpus = 2
    vb.customize ['modifyvm', :id, '--hwvirtex', 'on']
    vb.customize ['modifyvm', :id, '--audio', 'none']
    vb.customize ['modifyvm', :id, '--nictype2', 'virtio']
  end

  config.vm.provider 'aws' do |aws, config|
    proxy_command = "#{PROXY_COMMAND} ec2-user@%h"
    config.ssh.proxy_command = proxy_command
    config.ssh.username = 'ec2-user'

    config.vm.provision 'ansible', type: 'ansible' do |ansible|
      ansible.extra_vars = './ec2.yml'
      ansible.raw_ssh_args = ["-o ProxyCommand='#{proxy_command}'"]
    end

    aws.access_key_id = ENV['AWS_ACCESS_KEY_ID']
    aws.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']
    aws.ssh_host_attribute = :dns_name
    aws.keypair_name = 'ec2-user'
    aws.region = 'eu-west-1'
    aws.user_data = "#!/bin/sh\necho 'pass all keep state' >> /etc/pf.conf\necho pf_enable=YES >> /etc/rc.conf\necho pflog_enable=YES >> /etc/rc.conf\necho 'firstboot_pkgs_list=\"awscli sudo bash python27\"' >> /etc/rc.conf\necho ifconfig_xn0=\"SYNCDHCP -tso\"\nmkdir -p /usr/local/etc/sudoers.d\necho 'ec2-user ALL=(ALL) NOPASSWD: ALL' >> /usr/local/etc/sudoers.d/ec2-user"
    aws.block_device_mapping = [
      { 'DeviceName' => '/dev/sda1',
        'Ebs.VolumeSize' => 10,
        'Ebs.VolumeType' => 'gp2',
        'Ebs.DeleteOnTermination' => true },
      { 'DeviceName' => '/dev/sdf',
        'Ebs.VolumeSize' => 10,
        'Ebs.VolumeType' => 'gp2',
        'Ebs.DeleteOnTermination' => true }
    ]
    aws.terminate_on_shutdown = true
  end

end
