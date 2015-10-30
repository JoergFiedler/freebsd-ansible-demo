[![Build Status](https://travis-ci.org/JoergFiedler/freebsd-ansible-demo.svg?branch=master)](https://travis-ci.org/JoergFiedler/freebsd-ansible-demo)

# FreeBSD iocage Ansible

How to use Ansible and iocage to set up a FreeBSD jail server.

![Big Picture](https://github.com/JoergFiedler/freebsd-ansible-demo/raw/master/doc/big-picture-draw.io.png)

## Goals

- Ansible playbook that creates a FreeBSD server which hosts multiple jails.
- Travis is used to run/test the playbook.
- No service on the host is exposed externally.
- All external connections terminate within a jail.
- The playbook creates the jails as well (examples for nginx/php, postfix, cyrus-imap).

## Requirements

1. Ansible
1. VirtualBox
1. AWS account, with allows you to create and destroy EC2 instances (if you want to use Vagrant's aws provider)

## Notes

The box file `metadata.json` provides a box for VirtualBox and AWS. The AMI ids are preconfigured. The only thing you have to do is to choose a region `aws.region`.

## Howto

    git clone https://github.com/JoergFiedler/freebsd-ansible-demo.git
    cd freebsd-ansible-demo
    for provider in aws virtualbox; do vagrant box add --name JoergFiedler/freebsd-box  metadata.json --provider $provider; done
    vagrant up

Login into the jail host.

    vagrant ssh

## Next Steps

1. Create other jails (web, dns, mail)
1. Setup backup strategy using [Tarsnap](https://www.tarsnap.com/man-tarsnap.1.html)
1. Setup monitoring using datadog.
1. The AMI's used come from [here](http://www.daemonology.net/freebsd-on-ec2/). I would prefer to use a more stripped down FreeBSD installation. So, I would like to create an AMI that only contains a minimal FreeBSD installation plus the packages required to run Ansible playbooks.

## Useful Links

1. [FreeBSD on EC2](http://www.daemonology.net/freebsd-on-ec2/)
1. [EC2 Instance IP Addressing](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-instance-addressing.html)
1. [EC2 Device Mapping](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/block-device-mapping-concepts.html)
1. [unix domain socket too long](https://github.com/ansible/ansible/issues/11536)
1. [Encrypted Variables](http://docs.travis-ci.com/user/environment-variables/#Encrypted-Variables)

## Powered By

1. [FreeBSD](https://www.freebsd.org)
1. [iocage](https://github.com/pannon/iocage)
1. [VirtualBox](https://www.virtualbox.org)
1. [Ansible](http://www.ansible.com)
1. [Vagrant](https://www.vagrantup.com)
