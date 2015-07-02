WAL-E ansible role
==================

nstalls and configures WAL-E using envdir to store configuration. Sets up a
crontab entry to perform base backups.

Requirements
------------

 * You still need to configure Postgres manually/separately to archive WAL files.
 * By default uses the user `postgres`, which should be created previously, i.e.
   run the `postgres` role before this one.
 * You need a backing object store setup (S3, SWIFT, etc)

Role Variables
--------------

This is the list of variables used in this role and their defaults

AWS configuration (required):

 * `wal_e_s3_prefix` set the bucket url and prefix where store the values.
    More info in [WAL-E Documentation](https://github.com/wal-e/wal-e#backend-blob-store)

 * `wal_e_aws_instance_profile: false` Set it to `true` to enable IAM instance profiles.
   If enabled, wal_e_aws_access_key will be ignored
   More info in [WAL-E Documentation](https://github.com/wal-e/wal-e#using-aws-iam-instance-profiles)

   **IMPORTANT**: Your postgres configuration `archive_command` must also include this option.

 * `wal_e_aws_access_key` and `wal_e_aws_secret_key` AWS S3 credentials

Installation options:

 * `wal_e_version: 0.7.0` version of WAL-E to install
 * `wal_e_pgdata_dir: '/var/lib/postgresql/9.1/main/'` location of the backup data


Default backup job defined in cron:

 * `wal_e_base_backup_disabled: false` set `true` to disable automatic
    setup of this cron job or not
 * `wal_e_base_backup_minute:   '0'`
 * `wal_e_base_backup_hour:     '0'`
 * `wal_e_base_backup_day:      '*'`
 * `wal_e_base_backup_month:    '*'`
 * `wal_e_base_backup_weekday:  '1'`

 * `wal_e_base_backup_options: ''` Additional options for this job

Postgres user. WAL-E must be allow to read the WAL files:

 * `wal_e_user: 'postgres'`
 * `wal_e_group: 'postgres'`

Dependency packages and other options:

 * `wal_e_packages`: list of package dependencies
 * `wal_e_pips`: : list of package dependencies
 * `wal_e_envdir: '/etc/wal-e'` config directory


Dependencies
------------

Postgres user must be created.

Example Playbook
----------------

```
---
- hosts: all
  sudo: yes
  roles:
	wal_e_aws_access_key: 'MY_ACCESS_KEY'
	wal_e_aws_secret_key: 'MY_SECRET_KEY'
```

How to test this playbook?
--------------------------

There is a `Vagrantfile` and `site.yml` which allow test the playbook with
vagrant. Just execute:

```
vagrant up # first provision
vagrant provision # to rerun ansible
```

License
-------

See LICENSE file.

