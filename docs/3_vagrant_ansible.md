# Provisioning a VM with Docker using Ansible

## Purpose

Ansible is a tool for provisioning Virtual Machines. This tool is compatible with Vagrant. The combination means that I can develop an instruction list for installing Docker on an Ubuntu server VM. This is useful because I want to be able to document server setup in a script that I can reuse.

## Prerequisites

VirtualBox. An Ubuntu VM provisioned using Vagrant, as described [here](../docs/1_vagrant.md).

## Getting started

I installed Ansible using `conda`, from the `conda-forge` channel. I installed it in its own conda environment.

```
conda create --name ansi python=3.6
source activate ansi
conda search -c conda-forge ansible
conda install -c conda-forge ansible=2.7.5
```

## Existing example found

An excellent writeup is [here](https://ops.tips/blog/docker-ansible-role/). I decided to modify it to use the versions of Ubuntu and Docker that I wanted.

```
git clone https://github.com/cirocosta/example-docker-ansible.git dev_docker_ansible
cd ./dev_docker_ansible
```

## Modify for my purpose

I needed to make a few changes to support the OS and Docker versions I wanted.

In the `vagrant/Vagrantfile`: 

```
config.vm.box = "mwprcvl/ubuntu_server"

```

In the `roles/docker/defaults/main.yml`:

```
docker_version: '5:18.09.1~3-0~ubuntu-bionic'
docker_group_members: 
  - 'vagrant'
```

## Packaging the VM as an artifact for sharing

Finally, package the resulting VM into an artifact that can be uploaded to Vagrant Cloud.

```
vagrant halt
vagrant package
```

I then upload this `package.box` file to vagrant cloud as a new project, e.g., `mwprcvl/docker_ansible`. The uploaded `.box` is hosted [here](https://app.vagrantup.com/mwprcvl/boxes/docker_ansible).

## Using the new VM

This artifact can then be used in future, e.g.:

```
mkdir dev_docker_server
cd ./dev_docker_server
vagrant init mwprcvl/docker_ansible --box-version 1
vagrant up
```

## Summary

I have demonstrated the use of Ansible to provision a VM with Docker installed. I have shared this provisioned VM on Vagrant Cloud and given directions on how to use it.