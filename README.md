# Ansible/FreeBSD/iocage

It's still work in progress.

Simple demo project. Shows how to use Ansible and iocage to set up
a FreeBSD jail server.

## Requirements

You need to have to following software installed.

1. Ansible
1. VirtualBox

## Howto

    git clone https://github.com/JoergFiedler/freebsd-ansible-demo.git
    cd freebsdd-ansible-demo
    vagrant up

After the instance is provisioned you have to restart it.

Login into the jail host.

    vagrant ssh

or

    ssh -F ssh-config 10.1.0.1

## Next Steps

1. Remove reboot step
1. Complete documentation (draw the big picture)
1. Synchronize Vagrant/Ansible configuration
1. Create other jails (web, dns, mail)
1. Create configuration to recreate this instance on EC2 

