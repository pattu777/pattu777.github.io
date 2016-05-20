---
layout: post
title: Learning Ansible - Installing packages on a remote server.
comments: true
tags:
  - Ansible
  - Linux
  - Devops
  - Cloud server
---

Recently I bought a droplet from DigitalOcean. Luckily I also got the first two months free of charge. Now the next step was to configure the server. As I am currently learning Ansible, in this post I will describe how to execute certain tasks on a remote server using it.

# Ansible
> It's a free-software platform for configuring and managing computers, combines multi-node software deployment, ad hoc task execution, and configuration management.[^1]

In simplest terms, it's a command line tool to execute any command(or set of commands) on your remote server from your local system. Now some cool facts about Ansible.

* Open Source. Yay.
* Written in Python.
* Uses YAML files for configuration.
* Executes commands on remote servers using SSH channels.(Paramiko module in Python).
* It has a huge collection of modules.(System, cloud, networking etc.,).
* Fairly easy to write custom modules.

# Prerequisites
* A local system with Ansible installed.
* IP address or the hostname of a remote server.
* SSH public key of the local system copied onto the remote server.
* A remote user with sudo privilege.

In my case, both of my systems are debian based(Ubuntu 14.04). Let's write a playbook to perform the following tasks.

# Task
* Install some softwares on the remote server.

# Steps

### Create an inventory file.
An inventory file is nothing but a simple YAML file which contains information about the remote server. The only information we need about the server is it's IP address or hostname. Alternatively we can also specify multiple servers in one group and execute a set of plays on them. A simple inventory file is as shown below.

```yml
[server]
host.example.org

[test]
host2.example.org
host3.example.org

[localserver]
127.0.0.1
```

### A playbook to install packages.
A playbook consists of different plays and each play can contain multiple tasks. Let's create a playbook to install some packages on our remote Ubuntu server.

```yml
---
- hosts: server
  remote_user: Chinmaya
  become: yes
  tasks:
    - name: Install packages
      apt: name={{ item }} state=present
      with_items:
          - git-core
          - vim
          - python-pip
          - python-virtualenv
```
Now let's further examine the playbook.

* The *hosts* part contains the name of a server group from our inventory file.
* And *remote_user* contains name of a user of the remote server.
* The *become* keyword tells us to run the commands with sudo privilege.
* Next in our task, we use an Ansible module called __apt__ to install some packages. The play goes through all the package names under *with_items* part in a loop and installs them on the server.

### Executing the playbook
Now our directory contains two files, an inventory file(hosts) and an Ansible playbook(setup.yml).

```bash
$ ansible-playbook -i hosts setup.yml

PLAY *******************************************************

TASK [setup] ***********************************************
ok: [host.example.org]

TASK [Install packages] ************************************
changed: [host.example.org] => (item=[u'git-core', u'vim', u'python-pip', u'python-virtualenv'])

PLAY RECAP *************************************************
host.example.org       : ok=2    changed=1    unreachable=0    failed=0  
```

The code can be found on my github repository [^2].

[^1]: [https://en.wikipedia.org/wiki/Ansible_(software)](https://en.wikipedia.org/wiki/Ansible_(software))
[^2]: [https://github.com/pattu777/Ansible-Playground](https://github.com/pattu777/Ansible-Playground)
