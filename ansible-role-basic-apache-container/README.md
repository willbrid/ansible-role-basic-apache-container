Ansible Role : basic apache container
=========

Ce rôle est fourni avec les variables suivantes définies dans **defaults/main.yml** :

```
system_user: test
webapp_port: 80
file_template: index.html.j2
domain: test
```

Example Playbook
----------------

Pour tester ce rôle, vous devez fournir au moins : variable **system_user**

```
- hosts: servers
  vars: 
    system_user: test
  roles:
      - ansible-role-basic-apache-container
```

Pour tester avec le test intégré dans ce rôle, vous devez fournir au moins : les variables **system_user** et **domain**.