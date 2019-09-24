---
title: "Ansible within OME"
teaching: 10
exercises: 0
objectives:
- "Learn about Ansible use within OME"
keypoints:
- "Inventory"
- "prod-playbooks"
- "testing: travis (in Docker), molecule"
---


### OME Ansible Roles

The [public user example playbook](https://github.com/ome/ansible-example-omero-public-user) is mostly just a list of the roles with associated configuration parameters that we want to be run against our Vagrant machine.

OME have an ever growing set of roles which were used to build the [IDR](https://idr.openmicroscopy.org/), and production systems like the [OMERO demo server](https://demo.openmicroscopy.org/) and Nightshade, the School of Life Science's OMERO server.

If you want to find out more see these links:
- [Ansible roles on GitHub](https://github.com/search?utf8=%E2%9C%93&q=org%3Aome+ansible-role&type=)
- [Ansible roles on Galaxy](https://galaxy.ansible.com/ome)
- [IDR deployment playbooks](https://github.com/IDR/deployment/)
- [OME production playbooks](https://github.com/ome/prod-playbooks)

### Testing

* Most OME Ansible roles are tested using [Molecule](https://github.com/ome/ome-ansible-molecule).


{% include links.md %}
