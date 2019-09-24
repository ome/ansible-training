---
title: "Trying it out"
teaching: 25
exercises: 0
questions:
objectives:
- "Try our some basic Ansible code"
keypoints:
- "Run simple shell commands"
- "Add more hosts inventory files"
- "Gather data from multiple hosts in one command"
---

This episode describes the tools we use to build and manage lessons.
These simplify many tasks, but make other things more complicated.

## Trying it out

Let's create a folder to get started.
~~~
$ mkdir ~/ansible-training && cd ~/ansible-training
~~~
{: .bash}

#### 1. Module: Ping

~~~
$ ansible localhost -m ping
~~~
{: .bash}

~~~
 [WARNING]: Host file not found: /usr/local/etc/ansible/hosts

 [WARNING]: provided hosts list is empty, only localhost is available

localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
~~~
{: .output}

#### 2. Module: Setup

~~~
$ ansible localhost -m setup 
~~~
{: .bash}
~~~
..... woah!
~~~
{: .output}

Trying again: what info are we interest in? e.g. CPU cores:

~~~
$ ansible localhost -m setup | grep processor_cores
~~~
{: .bash}
~~~
 [WARNING]: Host file not found: /usr/local/etc/ansible/hosts
 [WARNING]: provided hosts list is empty, only localhost is available
        "ansible_processor_cores": "4",
~~~
{: .output}

> ## What's with all these [WARNING] statements?!
> 
> Ansible is looking for an inventory file - let's create one.
>
{: .callout}

#### 3. Inventory Files

Here's we'll create an inventory file for eel and cowfish 

~~~
$ echo localhost >> inventory-file
$ cat inventory-file
~~~
{: .bash}
~~~
localhost
~~~
{: .output}

Rather than typing `yes` for every host, `ssh-keyscan` takes care of that.
~~~
$ cat inventory-file | grep -vP '[\[#]'  | grep -P '\.' | sort | uniq | while read h; do ssh-keyscan $h >> ~/.ssh/known_hosts; done
~~~
{: .bash}

Trying your first [ad-hoc command](http://docs.ansible.com/ansible/latest/intro_adhoc.html):
~~~
$ ansible all -i inventory-file -a "/bin/echo hello"
~~~
{: .bash}


A real-world example: nginx versions
~~~
$ ansible all -i inventory-file -m shell -a 'rpm -q nginx'
~~~
{: .bash}



Further reading at [ansible.com - getting started](http://docs.ansible.com/ansible/latest/intro_getting_started.html)

{% include links.md %}
