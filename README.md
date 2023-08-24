# Ansible role: labocbz.install_apacheds

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: ApacheDS](https://img.shields.io/badge/Tech-ApacheDS-orange)
![Tag: LDAP](https://img.shields.io/badge/Tech-LDAP-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)
![Tag: Cluster](https://img.shields.io/badge/Tech-Cluster-orange)

An Ansible role to install and configure ApacheDS on your host.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
install_apacheds_version: "2.0.0.AM26"
install_apacheds_user: "apacheds"
install_apacheds_group: "apacheds"

install_apacheds_install_dir: "/usr/share/apacheds"
install_apacheds_path: "{{ install_apacheds_install_dir }}/apacheds-{{ install_apacheds_version }}"
install_apacheds_ssl_path: "{{ install_apacheds_path }}/ssl"

install_apacheds_ssl: true
install_apacheds_ssl_crt: "{{ install_apacheds_ssl_path }}/mycert.crt"
install_apacheds_ssl_key: "{{ install_apacheds_ssl_path }}/mycert.key"
install_apacheds_ssl_p12_password: "password"

install_apacheds_instances:
  - name: "local"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_prepare_host_users:
  - login: "apacheds"
    group: "apacheds"

inv_install_apacheds_version: "2.0.0.AM26"
inv_install_apacheds_user: "apacheds"
inv_install_apacheds_group: "apacheds"

inv_install_apacheds_install_dir: "/usr/share/apacheds"
inv_install_apacheds_path: "{{ install_apacheds_install_dir }}/apacheds-{{ install_apacheds_version }}"

inv_install_apacheds_ssl_path: "{{ inv_install_apacheds_path }}/ssl"
inv_install_apacheds_ssl: true
inv_install_apacheds_ssl_crt: "{{ inv_install_apacheds_ssl_path }}/my-apacheds-cluster.domain.tld/my-apacheds-cluster.domain.tld.pem.crt"
inv_install_apacheds_ssl_key: "{{ inv_install_apacheds_ssl_path }}/my-apacheds-cluster.domain.tld/my-apacheds-cluster.domain.tld.pem.key"
inv_install_apacheds_ssl_p12_password: "password"

inv_install_apacheds_instances:
  - name: "local"

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_apacheds"
    tags:
    - "labocbz.install_apacheds"
    vars:
    install_apacheds_version: "{{ inv_install_apacheds_version }}"
    install_apacheds_user: "{{ inv_install_apacheds_user }}"
    install_apacheds_group: "{{ inv_install_apacheds_group }}"
    install_apacheds_install_dir: "{{ inv_install_apacheds_install_dir }}"
    install_apacheds_path: "{{ inv_install_apacheds_path }}"
    install_apacheds_instances: "{{ inv_install_apacheds_instances }}"
    install_apacheds_ssl_path: "{{ inv_install_apacheds_ssl_path }}"
    install_apacheds_ssl: "{{ inv_install_apacheds_ssl }}"
    install_apacheds_ssl_crt: "{{ inv_install_apacheds_ssl_crt }}"
    install_apacheds_ssl_key: "{{ inv_install_apacheds_ssl_key }}"
    install_apacheds_ssl_p12_password: "{{ inv_install_apacheds_ssl_p12_password }}"
    ansible.builtin.include_role:
    name: "labocbz.install_apacheds"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-08-21: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-08-23: Started and default instance tested

* Role can now install and copy the default instance to another one
* No changement in the configuration for now
* Tested with ldapsearch command

### 2023-08-24: SSL Available and fixs

* Role can now create a JKS for SSL remote access
* You can now deploy a default instance and then do your stuff inside
* Added some fix about lint

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
