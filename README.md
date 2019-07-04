enix.proxmox_ve
=================

A role for deploying and configuring [Proxmox VE](https://www.proxmox.com/en/proxmox_ve) on unix hosts using [Ansible](http://www.ansible.com/).
This role is really tied to enix usage by configuring local LVM volumes, ISCSI multi-path, etc... So it is maybe not relevant to everyone, however every step is configurable so can be used independently


Requirements
------------

Supported targets:

- Debian 8 "Jessie"
- Debian 9 "Stretch"


Role Variables
--------------

This roles comes preloaded with almost every available default. You can override each one in your hosts/group vars, in your inventory, or in your play. See the annotated defaults in `defaults/main.yml` for help in configuration. All provided variables start with `proxmox_ve__`.

- `proxmox_ve__force_reboot` - :warning: **Caution** :warning:. In case of important configuration changes this will automatically reboot the host. default: false.
- `proxmox_ve__enterprise` - enable or not the enterprise subscription for Proxmox VE. default: false.
- `proxmox_ve__disable_smt` - disable SMT (Hyperthreading) as a boot kernel option. See [https://www.kernel.org/doc/html/latest/admin-guide/hw-vuln/l1tf.html#smt-control] for details about associated flaws. default: false.
- `proxmox_ve__net_ovs` - enable OpenVswitch network configuration on host, default: false.
- `proxmox_ve__net_template` - template used for `/etc/network/interfaces` configuration on the host, default: interfaces.j2. The path can be either changed or overloaded in your playbook. The default template only provide a basic bridge configuration.
- `proxmox_ve__storage_lvm` - description of lvm storage to initialise and configure in proxmox. exemple configuration above.
- `proxmox_ve__lvm_global_filter` - lvm global_filter. default: `[ "r|/dev/zd.*|", "r|/dev/mapper/pve-.*|" ]`. If using LVM inside VMs this can lead to issues. So it must be set to `[ "r|/dev/zd.*|", "r|/dev/mapper/pve-.*|", "r|/dev/[^/]+/vm-[0-9]+-disk-.*|" ]`.
- `proxmox_ve__storage_iscsi` - description of iscsi storage to configure in proxmox. exemple configuration above.
- `proxmox_ve__storage_iscsi_options` - options to change in iscsid.conf. default:
```
proxmox_ve__storage_iscsi_options:
  - option: node.session.timeo.replacement_timeout
    value: 10
    state: present
```
- `proxmox_ve__storage_iscsi_multipath_template` - template file to use for multipath configuration.


Dependencies
------------

- None

Usage
-----

Clone this repo into your roles directory:

    $ git clone https://gitlab.enix.org/ansible/ansible-proxmox_ve.git roles/proxmox_ve

Or use Ansible galaxy requirements.yml

    # public role
    - src: enix.proxmox_ve
      name: proxmox_ve

And add it to your play's roles:

    - hosts: all
      roles:
        - role: enix.proxmox_ve
          proxmox_ve__storage_iscsi:
            - name: iscsi-storage
              portal: 192.168.0.1
              target: iqn.2015-11.com.storage:iscsi.12315132
              volumes:
                - name: bigvolume
                  wwid: 3600c0ff0003bb7fcb730e75a01000000
          proxmox_ve__storage_lvm:
            - name: "localvm"
              devices:
                - /dev/md12
              pesize: "128"
              shared: 0
            - name: "iscsilvm"
              devices:
                - /dev/mapper/bigvolume
              pesize: "256"
              shared: 1

You can also use the role as a playbook. You will be asked which hosts to provision, and you can further configure the play by using `--extra-vars`.

    $ ansible-playbook -i inventory --extra-vars='{...}' main.yml

Still to do
-----------

- auto add hosts to clusters
- manage users and credentials


Changelog
---------

### 2.2.0

Add proxmox_ve__disable_smt option

### 2.1.1

Switch to enix.deb role_name

### 2.1.0

Allow override of lvm global_filter
Add configuration of /etc/hosts

### 2.0.0

Now include automatic host reboot feature, proper storage manangement


### 1.0.0

First version that include basic storage configuration.

License
-------

GPLv2

Author Information
------------------

Laurent CORBES <laurent.corbes@enix.fr> - http://www.enix.fr
