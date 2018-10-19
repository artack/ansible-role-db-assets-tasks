# Ansible DB & ASSETS Tasks

Adds DB & ASSETS Tasks to any project like pull, push and synchronize. Based on Ansible. This project is heavily inspired by its [Ruby Version](https://github.com/sgruhier/capistrano-db-tasks).

Currently only supported database is MySQL & MariaDB.

## Status
As per now only `db:pull` and `db:push` are supported. Assets task are to come soon!

## Install
Words about ansible-galaxy, tbc...

## Update
Words about ansible-galaxy, tbc...

## Setup
### Inventory
Define two hosts (`dat_remote` and `dat_local`) in a group `dat`. Use whatever connection settings are needed in your very own setup (i use vagrant here).

```ini
[dat]
dat_remote  ansible_host=remote-host.example.com    ansible_user=remote-user    ansible_port=1337
dat_local   ansible_host=192.168.0.100              ansible_user=vagrant        ansible_ssh_pass=vagrant
```

### Playbook
````yaml
- name: DAT Example playbook
  hosts: dat # The group from the inventory file

  vars_prompt:
    - name: dat_action
      prompt: "Enter an action to perform"
      private: no

  roles:
    - { role: roles/ansible-db-assets-tasks } // TODO: Replace with galaxy role
````

## Variables
````yaml
# To remove the local dump after using
dat_db_local_clean: true

# To remove the remote dump after using
dat_db_remote_clean: true

# To exclude tables from the dump
dat_db_ignore_tables: [] // NOT yet supported

# To exclude content (but still get the schema) from the dump
dat_db_ignore_data_tables: [] // NOT yet supported

# To specify the temp directory wher the dump is saved
dat_db_dump_dir: /tmp/.db

# To disallow any push to the remote (maybe productive) server
dat_disallow_pushing: true // NOT yet supported

# To specify the local & remote dirs to be synced - ATTENTION: Must be in the VERY SAME order!
dat_assets_local_dir: []  // NOT yet supported
dat_assets_remote_dir: []  // NOT yet supported
````

## Available Tasks
```
db:pull      # Synchronize your local database using remote database data
db:push      # Synchronize your remote database using local database data
```
