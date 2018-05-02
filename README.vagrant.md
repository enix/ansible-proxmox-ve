Test ansible role using Vagrant
=====================

Ansible is a great tool, and trying things out with a virtual-machine is a
great way to learn, try things out, and test changes. But if you're trying
to learn Ansible and Vagrant/Virtualbox all at once, configuring a test-setup
just right can be a steep learning curve. This template demonstrates how
to set-up Vagrant to test your new Ansible role.


tl;dr:
------

To startup tests simply run:

    cd vagrant/
    vagrant up

In case you want to relaunch the tests, restart vagrant provision
    vagrant provision

For debugging purpose vagrant provide an easy way to connect inside the VM
    vagrant ssh


Notes
------
*Vagrantfile* sets up a testing virtual-machine,
by default named `debianjessie64` (see [Vagrantfile](vagrant/Vagrantfile)).
This should be renamed to something sensible for your role so you can
recognise it if you have multiple VM's up and running, e.g. when running::

    vagrant global-status

... to remind yourself where all your RAM went.

The *ansible.cfg* file tells ansible how to find the Vagrant SSH login
details, which makes it easy to run Ansible manually against your
VM via the command line, rather than indirectly
by re-running Vagrant commands - this can speed up testing.

Requirements
------------

[Vagrant-cachier](http://fgrehm.viewdocs.io/vagrant-cachier/) is recommended,
but optional.

To retrieve a copy of the roles listed in a requirements file, run::

    ansible-galaxy install -r requirements.yml

or

    ansible-galaxy install --force -r requirements.yml

to forcibly update old copies.

Note that *[ansible.cfg](vagrant/ansible.cfg)* is configured such that
roles installed via Ansible Galaxy will be installed under
*vagrant/galaxy_roles*.

When documenting a role, you should either specify expected
pre-requisites (e.g. git) in the README, or if your dependencies
are provided by a specific role then you should record it in the
role metadata ([see docs](https://galaxy.ansible.com/intro#dependencies)).

License
-------
GPLv2
