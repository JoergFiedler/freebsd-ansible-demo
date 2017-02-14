[![Build Status](https://travis-ci.org/JoergFiedler/freebsd-ansible-demo.svg?branch=master)](https://travis-ci.org/JoergFiedler/freebsd-ansible-demo)

# Ansible Versions

Supported Ansible versions:
- = 2.0.1.0 - tested
- = 2.1.1.0 - works
- = 2.2.0.0 and later - broken 

# FreeBSD, iocage and Ansible

How to use Ansible and iocage to set up a FreeBSD btsync server.

![Big Picture](https://github.com/JoergFiedler/freebsd-ansible-demo/raw/master/doc/big-picture-draw.io.png)

## Goals

- Ansible playbook that creates a FreeBSD server which hosts multiple jails.
- Travis is used to run/test the playbook.
- No service on the host is exposed externally.
- All external connections terminate within a jail.
- Roles can be reused using Ansible Galaxy.
- Combine any of those roles to create FreeBSD server, which perfectly suits you.

## Requirements

1. Vagrant >= 1.8.1
1. Ansible == 2.0.1
1. VirtualBox

### Ansible Roles

The following roles are available.

 1. [freebsd-build-server - Creates a FreeBSD poudriere build server](https://galaxy.ansible.com/JoergFiedler/freebsd-build-server/)
 1. [freebsd-jail-host - FreeBSD Jail host](https://galaxy.ansible.com/JoergFiedler/freebsd-jail-host/)
 1. [freebsd-jailed - Provides a jail](https://galaxy.ansible.com/JoergFiedler/freebsd-jailed/)
 1. [freebsd-jailed-nginx - Provides a jailed nginx server](https://galaxy.ansible.com/JoergFiedler/freebsd-jailed-nginx/)
 1. [freebsd-jailed-sshd - Provides a jailed sshd server.](https://galaxy.ansible.com/JoergFiedler/freebsd-jailed-sshd/)
 1. [freebsd-jailed-syslogd - Provides a jailed syslogd](https://galaxy.ansible.com/JoergFiedler/freebsd-jailed-syslogd/)
 1. [freebsd-jailed-btsync - Provides a jailed btsync instance server](https://galaxy.ansible.com/JoergFiedler/freebsd-jailed-btsync/)
 1. [freebsd-jailed-joomla - Installs Joomla](https://galaxy.ansible.com/JoergFiedler/freebsd-jailed-joomla/)
 1. [freebsd-jailed-mariadb - Provides a jailed MariaDB server](https://galaxy.ansible.com/JoergFiedler/freebsd-jailed-mariadb/)
 1. [freebsd-jailed-wordpress - Provides a jailed Wordpress server.](https://galaxy.ansible.com/JoergFiedler/freebsd-jailed-wordpress/)

## HowTo

The following machines are configured (replace MACHINE with one of those names):

* btsync

### Start machines using VirtualBox

    git clone https://github.com/JoergFiedler/freebsd-ansible-demo.git
    cd freebsd-ansible-demo
    for provider in aws virtualbox; do \
      vagrant box add \
        --provider ${provider} \
        https://rawgit.com/JoergFiedler/freebsd-box/freebsd-11.0/metadata.json; \
    done
    vagrant up btsync

You should now be able to access btsync webui using https://localhost:10443 with
user `admin` and password `admin`.

## Next Steps

1. Create other jail roles (~~nginx~~, ~~wordpress~~, mail)
1. ~~Role which uses [Tarsnap](https://www.tarsnap.com/man-tarsnap.1.html) to backup jail's user data.~~
1. The AMI's used come from [here](http://www.daemonology.net/freebsd-on-ec2/).
   I would prefer to use a more stripped down FreeBSD installation. That's why 
   I like to create an AMI that only contains a minimal FreeBSD installation 
   plus the packages required to run Ansible playbooks.

## Useful Links

1. [FreeBSD on EC2](http://www.daemonology.net/freebsd-on-ec2/)
1. [EC2 Instance IP Addressing](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-instance-addressing.html)
1. [EC2 Device Mapping](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/block-device-mapping-concepts.html)
1. [unix domain socket too long](https://github.com/ansible/ansible/issues/11536)
1. [Encrypted Variables](http://docs.travis-ci.com/user/environment-variables/#Encrypted-Variables)
1. [Strong SSL Security On nginx](https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html)
1. [ZFS Performance](http://open-zfs.org/wiki/Performance_tuning#LZ4_compression)
1. [FreeBSD Network Tuning](https://calomel.org/freebsd_network_tuning.html)

## Powered By

1. [FreeBSD](https://www.freebsd.org)
1. [iocage](https://github.com/pannon/iocage)
1. [VirtualBox](https://www.virtualbox.org)
1. [Ansible](http://www.ansible.com)
1. [Vagrant](https://www.vagrantup.com)
