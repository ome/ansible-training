---
title: "Playbooks and Roles"
teaching: 5
exercises: 0
objectives:
- "Exposure to playbooks"
keypoints:
- "Bringing together hosts and roles"
---

### What's a playbook?

Can be as simple as a sequence of tasks. 

Example - from your clone of [omero-deployment-examples](https://github.com/ome/omero-deployment-examples):

~~~
$ cd omero-deployment-examples/ansible-example-omero-public-user/
$ cat playbook.yml
~~~
{: .bash}
~~~
---
# Install OMERO.server and OMERO.web with a public user on localhost

- hosts: all
  roles:

    - role: ome.postgresql
      postgresql_databases:
        - name: omero
      postgresql_users:
        - user: omero
          password: omero
          databases: [omero]
      postgresql_version: "9.6"

    - role: ome.omero_server
      ice_python_wheel: "https://github.com/ome/zeroc-ice-py-centos7/releases/\
        download/0.1.0/zeroc_ice-3.6.4-cp27-cp27mu-linux_x86_64.whl"
      omero_server_release: latest

    - role: ome.omero_web
      ice_python_wheel: "https://github.com/ome/zeroc-ice-py-centos7/releases/\
        download/0.1.0/zeroc_ice-3.6.4-cp27-cp27mu-linux_x86_64.whl"
      omero_web_release: latest
      omero_web_config_set:
        omero.web.public.enabled: true
        omero.web.public.server_id: 1
        omero.web.public.user: public
        omero.web.public.password: "{{ omero_web_public_password }}"
        omero.web.public.url_filter: "^/(webadmin/myphoto/|webclient/\
          (?!(action|logout|annotate_(file|tags|comment|rating|map)|\
          script_ui|ome_tiff|figure_script))|\
          webgateway/(?!(archived_files|download_as)))"


    # This role only works on OMERO 5.3+
    - role: ome.omero_user
      omero_user_bin_omero: /opt/omero/server/OMERO.server/bin/omero
      omero_user_system: omero-server
      omero_user_admin_user: root
      omero_user_admin_pass: omero
      omero_group_create:
        - name: demo
          type: read-only
      omero_user_create:
        - login: public
          firstname: public
          lastname: user
          password: "{{ omero_web_public_password }}"
          groups: "--group-name demo"

  vars:
    omero_web_public_password: public
    ## PRODUCTION VARIABLES: For a production system,
    ## remove "_DISABLED" from the following property
    ## in order to scale up for more users. Disabling
    ## permits testing on smaller VMs.
    omero_server_config_set_DISABLED:
      omero.db.poolsize: 50
      omero.jvmcfg.percent.blitz: 50
      omero.jvmcfg.percent.indexer: 20
      omero.jvmcfg.percent.pixeldata: 20
~~~
{: .output}

[`playbook.yml`](https://github.com/ome/ansible-example-omero-public-user/blob/4ed8a3ba4aee4ab409f6905add7e23aaf17b7f98/playbook.yml), uses roles such as [ome.postgresql](https://github.com/ome/ansible-role-postgresql) and [ome.omero-server](https://github.com/ome/ansible-role-omero-server).

The full list of official OME Ansible roles can be found in [OME Ansible Galaxy](https://galaxy.ansible.com/ome).

{% include links.md %}
