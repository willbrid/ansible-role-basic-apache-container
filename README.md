Ansible Role : basic apache container
=========

Requirements
------------

No requirements

Role Variables
--------------

This role comes with following variables defined in defaults/main.yml :
system_user: test
webapp_port: 80
file_template: index.html.j2
domain: test

Dependencies
------------

No dependencies

Example Playbook
----------------

For testing this role you should provider at least : system_user and domain variables

- hosts: servers
  vars: 
    system_user: test
    domain: test
  roles:
      - { role: willbrid.ansible-role-basic-apache-container }

License
-------

GPLv3