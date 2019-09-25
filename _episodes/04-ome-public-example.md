---
title: "Creating an OMERO server"
teaching: 20
exercises: 15
objectives:
- "Experience the installation of an OMERO server via Ansible"
keypoints:
- "OME publish example OMERO server playbooks"
- "Vagrantfile defines your VM, including NAT port mappings"
- "We can ask Vagrant about ports and ssh-keys at the CLI"
---
OME have published (and maintain) a set of example playbooks which use the same roles OME use in production which will build an OMERO server.

Here we're going to execute one of those examples - the "public user" example.

If you haven't aleady cloned the GitHub repository with the deployment examples:

~~~
$ git clone --recurse https://github.com/ome/omero-deployment-examples
~~~
{: .bash} 

Change to the `ansible-example-omero-public-user` directory in the clone of `omero-deployment-examples`:
~~~
$ cd omero-deployment-examples/ansible-example-omero-public-user/
~~~
{: .bash} 

Install the prerequisite roles, defined in [`requirements.yml`](https://github.com/ome/ansible-example-omero-public-user/blob/4ed8a3ba4aee4ab409f6905add7e23aaf17b7f98/requirements.yml):
~~~
$ ansible-galaxy install -r requirements.yml -p roles
~~~
{: .bash}

The Vagrantfile will create a CentOS 7 virtual machine with port-forwarding so you can access the deployed server.

~~~
$ cat Vagrantfile
~~~
{: .bash} 
~~~
# -*- mode: ruby -*-

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  config.vm.provider "virtualbox" do |vb|
    config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true
    config.vm.network "forwarded_port", guest: 443, host: 8443, auto_correct: true
    config.vm.network "forwarded_port", guest: 4063, host: 4063, auto_correct: true
    config.vm.network "forwarded_port", guest: 4064, host: 4064, auto_correct: true
  end

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = "2"
  end

  # Uncomment this to automatically run Ansible
  # config.vm.provision "ansible" do |ansible|
  #   ansible.playbook = "playbook.yml"
  #   ansible.galaxy_role_file = "requirements.yml"
  # end
end
~~~
{: .language-ruby}

Tell Vagrant to create and start the VM.
If this is the first time you've run this it may take several minutes to download the VM image.
~~~
$ vagrant up
~~~
{: .bash} 

## Connecting to the virtual machine
Vagrant takes care of simultaneously running one or more machines by assigning the VMs different port numbers for SSH for example.

To obtain the SSH configuration for your VM:
~~~
$ vagrant ssh-config > ssh-config
$ cat ssh-config
~~~
{: .bash}
~~~
Host default
  HostName 127.0.0.1
  User vagrant
  Port 2200
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /omero-deployment-examples/ansible-example-omero-public-user/.vagrant/machines/default/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
~~~
{: .output}

Note the VM is called `default`.

SSH into the VM using this configuration file:
~~~
ssh -F ssh-config default
~~~
{: .bash}
~~~
[vagrant@localhost ~]$ exit
~~~
{: .output}


## Create an inventory file for the local VM:
Create a file called `inventory.yml` with the following
~~~
all:
  hosts:
    default:
      ansible_ssh_common_args: "-F ssh-config"
~~~
{: .yaml}


Verify Ansible can connect to the VM using the `ping` or `setup` module again.
~~~
ansible -i inventory.yml all -m ping
~~~
{: .bash}
~~~
default | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
~~~
{: .output}

## Deploy OMERO:
Finally run the playbook.
This will take around 15 minutes depending on your computer.

~~~
$ ansible-playbook -i inventory.yml playbook.yml
~~~
{: .bash}
~~~
...

PLAY RECAP *********************************************************************
default                    : ok=100  changed=65   unreachable=0    failed=0
~~~
{: output}

Now we should be able to connect to our local Vagrant-driven VM.

Open up a web browser, and connect to [http://localhost:8080](http://localhost:8080)

You should be automatically logged in as the [OMERO.web public user](https://docs.openmicroscopy.org/omero/latest/sysadmins/public.html).

If this port was already taken when we issued `vagrant up`, [Vagrant should have 
auto-corrected and picked another port](https://www.vagrantup.com/docs/networking/forwarded_ports.html#auto_correct). We can ask for the current port mappings as follows:

~~~
$ vagrant port
~~~
{: .bash}
~~~
The forwarded ports for the machine are listed below. Please note that
these values may differ from values configured in the Vagrantfile if the
provider supports automatic port collision detection and resolution.

    22 (guest) => 2222 (host)
  4063 (guest) => 4063 (host)
  4064 (guest) => 4064 (host)
    80 (guest) => 8080 (host)
   443 (guest) => 8443 (host)
~~~
{: .output}

## Rerun the playbook:
A well-written Ansible playbook is *idempotent*, this means you can run it multiple times and it will only make changes to the server if necessary:
~~~
$ ansible-playbook -i inventory.yml playbook.yml
~~~
{: .bash}
~~~
...

PLAY RECAP *********************************************************************
default                    : ok=90   changed=0    unreachable=0    failed=0
~~~
{: .output}

{% include links.md %}
