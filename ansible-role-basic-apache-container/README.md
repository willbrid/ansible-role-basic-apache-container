Role Ansible : basic apache container
======================================

Ce rôle est fourni avec les variables suivantes définies dans **defaults/main.yml** :

```
system_user: test
domain: test
webapp_container_name: "webapp"
webapp_container_port: 80
webapp_file_index_template: index.html.j2
apache_image_version: "latest" # httpd image version
apache_port: 80
apache_website_folder: "/usr/local/apache2/htdocs"
```

Exemple Playbook
----------------

Pour tester ce rôle, vous devez fournir au moins : variable **system_user**

```
- hosts: servers
  
  vars: 
    system_user: test
    domain: test
  
  roles:
  - ansible-role-basic-apache-container
```

Pour effectuer un test avec le fichier de test intégré dans ce rôle, il est nécessaire de fournir au minimum les variables **system_user** et **domain**.