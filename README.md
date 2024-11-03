# Ansible Roles Tools

Ce dépôt contient une collection de rôles Ansible développés pour automatiser et standardiser différentes configurations et déploiements d'infrastructure.

### Description

Ce projet a pour objectif de fournir des rôles Ansible modulaires et faciles à utiliser pour simplifier la gestion d’infrastructure et les tâches d'automatisation. Chaque rôle est conçu pour être réutilisable, maintenable et adaptable à divers environnements.

### Prérequis

**Ansible version minimale recommandée** : **ansible version 2**
**Systèmes supportés** : famille **Redhat** et famille **Debian**

### Utilisation

1. Clonez le dépôt

```
git clone https://github.com/willbrid/ansible-roles-tools.git
```

2. Ajoutez les rôles dans votre fichier playbook

```
- hosts: all

  roles:
    - role: chemin_vers_role_1
    - role: chemin_vers_role_2
```

3. Exécutez votre playbook

```
ansible-playbook -i hosts playbook.yml
```

### Contributions

Les contributions sont les bienvenues !