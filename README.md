Ansible Role: restic
====================

Install and configure restic backup.


Requirements
------------

None.

Role Variables
--------------
```
# Install restic
# Default true
restic_enable: true

# Config path
restic_config_path: /etc/restic

# where filelist is saved
restic_filelist: '{{ restic_config_path }}/filelist'

# Where restic env variables are stored
restic_env: '{{ restic_config_path }}/env'

# Link the env file to the /etc/profile.d directory.
# This make available enviroment vars to all users for
# cli operation. Default is true, if false you need
# to source the env file before operating with restic
# in command line.
restic_env_profile: true

# Repository type
# Supported types are:
# - local
# - aws
restic_repository_type: local

restic_repository_url: /tmp/repo

# Repository password
# You are strongly encouraged to use a strong password 
# and save it in a safe place.
# Consider that you will never be asked to type it.
restic_repository_password: changeme

# For aws repository: it is mandatory to
# set restic_aws_access_key_id and
# restic_aws_secret_access_key.
# No default is provided.
# restic_aws_access_key_id: none
# restic_aws_secret_access_key: none

# If true, repository will be initialized
# during the setup.
# Please consider this operation is not
# idempotent, calling init on a repository
# already initialized produces an error.
restic_init_repository: false

# List of files/directories to backup
restic_backup_files:
  - /etc

# Backup tag. Used to identify our backups, Used to identify our backups, 
# you need to change it if the repository is shared with other installations.
restic_backup_tag: "ansible_managed"

#Backup scheduling
restic_backup_scheduled: true
restic_backup_minute: "0"
restic_backup_hour: "8-22"
restic_backup_month: "*"
restic_backup_weekday: "*"

#Forget scheduling
restic_forget_scheduled: true
restic_forget_policy: "--keep-daily 7"
restic_forget_minute: "30"
restic_forget_hour: "22"
restic_forget_month: "*"
restic_forget_weekday: "*"

#Prune scheduling
restic_prune_scheduled: true
restic_prune_minute: "0"
restic_prune_hour: "23"
restic_prune_month: "*"
restic_prune_weekday: "*"

# Backup log. 
# consider configuring logrotate accordingly.
restic_backup_log: /var/log/restic_backup.log-$(date +"\%Y\%d\%m")
restic_forget_log: /var/log/restic_forget.log-$(date +"\%Y\%d\%m")
```


Dependencies
------------

None


Example Playbook
----------------
```
---
- hosts: localhost
  roles:
    - role: msarti.restic
      vars:
        restic_backup_files: 
          - /etc
          - /home/msarti
          - /usr/local
          - /var/spool/cron
        restic_repository_url: s3:https://s3.amazonaws.com/example
        restic_repository_password: some_strong_password
        restic_aws_access_key_id: xxxxxxxxxxx
        restic_aws_secret_access_key: xxxxxxxxxxxxxx
        restic_repository_type: aws
        restic_init_repository: false
        restic_backup_minute: "0"
        restic_backup_hour: "8-17"
        restic_backup_month: "*"
        restic_backup_weekday: "*"
        restic_forget_policy: "--keep-daily 7"
        restic_forget_minute: "15"
        restic_forget_hour: "17"
        restic_forget_month: "*"
        restic_forget_weekday: "*"
        restic_prune_minute: "30"
        restic_prune_hour: "17"
        restic_prune_month: "*"
        restic_prune_weekday: "*"
```

License
-------

BSD

