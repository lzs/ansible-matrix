# Ansible Playbook for Installing Matrix-Synapse

This is an Ansible playbook for installing Synapse (Matrix-Synapse), along with Nginx, MySQL, as well as
ancillary tools and configuration.

Matrix-Synapse is a reference implementation of Matrix (https://matrix.org/), an open network
for secure, decentralised communication. In order to support "larger scale" deployments where performance
is a concern, Synapse is configured using
[workers](https://github.com/matrix-org/synapse/blob/develop/docs/workers.md) whicih splits out functionality
into multiple separate python processes. This allows workers to use mutiple processors, where the default
monolithic mode is unable to exploit.

PostgreSQL is used as the backing database.

## Setup

Setup your hosts.ini inventory file as needed. Refer to hosts.sample.ini.

Modify vars/config.yml as needed.

Put your database password in var/vault.yml. Refer to vault.sample.yml for a sample.

You also may want to look in homeserver.yaml.j2, and remove the customisation in room_list_publication_rules and alias_creation_rules, or just change them to suit your needs.

Install/setup the database with:

```
ansible-playbook -i hosts.ini install_db.yml
```

Install/setup Matrix, Nginx and everything else:

```
ansible-playbook -i hosts.ini install_all.yml
```

Go to the Matrix host, and run:

```
register_new_matrix_user -c /etc/matrix-synapse/homeserver.yaml -u admin -a http://localhost:8008
```

to register your admin user.

## To-Do

Add Synapse-Admin web administration UI.
