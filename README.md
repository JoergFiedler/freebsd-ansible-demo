https://travis-ci.org/JoergFiedler/freebsd-ansible-demo.svg?branch=travis-integration
# FreeBSD iocage Ansible

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

## Next Steps

1. Create other jails (web, dns, mail)
1. Synchronize Vagrant/Ansible configuration

## Useful Links

1. [FreeBSD on EC2](http://www.daemonology.net/freebsd-on-ec2/)
1. [EC2 Instance IP Addressing](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-instance-addressing.html)
1. {EC2 Device Mapping](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/block-device-mapping-concepts.html)
1. [unix domain socket too long](https://github.com/ansible/ansible/issues/11536)
1. [Encrypted Variables](http://docs.travis-ci.com/user/environment-variables/#Encrypted-Variables)

## Powered By

1. [FreeBSD](https://www.freebsd.org)
1. [iocage](https://github.com/pannon/iocage)
1. [VirtualBox](https://www.virtualbox.org)
1. [Ansible](http://www.ansible.com)
1. [Vagrant](https://www.vagrantup.com)
