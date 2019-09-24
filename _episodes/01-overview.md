---
title: "Overview of terms"
teaching: 10
exercises: 0
objectives:
- "Learn "
keypoints:
- "Ansible - configuration automation"
- "Ansible inventory - our "
- "Vagrant - drives local VM provider like VirtualBox"
---

We'll start with a rough overview of:

* Ansible
* Inventory file
* Vagrant

#### Ansible

* Like ssh, but on can run against >1 at once.
* Agent-less
* Human-readable `YAML` language to define state of a system

#### Inventory (files)

* List(s) of systems
* Groups systems together

Looks like:

~~~
[omero-servers]
omero.example.org
omero2.example.org

[web-servers]
www.example.org
~~~
{: .source}

#### Vagrant

* Takes care of creating and managing virtual machines
* Uses a file called `Vagrantfile` to configure the virtual machine
* For the purposes of this training use a local virtual machine

#### Virtualbox

* Lightweight Virtual Machine hypervisor

{% include links.md %}
