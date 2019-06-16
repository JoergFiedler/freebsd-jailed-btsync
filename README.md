freebsd-jailed-rslsync
=========

[![Build Status](https://travis-ci.org/JoergFiedler/freebsd-jailed-rslsync.svg?branch=master)](https://travis-ci.org/JoergFiedler/freebsd-jailed-rslsync)

This role provides a jailed Resilio Sync instance.

The free version of Resilio Sync is installed. On the host system a resilio dataset
is create, if it not exist already. Within this dataset a dataset for this 
specific instance is created as well. The web ui will be accessible on the 
configured port. Use user `admin` is allowed to login. Please set an 
encrypted password via `rslsync_webui_password_hash`. To create one please 
type `openssl passwd -crypt PASSWORD`.

Requirements
------------

This role is intent to be used with a fresh FreeBSD installation. There is a 
Vagrant Box with providers for VirtualBox and EC2 you may use.

Role Variables
--------------

##### rslsync_home
Home diretory for resilio within the jail. Default: `'/srv/rslsync'`.

##### rslsync_home_host_zfs_dataset
The ZFS dataset for this particular instance. 
Default: `'{{ host_zfs_rslsync_dataset }}/{{ jail_name }}'`.

##### rslsync_home_host_zfs_dir
The ZFS dataset mountpoint for this particular instance. 
Default: `'{{ host_zfs_rslsync_dir }}/{{ jail_name }}'`.

##### rslsync_port
The port this resilio instance listens on. This is the hosts external port as 
well as the port used by Resilio Sync within the jail. A pf rdr rule is created to 
redirect the traffic to the jail. Default: `'10100'`.

##### rslsync_tarsnap
Backup Resilio Sync instance's data using tarsnap. Default: '`false`'.

Needs tarsnap enabled on the host system.

##### rslsync_webui_login
The user name allowed to login. Default: `'admin'`.

##### rslsync_webui_password_hash
The user's password hash. To create a password hash type `openssl passwd -crypt PASSWORD`. 
Default: `'mfKUcqy7pdU4I'` (admin).

##### rslsync_webui_port
The port for resilio's web ui. This is the hosts external port as well as the 
port used by Resilio Sync within the jail. A pf rdr rule is created to redirect the 
traffic to the jail. Default: `'10080'`.

##### host_zfs_rslsync_dataset
The ZFS dataset for Resilio Sync on the host. 
Default: `'{{ host_srv_dataset }}/rslsync'`.

##### host_zfs_rslsync_dir
The Resilio Sync's ZFS dataset mountpoint on the host. 
Default: `'{{ host_srv_dir }}/rslsync'`.

Dependencies
------------

- [JoergFiedler.freebsd-jailed](https://galaxy.ansible.com/joergfiedler/freebsd-jailed)

Example Playbook
----------------

    - hosts: all
      become: true

      vars:
        ansible_python_interpreter: '/usr/local/bin/python2.7'
        - import_role:
            name: 'JoergFiedler.freebsd-jail-host'
        - include_role:
            name: 'JoergFiedler.freebsd-jailed-rslsync'
          vars:
            jail_net_ip: '10.1.0.10'
            jail_name: 'rslsync'
            jail_freebsd_release: '11.2-RELEASE'
            jail_build_server_enabled: yes
            jail_build_server_url: 'http://vastland.moumantai.de/FreeBSD/packages/freebsd-11_2_x64-branches_2018Q4'

License
-------

BSD

Author Information
------------------

If you like it or do have ideas to improve this project, please open an issue on Github. Thanks.
