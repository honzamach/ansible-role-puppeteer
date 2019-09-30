.. _section-role-puppeteer:

Role **puppeteer**
================================================================================


Ansible role for installation of central node for custom server management system.

* `Ansible Galaxy page <https://galaxy.ansible.com/honzamach/puppeteer>`__
* `GitHub repository <https://github.com/honzamach/ansible-role-pupeteer>`__
* `Travis CI page <https://travis-ci.org/honzamach/ansible-role-pupeteer>`__


Description
--------------------------------------------------------------------------------


This role is responsible for provisioning of **Puppeteer** server.


Dependencies
--------------------------------------------------------------------------------


This role is dependent on following roles:

* :ref:`accounts <section-role-accounts>`

No other roles have direct dependency on this role.


Installation
--------------------------------------------------------------------------------

To install the role `honzamach.puppeteer <https://galaxy.ansible.com/honzamach/puppeteer>`__
from `Ansible Galaxy <https://galaxy.ansible.com/>`__ please use variation of
following command::

    ansible-galaxy install honzamach.puppeteer

To install the role directly from `GitHub <https://github.com>`__ by cloning the
`ansible-role-puppeteer <https://github.com/honzamach/ansible-role-puppeteer>`__
repository please use variation of following command::

    git clone https://github.com/honzamach/ansible-role-puppeteer.git honzamach.puppeteer

Currently the advantage of using direct Git cloning is the ability to easily update
the role when new version comes out.


Example Playbook
--------------------------------------------------------------------------------

Example content of inventory file ``inventory``::

    [servers_puppeteer]
    localhost

Example content of role playbook file ``playbook.yml``::

    - hosts: servers_puppeteer
      remote_user: root
      roles:
        - role: honzamach.puppeteer
      tags:
        - role-puppeteer

Example usage::

    # Run everything:
    ansible-playbook -i inventory playbook.yml


License
--------------------------------------------------------------------------------

MIT


Author Information
--------------------------------------------------------------------------------

Jan Mach <honza.mach.ml@gmail.com>
