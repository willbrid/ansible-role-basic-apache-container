# Ansible-role-basic-apache-container

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/willbrid/ansible-role-basic-apache-container/blob/main/LICENSE) [![CI](https://github.com/willbrid/ansible-role-basic-apache-container/actions/workflows/ci.yml/badge.svg)](https://github.com/willbrid/ansible-role-basic-apache-container/actions/workflows/ci.yml)

The **ansible-role-basic-apache-container** role allows you to deploy a basic apache container from the **httpd** image and host a static web page **index.html** which can be redefined.

## Requirements

This role relies heavily on the **geerlingguy.pip** role, which is automatically run upstream to install the **Python docker** package on the target hosts, necessary for the proper functioning of the **community.docker.docker_container** module.

## Description des Variables

|Name|Type|Description|Required|Default value|
|----|----|-----------|--------|-------------|
`bac_container_name`|str|name of the web application container to install|yes|`""`
`bac_container_port`|int|port of the web application container to publish|no|`80`
`bac_container_mount_dir`|str|Ppath to the web application hosting directory|yes|`""`
`bac_file_index_template`|str|jinja template of the index.html page|no|`"index.html.j2"`
`bac_apache_image_version`|str|tag of the httpd image to use|no|`"latest"`
`bac_apache_website_folder`|str|directory to host the web application inside the container|no|`"/usr/local/apache2/htdocs"`

## Dependancies

The **geerlingguy.pip** role must be installed beforehand via the **ansible-galaxy** command.

```bash
ansible-galaxy role install geerlingguy.pip
```

The **community.docker** collection must be installed beforehand via the **ansible-galaxy** command.

```bash
ansible-galaxy collection install community.docker
```

> Note: Target systems must have `Docker` installed. For this, you can, for example, use the `geerlingguy.docker` role, which itself requires the `community.general` collection.

## Example Playbook

- Role installation

```bash
mkdir -p $HOME/install-basic-apache-container
```

```bash
vim $HOME/install-basic-apache-container/requirements.yml
```

```yaml
- name: ansible-role-basic-apache-container
  src: git+https://github.com/willbrid/ansible-role-basic-apache-container.git
  version: v0.0.1
```

```bash
cd $HOME/install-basic-apache-container && ansible-galaxy install --force -r requirements.yml
```

> Note: It is assumed that a `hosts.ini` file (in the `$HOME/install-basic-apache-container` directory) is defined, containing the inventory of `webapp` group servers using `Debian` or `RedHat` distributions.

- Using the role in a playbook

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

## License

MIT

## Information about the authors

William Bridge NGASSAM
