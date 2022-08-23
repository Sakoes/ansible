clean-install playbook for rpi
===============================

# Overview
This playbook can be run on a fresh raspbian install. It configures, downloads and runs all tools and packages I like to work with
To be able to run this playbook, the community.docker plugin is needed. It is defined in the requirements.txt file.

# Roles
The playbook executes following roles

## configuration
The configuration role sets the hostname of the pi to _pihome_.
The role also gives a static IP adrress to the Raspberry Pi.

## disk-mount
This role detects an external harddrive of a certain size.
For this drive, it creates an ext4 partition if not present out of the box.
After, it creates a path, mounts the drive to the Raspberry Pi and sets the correct ownership.
It also configures the Pi to mount the drive at startup

## install-tools
This role upgrades apt packages and installs following tools:
- Git
- Python
- jq & yq
- Docker
- Portainer

## [DEPRECATED] run-tools
__These services/tools are not run anymore with docker compose. Instead they are deployed in a k3s cluster with Helm__  
This role runs/starts following services using docker compose. The compose files are defined in the [services](https://github.com/Sakoes/services) repository :
- Docker
- Portainer
- Homer dashboard
- Jenkins
