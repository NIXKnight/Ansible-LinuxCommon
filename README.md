# **Ansible-LinuxCommon**

Ansible-LinuxCommon is an Ansible role for configuring shell environment and installing some very basic utilities on a new Linux system. It does the following on a default install:
* Change hostname.
* Replace the host file according to the hostname.
* Replace a pre-modified `/etc/bash.bashrc`, `.profile` and `.bashrc` files for root and user.
* Replace default `sources.list` to include all official repositories e.g. `main`, `contrib`, `non-free` (in case of Debian) and `main`, `restricted`, `universe`, `multiverse`, `partner` (in case of Ubuntu).
* Install updates.
* Installs `htop`, `iotop`, `iftop`, `ncdu`, `ntp`, `ntpdate`, `curl`, `bash-completion`, `vim`, `mtr-tiny`, `git` and `unzip`.
* Purges `exim`.
* configures `vim` editor.
* Install `sudo`.
* Configure password-less `sudo` for `sudo` group users.
* Add `ansible_ssh_user` to sudo group.
* Installs `libpam-systemd` (only for Debian Jessie. See [this](https://serverfault.com/questions/706475/ssh-sessions-hang-on-shutdown-reboot) thread)
* Check kernel version upgrade and reboots the server if there is a kernel update.

bash.bashrc is a modified version of the default file that comes with Debian.

Bits for `sudo` are added incase you have installed Debian/Ubuntu via CD/DVD.

## **Requirements**

At the moment this role is being written for Debian based distributions e.g. Debian/Ubuntu. It will evolve to include other major distributions.

## **Role Variables**

The variable `change_hostname` in `defaults/main.yml` based on which the role decides wether to change the hostname or not. By default it is set to `True`. If you don't want to change change hostname, you can set it to `False`. See the example below. The role sets `inventory_hostname_short` as the hostname.

Based on `setup_sudo` variable in `defaults/main.yml`, the role can decide if `sudo` related actions should be performed. By defaults its set to `True`. You can change it to `False` if you don't want the role to perform `sudo` actions. See the example below.

`enable_src_repos` controls if source repositories should be enabled or not. By default it is set to `False`.

Variables:
* `debian_mirror`
* `debian_security_mirror`
* `ubuntu_mirror`
* `Ubuntu_canonical_partner_mirror`

Can be used to change mirrors for package repositories. See `defaults/main.yml` for their default values.

By default, variables:
* `debian_repos`
* `ubuntu_repos`

Are used to enable repositories `main`, `contrib`, `non-free` (in case of Debian) and `main`, `restricted`, `universe`, `multiverse`, `partner` (in case of Ubuntu).
## **Dependencies**

This role doesn't depends on any other role for execution. The role is written to support both CD/DVD installs and perinstalled images (e.g. Amazon Machine Images) where `sudo` is already installed and configured. You can also use `su` method to become root if you don't have sudo installed.

## **Example Playbook**

Facts gathering must be enabled.

An example of running the role is as follows:

    - hosts: servers
      gather_facts: True
      roles:
         - Ansible-LinuxCommon

If you don't want to change hostname:

    - hosts: servers
      gather_facts: True
      roles:
         - {role: Ansible-LinuxCommon, change_hostname: False}

If you don't want to add the user to password-less `sudo` or if its already there:

    - hosts: servers
      gather_facts: True
      roles:
         - {role: Ansible-LinuxCommon, setup_sudo: False}

## **License**

This playbook is licensed under MIT License (See the LICENSE file).

## **Author**

[Saad Ali](https://github.com/nixknight)
