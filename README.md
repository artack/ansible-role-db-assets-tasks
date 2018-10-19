# Ansible DB & ASSETS Tasks
Adds DB & ASSETS Tasks to any project like pull, push and synchronize, based on Ansible. This project is heavily inspired by its [ruby version](https://github.com/sgruhier/capistrano-db-tasks).

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
  vars:
  
    dat_db_name: xmas-web
    dat_assets_dir: # paths must end with a directory separator (/)
      -
        local: /vagrant/files/xmas/
        remote: ~/public_html/shared/files/xmas/

  vars_prompt:
    - name: dat_action
      prompt: "Enter an action to perform ({{ dat_allowed_tasks|join(' | ') }})"
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

# To specify the temp directory wher the dump is saved
dat_db_temp_dir: /tmp/.db

# To disallow any push to the remote (maybe productive) server
dat_disallow_pushing: true // NOT yet supported

# To specify the local & remote dirs to be synced - ATTENTION: Must end with a directory separator (/)
dat_assets_dir: []
````

## Available Tasks
```
app:pull    # Synchronize your local database & assets using remote data
app:push    # Synchronize your remote database & assets using local data

assets:pull # Synchronize your local assets using remote assets data
assets:push # Synchronize your remote assets using local assets data

db:pull     # Synchronize your local database using remote database data
db:push     # Synchronize your remote database using local database data
```
