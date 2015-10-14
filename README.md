# Ansible/FreeBSD/iocage

How to use Ansible and iocage to set up a FreeBSD jail server.

![Big Picture](https://github.com/JoergFiedler/freebsd-ansible-demo/raw/master/doc/big-picture-draw.io.png)

## Goals

- Ansible playbook that creates a FreeBSD server which hosts multiple jails.
- No service on the host is externally exposed.
- All services that accept external connections terminate within a jail.
- The playbook creates the jails as well (examples for nginx/php, postfix, cyrus-imap).

## Requirements

1. Ansible
1. VirtualBox

## Howto

    git clone https://github.com/JoergFiedler/freebsd-ansible-demo.git
    cd freebsdd-ansible-demo
    vagrant up

Login into the jail host.

    vagrant ssh

or

    ssh -F ssh-config 10.1.0.1

## Next Steps

1. Create other jails (web, dns, mail)
1. Synchronize Vagrant/Ansible configuration
1. Create configuration to recreate this instance on EC2 

## Powered By

1. [FreeBSD](https://www.freebsd.org)
1. [iocage](https://github.com/pannon/iocage)
1. [VirtualBox](https://www.virtualbox.org)
1. [Ansible](http://www.ansible.com)
1. [Vagrant](https://www.vagrantup.com)
