freebsd-jailed-btsync
=========

This role provides a jailed BTSync instance.

The free version of btsync is installed. On the host system a btsync dataset is create, if it not exist already. Within this dataset a dataset for this specific instance is created as well. The web ui will be accessible on the configured port (default: 20202). Use user `admin` is allowed to login. Please set an encrypted password via `btsync_webui_password_hash`. To create one please type `openssl passwd -crypt PASSWORD`.

To see this role in action, have a look at [this project of mine](https://github.com/JoergFiedler/freebsd-ansible-demo).

Requirements
------------

This role is intent to be used with a fresh FreeBSD 10.2 installation. There is a Vagrant Box with providers for VirtualBox and EC2 you may use.

You will find a sample project which uses this role [here](https://github.com/JoergFiedler/freebsd-ansible-demo).

Role Variables
--------------

##### btsync_home
Home diretory for btsync within the jail. Default: `'/srv/btsync'`.

##### btsync_port
The port this btsync instance listens on. This is the hosts external port as well as the port used by btsync within the jail. A pf rdr rule is created to redirect the traffic to the jail. Default: `'10202'`.

##### btsync_webui_port
The port for btsync's web ui. This is the hosts external port as well as the port used by btsync within the jail. A pf rdr rule is created to redirect the traffic to the jail. Default: `'20202'`.

##### btsync_webui_login
The user name allowed to login. Default: `'admin'`.

##### btsync_server_certbundle
The servers certificate as well as required intermediate certificates for the web ui. Default: 'btsync_server_certbundle.pem`.

##### btsync_server_key
The servers key used to secure the web ui. Default: 'btsync_server_key.pem`.

##### btsync_webui_password_hash
The user's password hash. To create a password hash type `openssl passwd -crypt PASSWORD`. Default: `'mfKUcqy7pdU4I'` (passwd).

##### host_zfs_btsync_dataset
The ZFS dataset for btsync on the host. Default: `'{{ host_srv_dataset }}/btsync'`.

##### host_zfs_btsync_dir
The btsync's ZFS dataset mountpoint on the host. Default: `'{{ host_srv_dir }}/btsync'`.

##### btsync_home_host_zfs_dataset
The ZFS dataset for this particular instance. Default: `'{{ host_zfs_btsync_dataset }}/{{ jail_name }}'`.

##### btsync_home_host_zfs_dir
The ZFS dataset mountpoint for this particular instance. Default: `'{{ host_zfs_btsync_dir }}/{{ jail_name }}'`.

Dependencies
------------

- [JoergFiedler.freebsd-jailed](https://galaxy.ansible.com/detail#/role/6599)

Example Playbook
----------------

    - { role: JoergFiedler.freebsd-jailed-btsync,
        tags: ['btsync'],
        use_ssmtp: true,
        use_syslogd_server: true,
        jail_name: 'btsync',
        jail_net_ip: '10.1.0.6' }

License
-------

BSD

Author Information
------------------

If you like it or do have ideas to improve this project, please open an issue on Github. Thanks.
