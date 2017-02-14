VAGRANTFILE_API_VERSION = '2'
PROXY_COMMAND = '/usr/bin/ssh -p %p ' +
    '-i ~/.vagrant.d/insecure_private_key ' +
    '-o StrictHostKeyChecking=no ' +
    '-o UserKnownHostsFile=/dev/null ' +
    '-q ' +
    '-W %h:22'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = 'JoergFiedler/freebsd-11.0'
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.ssh.insert_key = false

  config.vm.define 'btsync' do |btsync|
    btsync.vm.provision 'ansible', type: 'ansible' do |ansible|
      ansible.playbook = './playbook/btsync.yml'
    end
  end

  config.vm.provision 'ansible', type: 'ansible' do |ansible|
    ansible.galaxy_roles_path = ENV['ANSIBLE_ROLES_PATH'] || './playbook/roles'
    # ansible.galaxy_role_file = './roles.yml'
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
    config.vm.network 'forwarded_port', guest: 80, host: 10080
    config.vm.network 'forwarded_port', guest: 443, host: 10443
    config.vm.network 'forwarded_port', guest: 10101, host: 10101

    vb.gui = false
    vb.memory = 4096
    vb.cpus = 2
    vb.customize ['modifyvm', :id, '--hwvirtex', 'on']
    vb.customize ['modifyvm', :id, '--audio', 'none']
  end
end
