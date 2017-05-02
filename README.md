# Configuration repository: vagrant

## Description

A test and development Vagrant environment for OpenStack Kolla.

# Requirements

* Vagrant and Virtualbox have to be installed.

  * Instructions are available at https://www.vagrantup.com/docs/installation/.

* Ansible has to be installed.

  * Instructions are available at http://docs.ansible.com/ansible/intro_installation.html.

  * At least Ansible >= 2.2.0.0 has to be used. Use ``pip`` to install Ansible. The Ansible
    package included in Ubuntu 16.04 is pretty old.

* As an alternative install Ansible e.g. with ``apt-get install -y python-pip; pip install ansible`` and run
  ``ansible-playbook 000-prepare-vagrant.yml`` to install Vagrant and Virtualbox.

* By default the environment uses 8 VCPUs, 24 GByte memory, and 160 GByte storage.

## Preparations

* Add your credentials for ``nexus.betacloud.io`` to the ``vars/secrets.yml`` file.

  * If you do not have credentials send a mail to ``support@betacloud.io`` and request them.

* Add your Kolla SSH keypair to the ``vars/secrets.yml`` file.

  * If you do not have one you can generate it with ``ssh-keygen -f id_rsa.kolla``.

## Deployment

```shell
$ ansible-playbook 000-prepare-localhost.yml
 [WARNING]: provided hosts list is empty, only localhost is available


PLAY [Create ssh private key file] *********************************************

TASK [Include secrets] *********************************************************
ok: [localhost]

TASK [Set permissions of secrets directory] ************************************
changed: [localhost]

TASK [Create ssh private key file] *********************************************
changed: [localhost]

PLAY [Download and install private ansible roles] ******************************

TASK [Include secrets] *********************************************************
ok: [localhost]

TASK [Download private tarballs] ***********************************************
changed: [localhost] => (item=betacloud.seed)

TASK [Install required ansible roles] ******************************************
changed: [localhost]

TASK [Remove private tarballs] *************************************************
ok: [localhost] => (item=betacloud.seed)

PLAY RECAP *********************************************************************
localhost                  : ok=7    changed=4    unreachable=0    failed=0
```

```shell
$ vagrant up --no-provision
Bringing machine 'ubuntu' up with 'virtualbox' provider...
==> ubuntu: Box 'ubuntu/xenial64' could not be found. Attempting to find and install...
    ubuntu: Box Provider: virtualbox
    ubuntu: Box Version: >= 0
==> ubuntu: Loading metadata for box 'ubuntu/xenial64'
    ubuntu: URL: https://atlas.hashicorp.com/ubuntu/xenial64
==> ubuntu: Adding box 'ubuntu/xenial64' (v20170511.0.0) for provider: virtualbox
    ubuntu: Downloading: https://atlas.hashicorp.com/ubuntu/boxes/xenial64/versions/20170511.0.0/providers/virtualbox.box
==> ubuntu: Successfully added box 'ubuntu/xenial64' (v20170511.0.0) for 'virtualbox'!
==> ubuntu: Importing base box 'ubuntu/xenial64'...
==> ubuntu: Matching MAC address for NAT networking...
==> ubuntu: Checking if box 'ubuntu/xenial64' is up to date...
==> ubuntu: Setting the name of the VM: configuration-vagrant_ubuntu_1494584370862_85831
==> ubuntu: Clearing any previously set network interfaces...
==> ubuntu: Preparing network interfaces based on configuration...
    ubuntu: Adapter 1: nat
==> ubuntu: Forwarding ports...
    ubuntu: 22 (guest) => 2222 (host) (adapter 1)
==> ubuntu: Running 'pre-boot' VM customizations...
==> ubuntu: Booting VM...
==> ubuntu: Waiting for machine to boot. This may take a few minutes...
    ubuntu: SSH address: 127.0.0.1:2222
    ubuntu: SSH username: ubuntu
    ubuntu: SSH auth method: password
    ubuntu: 
    ubuntu: Inserting generated public key within guest...
    ubuntu: Removing insecure key from the guest if it's present...
    ubuntu: Key inserted! Disconnecting and reconnecting using new SSH key...
==> ubuntu: Machine booted and ready!
==> ubuntu: Checking for guest additions in VM...
==> ubuntu: Setting hostname...
==> ubuntu: Mounting shared folders...
    ubuntu: /opt/configuration => /root/configuration-vagrant
==> ubuntu: Machine not provisioned because `--no-provision` is specified.
```

```shell
$ vagrant provision
==> ubuntu: Running provisioner: ansible...
    ubuntu: Running ansible-playbook...

PLAY [Install python version 2] ************************************************

TASK [Include distribution specific tasks] *************************************
included: /root/configuration-vagrant/tasks/001-Debian.yml for ubuntu

TASK [Install python version 2] ************************************************
ok: [ubuntu]

PLAY [Make ssh pipelining working] *********************************************

TASK [Do not require tty for all users] ****************************************
ok: [ubuntu]

...
```

## Notes

* In this repository do not edit files with this note:

  ``DO NOT EDIT THIS FILE BY HAND -- FILE IS SYNCHRONIZED REGULARLY``
