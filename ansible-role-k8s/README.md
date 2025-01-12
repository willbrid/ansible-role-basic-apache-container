Ansible-role-k8s
================

Ce rôle Ansible configure et installe un cluster Kubernetes multi-nœuds pour les distributions basées sur RedHat (RHEL, CentOS, Rocky Linux) et Debian (Debian, Ubuntu). Il prend en charge la configuration d'un plan de contrôle principal, de nœuds de travail et d'autres plans de conrôle secondaires pour une architecture HA (High Availability).

Exigences
------------

- **Inventaire**

Le fichier d'inventaire doit être organisé pour refléter les différents types de nœuds de votre cluster Kubernetes au travers des différentes valeurs possibles de rôle : **primary_control_plane**, **secondary_control_plane** et **node**.

**Exemple pour un cluster avec 3 noeuds plan de contrôle (1 noeud principal et 2 noeuds secondaire) et 4 noeuds worker**

```
[primary_control_plane]
192.168.1.5

[secondary_control_plane]
192.168.1.6
192.168.1.7

[node]
192.168.1.8
192.168.1.9
192.168.1.10
192.168.1.11
```

- **Variables obligatoires**

Les variables suivantes doivent être définies pour garantir que le rôle fonctionne correctement : 

--- **kubernetes_role** : pour préciser le rôle du noeud à configurer parmi : **primary_control_plane**, **secondary_control_plane** et **node**

--- **kubernetes_control_plane_ip** : pour préciser l'adresse ip du noeud plan de contrôle

--- **kubernetes_control_plane_endpoint** : pour préciser l'adresse ip ou nom dns du endpoint du plan de contrôle.

Description des Variables
----------------------------

- **kubernetes_version** : version de kubernetes à installer **>= 1.28**
- **kubernetes_specific_version** : version spécifique de kubernetes, **Exemple: 1.28.0**
- **kubernetes_role** : rôle du noeud à configurer. Valeurs possibles : **primary_control_plane**, **secondary_control_plane**, **node**
- **kubernetes_control_plane_ip** : adresse ip du noeud plan de contrôle
- **kubernetes_control_plane_endpoint** : adresse ip ou nom dns du endpoint du plan de contrôle
- **kubernetes_cni_network** : variable de configuration du plugin réseau : <br>
--- **kubernetes_cni_network.cni** : nom du plugin réseau. Valeurs possibles : **calico**, **flannel**, **weave** <br>
--- **kubernetes_cni_network.cidr** : plage réseau cidr récommandée par le plugin réseau <br>
--- **kubernetes_cni_network.pod_host_port** : port d'hôte d'intercommunication entre les pods du plugin réseau <br>
--- **kubernetes_cni_network.manifest** : fichier manifest d'installation du plugin réseau
- **kubernetes_control_plane_ports** : ports réseau à autoriser pour le bon fonctionnement du noeud plan de contrôle
- **kubernetes_node_ports** : ports réseau à autoriser pour le bon fonctionnement du noeud worker

**Valeurs par défaut de toutes les variables**

```
kubernetes_version: '1.29'
kubernetes_specific_version: '1.29.0'
kubernetes_role: "primary_control_plane"
kubernetes_control_plane_ip: ""
kubernetes_control_plane_endpoint: ""
kubernetes_cni_network:
  cni: 'calico'
  cidr: '172.16.0.0/16'
  pod_host_port: "179"
  manifest: "https://docs.projectcalico.org/manifests/calico.yaml"
kubernetes_control_plane_ports:
- '6443'
- '2379-2380'
- '10250'
- '10257'
- '10259'
kubernetes_node_ports:
- '10250'
- '10256'
- '30000-32767'
```

Dépendances
-------------

Aucune.

Exemples Playbook
------------------

- Configuration du noeud plan de contrôle

```
---
- hosts: "{{ kubernetes_role }}"
  become: yes

  vars:
  - kubernetes_role: "primary_control_plane"
  - kubernetes_control_plane_ip: "192.168.1.5"
  - kubernetes_control_plane_endpoint: "192.168.1.5"

  roles:
  - ansible-role-k8s
```

- Configuration des noeuds secondaires plan de contrôle

```
---
- hosts: "{{ kubernetes_role }}"
  become: yes

  vars:
  - kubernetes_role: "secondary_control_plane"
  - kubernetes_control_plane_ip: "192.168.1.5"
  - kubernetes_control_plane_endpoint: "192.168.1.5"

  roles:
  - ansible-role-k8s
```

- Configuration des noeuds worker

```
---
- hosts: "{{ kubernetes_role }}"
  become: yes

  vars:
  - kubernetes_role: "node"
  - kubernetes_control_plane_ip: "192.168.1.5"
  - kubernetes_control_plane_endpoint: "192.168.1.5"

  roles:
  - ansible-role-k8s
```

Licence
-------

BSD,MIT

Informations sur l'auteur
------------------

William Bridge NGASSAM