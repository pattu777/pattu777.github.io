---
layout: post
title: Ansible playbooks to deploy a Django project - Part 1
comments: true
---

Two weeks back, I started learning Django. I built a `polls` app by following the tutorial from Django's website[^1]. Later I also built a blog using it. My code is available at [https://github.com/pattu777/LearningDjango](https://github.com/pattu777/LearningDjango).

But I also wanted to know how a Django project is typically deployed on a remote server. I read a couple of blogs about the deployment process[^2]. And I thought of automating it using Ansible.

So I ended up writing a bunch of Ansible playbooks to accomplish this. I bought a domain name `learningdjango.in` from GoDaddy and a cheap server from Digital Ocean to host my project. Please use my [__referral link__](https://m.do.co/c/9373dbe909b4) if you want to sign up with Digital Ocean. So in this post I will describe

# Prerequisites
* A remote server.
* Ansible installed on your local system.
* Passwordless setup for a user with sudo privilleges.
* A Django project copied onto the remote server.
* For simplicity I am using SQLite as my back-end database.
* A custom domain pointing to your cloud server(Optional). This domain name is used while configuring Nginx.

# Architecture
First and foremost let's see how our deployment architecture looks like. So we have our Django project on a remote server. We will use __`Gunicorn`__ as our app server. In development we often use Django's builtin server. But in production, it's not advisable to use it. We will run __`Gunicorn`__ on port __`8000`__.

Next we will use __`Nginx`__ as a reverse proxy server. Let me explain what it'll do. From browser when we visit [http://learningdjango.in](http://learningdjango.in), the browser will send a HTTP request to our server. Now __`Nginx`__ will accept this request and simply forward it to our __`Gunicorn`__ server running at port __`8000`__. Apart from this, __`Nginx`__ will also serve all the static(CSS, JS files) and media assets of our Django project. We will provide path to our static files in __`Nginx`__ config file. We will run __`Nginx`__ on port __`80`__.

Now let's create a new directory to save our playbooks.

```bash
$ pwd
/home/user
$ mkdir Ansible-Django
```

# Initial setup

Inside our directory we will have to create 3 files.

* An inventory file called `hosts`.

```bash
$ cat hosts
[server]
<IP-address-of-our-remote-server>
```

* An ansible config file

```
$ cat ansible.cfg
# Config file for ansible
# =======================

[defaults]
inventory	= ./hosts
remote_user = <user_name>
host_key_checking = no
private_key_file = ~/.ssh/id_rsa

become = yes
become_method = sudo
become_user = <user_name>

retry_files_enabled = no
retry_files_save_path	= ~/.ansible-retry
```

* The main ansible YAML file `deploy.yml`.

```
$ cat deploy.yml
---

- name: Ansible playbook to deploy a Django app
  hosts: server

  roles:
    - core
    - django
    - services
```

As seen above, `deploy.yml` file contains three roles.

* The role `core` will handle package installation.
* The role `django` will execute Django specific tasks such as migration, collection of static files etc.
* The role `services` will configure and run gunicorn and Nginx.

Now create individual directories for each of the roles as shown below. Don't worry about the YAML files in tasks and vars folder inside each roles. We will talk about them later. I have also added a `README` and a `LICENSE` file.

```bash
$ tree
.
├── ansible.cfg
├── deploy.yml
├── hosts
├── LICENSE
├── README.md
└── roles
    ├── core
    │   ├── tasks
    │   │   └── main.yml
    │   └── vars
    │       └── main.yml
    ├── django
    │   ├── tasks
    │   │   └── main.yml
    │   └── vars
    │       └── main.yml
    └── services
        ├── handlers
        │   └── main.yml
        ├── tasks
        │   └── main.yml
        ├── templates
        │   ├── gunicorn.j2
        │   └── nginx.j2
        └── vars
            └── main.yml

12 directories, 14 files

```

Each role contains multiple directories.

* `tasks` - contains the main playbook.
* `vars` - contains a YAML file with user defined variables.
* `templates` - Jinja templates used in plays.
* `handlers` - Playbooks which are triggered once a certain task is completed from the main YAML file from `tasks` directory.

# Install necessary packages.

In our first role `core`, we will install some system packages on our server. The list of package names are present in `core/vars/main.yml` file.

```bash
$ cat core/vars/main.yml
---
# Packages to be installed
system_packages:
  - build-essential
  - git
  - libevent-dev
  - nginx
  - python-dev
  - python-pip
  - python-virtualenv
  - python-setuptools
  - python-software-properties
  - openssh-server
  - libffi-dev
  - sqlite3
```

In `core/tasks/main.yml`, we will write a task that will loop through these packages and install them one by one. I am assuming the remote server is Debian based as I am using `apt` module in our playbook. Otherwise you can use `yum` to install the packages.

```YAML
---

- name: Update cache
  apt: update_cache=yes

- name: Install system packages
  apt: pkg={{ item }} update_cache=yes state=present
  with_items:
      - {% raw %}"{{ system_packages }}"{% endraw %}

- name: Update pip
  pip: name=pip state=latest
```

That's it for this post. I will talk about configuring our Django project and setting up Nginx and gunicorn in a new post. So stay tuned.

[^1]: [https://docs.djangoproject.com/en/1.9/intro/tutorial01/](https://docs.djangoproject.com/en/1.9/intro/tutorial01/)
[^2]: [https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-14-04](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-14-04)
