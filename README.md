eNiXHosting.proxmox-ve
=================

A role for deploying and configuring [Proxmox VE](https://www.proxmox.com/en/proxmox-ve) on unix hosts using [Ansible](http://www.ansible.com/).
This role is really tied to eNiXHosting usage by configuring local LVM volumes, ISCSI multi-path, etc... So it is maybe not relevant to everyone, however every step is configurable so can be used independently


Requirements
------------

Supported targets:

- Debian 8 "Jessie"
- Debian 9 "Stretch"


Role Variables
--------------

This roles comes preloaded with almost every available default. You can override each one in your hosts/group vars, in your inventory, or in your play. See the annotated defaults in `defaults/main.yml` for help in configuration. All provided variables start with `proxmox_ve__`.

- `proxmox_ve__enterprise` - enable or not the enterprise subscription for Proxmox VE. default: false

Dependencies
------------

- None

Usage
-----

Clone this repo into your roles directory:

    $ git clone https://gitlab.enix.org/ansible/ansible-proxmox-ve.git roles/proxmox-ve

Or use Ansible galaxy requirements.yml

    # public role
    - src: eNiXHosting.proxmox-ve
      name: proxmox-ve

And add it to your play's roles:

    - hosts: all
      roles:
        - role proxmox-ve:
            proxmox_ve__var: true

You can also use the role as a playbook. You will be asked which hosts to provision, and you can further configure the play by using `--extra-vars`.

    $ ansible-playbook -i inventory --extra-vars='{...}' main.yml

Still to do
-----------

- Add proxmox-ve debian repository
- Install proxmox-ve software suite
- auto add hosts to clusters
- Install and configure OpenVswitch
- Configure network using playbook template
- Create and configure local LVM vg (using raid10)
- Install and configure ISCSI target volume + LVM vg


Changelog
---------

### 0.1

Initial version.

License
-------

GPLv2

Author Information
------------------

$your $name <$your.$name@enix.fr> - http://www.enix.fr
