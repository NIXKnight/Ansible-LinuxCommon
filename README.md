# **LinuxCommon**

LinuxCommon is an Ansible role for configuring shell environment and installing some very basic utilities on a new Linux system. It does the following on a default install:
* Change hostname.
* Replace the host file according to the hostname.
* Replace a pre-modified `/etc/bash.bashrc`, `.profile` and `.bashrc` files for root and user.
* Replace default `sources.list` to include all official repositories e.g. `main`, `contrib`, `non-free` (in case of Debian) and `main`, `restricted`, `universe`, `multiverse`, `partner` (in case of Ubuntu).
* Install updates.
* Installs `htop`, `iotop`, `iftop`, `ncdu`, `ntp`, `ntpdate`, `curl`, `bash-completion`, `vim`, `mtr-tiny`, `git` and `unzip`.
* Purges `exim`.
* configures `vim` editor.
* Installs `libpam-systemd` (only for Debian Jessie. See [this](https://serverfault.com/questions/706475/ssh-sessions-hang-on-shutdown-reboot) thread)
* Check kernel version upgrade and reboots the server if there is a kernel update.

bash.bashrc is a modified version of the default file that comes with Debian. The file includes shell customizations and logging. Shell logs are sent to user.info facility.

## **Requirements**

At the moment this role is being written for Debian based distributions e.g. Debian/Ubuntu. It will evolve to include other major distributions.

## **Role Variables**

There is one variable `change_hostname` in `defaults/main.yml` based on which the role decides wether to change the hostname or not. By default it is set to `True`. If you don't want to change change hostname, you can set it to `False`. See the example below.

The role sets `inventory_hostname_short` as the hostname.

## **Dependencies**

This role doesn't depends on any other role for execution. The role assumes that you have sudo installed. You can also use `su` method to become root if you don't want to install `sudo`.

## **Example Playbook**

Facts gathering must be enabled.

An example of running the role is as follows:

    - hosts: servers
      gather_facts: True
      roles:
         - LinuxCommon

If you don't want to change hostname:

    - hosts: servers
      gather_facts: True
      roles:
         - {role: LinuxCommon, change_hostname: False}

## **License**

This playbook is licensed under MIT License (See the LICENSE file).

## **Author**

[Saad Ali](https://github.com/nixknight)
