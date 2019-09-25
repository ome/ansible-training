---
layout: page
title: Setup
permalink: /setup/
---

#### This training session is in the form of an instructor-led workshop.

To follow along, you'll need the some prerequisites:

#### Hardware Requirements:
* 2 GB ram free
* 2 GB local disk free

#### Software Requirements (links/instructions below):
* Ansible (2.6+)
* Virtualbox (6+)
* Vagrant (2.2+)

#### Git clones of:
* [https://github.com/ome/omero-deployment-examples](https://github.com/ome/omero-deployment-examples)

#### Network Requirements:
* You must have access to the public internet.

### Installing the requirements

Shell commands and output are shown as follows:
~~~
$ example command
~~~
{: .bash}
~~~
example output
~~~
{: .output}

### Ansible
The installation instructions for all platforms are at [docs.ansible.com](http://docs.ansible.com/ansible/latest/intro_installation.html). 

OS X users: The Ansible project are currently recommending users install it via `pip`. I've read that using `--user` can make installation easier on 10.11+, so perhaps try `pip install --user ansible`.

You can test your install by running:
~~~
$ ansible localhost -m ping
~~~
{: .bash}
~~~
localhost | SUCCESS => {
    "changed": false,
        "ping": "pong"
        }
~~~
{: .output}

### Ansible Source and related files

The [omero-deployment-examples](https://github.com/ome/omero-deployment-examples) GitHub repository contains examples of deploying OMERO with Ansible on CentOS 7 or Docker.

~~~
$ git clone --recurse https://github.com/ome/omero-deployment-examples
~~~
{: .bash}

Make git clones of the following repositories, in your favourite clones directory, e.g. ~/clones/.



### VirtualBox 
A lightweight method of running virtual machines on your desktop/laptop. Follow the graphical installer you can download from [virtualbox.org](https://www.virtualbox.org/wiki/Downloads). Once that completes, VirtualBox should be ready to use.

### Vagrant

This is a tool which can automate the configuration of local virtual machines, including VirtualBox. With this we can create a VM from a text definition in one command.

1 Follow the graphical installer you can download from [vagrantup.com](https://www.vagrantup.com/downloads.html).

2 Once vagrant is installed, download the `centos/7` `box` as follows:

Run (in any directory):
~~~
$ vagrant box add centos/7 # When prompted, choose 3 `virtualbox` 
~~~
{: .bash}
~~~
==> box: Loading metadata for box 'centos/7'
    box: URL: https://vagrantcloud.com/centos/7
    This box can work with multiple providers! The providers that it
    can work with are listed below. Please review the list and choose
    the provider you will be working with.

    1) hyperv
    2) libvirt
    3) virtualbox
    4) vmware_desktop

    Enter your choice: 3
~~~
{: .output}
~~~
==> box: Adding box 'centos/7' (v1710.01) for provider: virtualbox
    box: Downloading: https://vagrantcloud.com/centos/boxes/7/versions/1710.01/providers/virtualbox.box
        box: Progress: 24% (Rate: 9.9M/s, Estimated time remaining: 0:00:27)
==> box: Successfully added box 'centos/7' (v1710.01) for 'virtualbox'!
~~~
{: .output}

Confirm the `box` was installed with:
~~~
$ vagrant box list | grep centos/7
~~~
{: .bash}
~~~
centos/7         (virtualbox, 1710.01)
~~~
{: .output}

> ## Problems getting set up?
>
> Ask in slack, and someone will help.
{: .keypoints}

{% include links.md %}



