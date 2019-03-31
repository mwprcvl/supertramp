# Installing Docker on a Virtual Machine

## Purpose

Docker is a tool for managing containers. I wanted to set up a server with Docker installed. This is useful because I want a way to play with workflows involving servers.


## Prerequisites

VirtualBox. An Ubuntu VM provisioned using Vagrant, as described [here](../docs/1_vagrant.md). Launch an Ubuntu VM using Vagrant and `vagrant ssh` into it.


## Installing Docker

The steps to install Docker on Ubuntu are described here: https://docs.docker.com/install/linux/docker-ce/ubuntu/. First there are some things to install and we should select a release of Docker to install.

```
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
apt-cache madison docker-ce
```

Having reviewed the available versions in the second column of the prior output, I have selected `18.09` and this is reflected in the install.

```
export VERSION_STRING='5:18.09.1~3-0~ubuntu-bionic'
sudo apt-get install \
    docker-ce=${VERSION_STRING} docker-ce-cli=${VERSION_STRING} containerd.io
sudo docker run hello-world
```

There are some post-install steps necessary, described here:
https://docs.docker.com/install/linux/linux-postinstall/. The crux of it is that some changes are required so that you do not have to type `sudo` before every `docker` command.

```
vagrant ssh
sudo groupadd docker
sudo usermod -aG docker $USER
exit
```

Now confirm that Docker runs and make it start on boot.

```
vagrant ssh
docker run hello-world
sudo systemctl enable docker
```


## Packaging the VM as an artifact for sharing

Finally, package the resulting VM into an artifact that can be uploaded to Vagrant Cloud.

```
vagrant halt
vagrant package
```

I then upload this `pacakge.box` file to vagrant cloud as a new project, e.g., `mwprcvl/ubuntu_docker`.

The uploaded `.box` is hosted [here](https://app.vagrantup.com/mwprcvl/boxes/ubuntu_docker).

## Using the new VM

This artifact can then be used in future, e.g.:

```
mkdir dev_ubuntu_docker
cd ./dev_ubuntu_docker
vagrant init mwprcvl/ubuntu_docker --box-version 1
vagrant up
```


## Summary

I have demonstrated how to install Docker on a Linux VM. I have shown how to create an image of this VM and how to share it on Vagrant Cloud. The next thing to do is understand how to provision the VM using Ansible, as this is a further step toward reproducibility.
