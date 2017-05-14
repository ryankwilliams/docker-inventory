# ansible-docker-inventory

Ansible dynamic inventory script for Docker.

## Purpose

The purpose of this project was to understand how Ansible's dynamic inventory
functionality worked and to help ease the work needed to use Ansible with docker
containers. This script removes the need to create inventory files each time
you create new containers.

## Requirements

* Ansible
* Docker
* Python
* Python virtualenv (optional)

## Install Python virtualenv

Install Python virtualenv if you would like to install the required Python
packages in an isolated environment. This is an optional step.
```Bash
$ pip install virtualenv
```

## Install required Python packages

Install the Python packages declared inside the projects requirements.txt.
```Bash
$ (venv) pip install -r requirements.txt
```

## Run

This assumes that you have docker running on your system and have containers
running.

You will need to either fork the project or clone the project to get a local
copy of the python script on your system. If you do not want to declare the
inventory file (-i) in each ansible call, you can move the script to
/etc/ansible (give permissions of 0755). In the examples below, we will be
declaring the inventory file.
```Bash
# Running container
$ (venv) docker ps -n 1
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
48e5b06946e4        fedora:latest       "/bin/bash"         56 seconds ago      Up 5 seconds                            demo

# List container by specific name
$ (venv) ~/ansible-docker-inventory/src/docker_inventory.py --host demo
{"all": {"hosts": ["demo"]}, "_meta": {"hostvars": {"demo": {"ansible_connection": "docker"}}}}

# List all containers
$ (venv) ~/ansible-docker-inventory/src/docker_inventory.py --list
{"all": {"hosts": ["demo"]}, "_meta": {"hostvars": {"demo": {"ansible_connection": "docker"}}}}

# Ping container
$ (venv) ansible demo -m ping -i ~/git/ansible-docker-inventory/src/docker_inventory.py
demo | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

# Run a shell command in the container
$ (venv) ansible demo -m shell -a "cat /etc/fedora-release" -i ~/ansible-docker-inventory/src/docker_inventory.py
demo | SUCCESS | rc=0 >>
Fedora release 25 (Twenty Five)

```

## Note

Your container will need to have Python installed in order to have Ansible
communicate to it.
