# Getting started with Vagrant and Linux VMs

## Purpose

Vagrant is a tool for managing virtual machines. Vagrant Cloud hosts virtual box images for download. Using vagrant makes virtual machine setup simple and reproducible. I am interested in this technology so that I can create a virtual server for replicating a production workflow.


## Prerequisites

VirtualBox.


## Getting started

Install vagrant using the downloadable dmg. Create an account so that you can upload vagrant compatible virtual box images. Find a VirtualBox image for an operating system of interest, in my case Ubuntu Server. I decided on this image from [here](https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64-vagrant.box)


## Releasing a `.box` from Vagrant Cloud

Using the web interface, I created a new Vagrant box named `ubuntu_server`, added a VirtualBox provider, and provided a link to the provider listed above. I then released the box. If you doubt the stability of the provider, Vagrant Cloud will also host the image for you: just download the `.box` and then upload it.


## Using the `.box` with Vagrant

To use the hosted `.box` with Vagrant, instructions are provided in the web interface. Assuming I am the desired directory, the commands will create a local Vagrantfile, download the `.box`, and launch the VM.

```
vagrant init mwprcvl/ubuntu_server --box-version 1
vagrant up
```


## Using the VM

Login to the VM. The default user is `vagrant`

```
vagrant ssh
```

## Stopping the VM

To pause and continue using the VM:

```
vagrant suspend
vagrant resume
```

To stop the VM:

```
vagrant halt
```


## Note on Virtual Box Guest Additions

I received a warning that host and guest had different version of VirtualBox Guest Additions:

```
The guest additions on this VM do not match the installed version of
VirtualBox! In most cases this is fine, but in rare cases it can
cause things such as shared folders to not work properly. If you see
shared folder errors, please update the guest additions within the
virtual machine and reload your VM.
```

The answer [here](https://stackoverflow.com/questions/20308794/how-to-upgrade-to-virtualbox-guest-additions-on-vm-box) recommends:

```
vagrant plugin install vagrant-vbguest
```

For me, this made the warning go away, at the cost of a long boot time. In some cases more configuration might be required, as documented [here](https://github.com/dotless-de/vagrant-vbguest).


## Summary

I have demonstrated use of Vagrant to manage a VM. I have shown the use of Vagrant Cloud to host VirtualBox images.
