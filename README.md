WAL-E ansible role
==================

nstalls and configures WAL-E using envdir to store configuration. Sets up a
crontab entry to perform base backups.

Requirements
------------

 * You still need to configure Postgres manually/separately to archive WAL files.
 * You need a backing object store setup (S3, SWIFT, etc)

Role Variables
--------------


Dependencies
------------


Example Playbook
----------------

License
-------

See LICENSE file.

