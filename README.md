# Ansible-role-basic-apache-container

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/willbrid/ansible-role-basic-apache-container/blob/main/LICENSE) [![CI](https://github.com/willbrid/ansible-role-basic-apache-container/actions/workflows/ci.yml/badge.svg)](https://github.com/willbrid/ansible-role-basic-apache-container/actions/workflows/ci.yml)

Le rôle **ansible-role-basic-apache-container** permet de déployer un conteneur Apache à partir de l'image **httpd** et d'y héberger une page web statique **index.html** qui peut être redéfinie.

## Exigences

Ce rôle repose fortement sur le rôle **geerlingguy.pip**, qui est automatiquement exécuté en amont afin d’installer le package **Python docker** sur les hôtes cibles, nécessaire au bon fonctionnement du module **community.docker.docker_container**.

## Description des Variables

|Nom|Type|Description|Obligatoire|Valeur par défaut|
|---|----|-----------|-----------|-----------------|
`bac_container_name`|str|nom du conteneur de l'application web à installer|oui|`""`
`bac_container_port`|int|port du conteneur de l'application web à publier|non|`80`
`bac_container_mount_dir`|str|chemin du repertoire d'hébergement de l'application web|oui|`""`
`bac_file_index_template`|str|template jinja de la page index.html|non|`"index.html.j2"`
`bac_apache_image_version`|str|tag de l'image httpd à utiliser|non|`"latest"`
`bac_apache_website_folder`|str|repertoire d'hébergement de l'application web à l'intérieur du conteneur |non|`"/usr/local/apache2/htdocs"`

## Dépendances

Le rôle **geerlingguy.pip** doit être préalablement installé via la commande **ansible-galaxy**.

```bash
ansible-galaxy role install geerlingguy.pip
```

La collection **community.docker** doit être préalablement installée via la commande **ansible-galaxy**.

```bash
ansible-galaxy collection install community.docker
```

> Note: Les systèmes cibles doivent avoir `Docker` installé. Pour cela, vous pouvez par exemple utiliser le rôle `geerlingguy.docker` qui lui même, nécessite la collection `community.general`.

## Exemple Playbook

- Installation du rôle

```bash
mkdir -p $HOME/install-basic-apache-container
```

```bash
vim $HOME/install-basic-apache-container/requirements.yml
```

```yaml
- name: ansible-role-basic-apache-container
  src: https://github.com/willbrid/ansible-role-basic-apache-container.git
  version: v0.0.1
```

```bash
cd $HOME/install-basic-apache-container && ansible-galaxy install --force -r requirements.yml
```

> Note: On suppose qu’un fichier `hosts.ini` (dans le repertoire `$HOME/install-basic-apache-container`) est défini, contenant l’inventaire des serveurs de groupe `webapp` utilisant des distributions `Debian` ou `RedHat`.

- Utilisation du rôle dans un playbook

```bash
vim $HOME/install-basic-apache-container/playbook.yml
```

```yaml
---
- hosts: webapp
  vars:
    bac_container_name: "webapp"
    bac_container_port: 8080
    bac_container_mount_dir: "/opt/webapp"
    bac_file_index_template: index.html.j2
    bac_apache_image_version: "latest"
    bac_apache_website_folder: "/usr/local/apache2/htdocs"

  roles:
    - ansible-role-basic-apache-container
```

```bash
cd $HOME/install-basic-apache-container && ansible-playbook -i hosts.ini playbook.yml
```

## Licence

MIT

## Informations sur l'auteur

William Bridge NGASSAM
