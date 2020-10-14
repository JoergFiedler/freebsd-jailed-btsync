freebsd-jailed-rslsync
=========

[![Build Status](https://travis-ci.org/JoergFiedler/freebsd-jailed-rslsync.svg?branch=master)](https://travis-ci.org/JoergFiedler/freebsd-jailed-rslsync)

This role provides a jailed Resilio Sync instance. The host should be setup using 

The free version of Resilio Sync is installed. On the host system a resilio dataset is created, if it not exist already. 
The web ui will be accessible via https. Please run `/usr/local/bin/acme-weekly.sh` and `/usr/local/bin/acme-deploy.sh` to get valid certificatess from Let' Encrypt.

Requirements
------------

This role is intent to be used with a fresh FreeBSD installation and [JoergFiedler.freebsd-jail-host](https://galaxy.ansible.com/JoergFiedler/freebsd-jail-host).
There is a Vagrant Box with providers for VirtualBox and EC2 you may use.

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

##### rslsync_webui_port
The port for resilio's web ui within the jail. Default: `'10080'`.

##### rslsync_certificate_aliases
Let's encrypt certificate aliases for webui. Default: `''`.

##### rslsync_key_file
NGINX ssl key file. Another new key file will be generated via acme.sh client. This one is just to allow NGINX to be started successfully after installation. Default: `'localhost-key.pem'`.

##### rslsync_certbundle_file
NGINX ssl cert bundle file. A new bundle file will be generated via acme.sh client. This one is just to allow NGINX to be started successfully after installation. Default: `'localhost-certbundle.pem'`.

##### rslsync_dhparam_file
SSL dh param file. There is a default one to allow easy setups, but make sure to provide unique one for every instance running. Default: `'localhost-dhparam.pem'`

##### host_zfs_rslsync_dataset
The ZFS dataset for Resilio Sync on the host. 
Default: `'{{ host_srv_dataset }}/rslsync'`.

##### host_zfs_rslsync_dir
The Resilio Sync's ZFS dataset mountpoint on the host. 
Default: `'{{ host_srv_dir }}/rslsync'`.

Dependencies
------------

- [JoergFiedler.freebsd-jailed-nginx](https://galaxy.ansible.com/joergfiedler/freebsd-jailed-nginx)

Example Playbook
----------------

    - hosts: all
      become: true
    
      tasks:
        - import_role:
            name: 'JoergFiedler.freebsd-jail-host'
        - include_role:
            name: 'JoergFiedler.freebsd-jailed-rslsync'
          vars:
            jail_net_ip: '10.1.0.10'
            jail_name: 'rslsync'
            jail_build_server_enabled: yes
            jail_build_server_url: 'http://vastland.moumantai.de/public/FreeBSD/packages/freebsd-12_1_x64-branches_2020Q4'
            rslsync_server_fqdn: 'localhost'

License
-------

BSD

Author Information
------------------

If you like it or do have ideas to improve this project, please open an issue on Github. Thanks.
