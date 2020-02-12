.. _section-role-puppeteer:

Role **puppeteer**
================================================================================

.. note::

    This documentation page and role itself is still work in progress.

* `Ansible Galaxy page <https://galaxy.ansible.com/honzamach/puppeteer>`__
* `GitHub repository <https://github.com/honzamach/ansible-role-pupeteer>`__
* `Travis CI page <https://travis-ci.org/honzamach/ansible-role-pupeteer>`__

Ansible role for installation of server providing shared Git repositories. It
may be used as central repository hosting node for the `MSMS <https://github.com/honzamach/msms>`__
system, hence the name (master of puppets ;-))

Currently the resulting state on the target host is very simple. Repositories
are created within preconfigured directory and ownership is given to specific
system group. There is a system account for each user that is allowed to access
any repository. Each user is assigned to one or more system groups according to
the list of repositories he/she is allowed to access. Finally each repository
is linked to home directories of all users that have been granted access.

**Table of Contents:**

* :ref:`section-role-puppeteer-installation`
* :ref:`section-role-puppeteer-dependencies`
* :ref:`section-role-puppeteer-usage`
* :ref:`section-role-puppeteer-variables`
* :ref:`section-role-puppeteer-files`
* :ref:`section-role-puppeteer-author`

This role is part of the `MSMS <https://github.com/honzamach/msms>`__ package.
Some common features are documented in its :ref:`manual <section-manual>`.


.. _section-role-puppeteer-installation:

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


.. _section-role-puppeteer-dependencies:

Dependencies
--------------------------------------------------------------------------------


This role is dependent on following roles:

* :ref:`accounts <section-role-accounts>`

No other roles have direct dependency on this role.


.. _section-role-puppeteer-usage:

Usage
--------------------------------------------------------------------------------

Example content of inventory file ``inventory``::

    [servers_puppeteer]
    your-server

Example content of role playbook file ``role_playbook.yml``::

    - hosts: servers_puppeteer
      remote_user: root
      roles:
        - role: honzamach.puppeteer
      tags:
        - role-puppeteer

Example usage::

    # Run everything:
    ansible-playbook --ask-vault-pass --inventory inventory role_playbook.yml

It is recommended to follow these configuration principles:

* Use files ``inventory/host_vars/[your-server]/vars.yml`` to customize settings
  for particular servers. Please see section :ref:`section-role-puppeteer-variables`
  for all available options. Example::

        # Define three users with access to same shared repository with 'user1'
        # being the primary owner of the repository.
        hm_accounts__users:
          user1:
            groups:
              - user1
          user2:
            groups:
              - user1
          user3:
            groups:
              - user1

        hm_puppeteer__repositories:
          reponame: user1


.. _section-role-puppeteer-variables:

Configuration variables
--------------------------------------------------------------------------------


Internal role variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. envvar:: hm_puppeteer__install_packages

    List of packages defined separately for each linux distribution and package manager,
    that MUST be present on target system. Any package on this list will be installed on
    target host. This role currently recognizes only ``apt`` for ``debian``.

    * *Datatype:* ``dict``
    * *Default:* (please see YAML file ``defaults/main.yml``)
    * *Example:*

    .. code-block:: yaml

        hm_puppeteer__install_packages:
          debian:
            apt:
              - git
              - ...

.. envvar:: hm_puppeteer__repositories_dir

    Path do directory to which shared repositories will be placed.

    * *Datatype:* ``string``
    * *Default:* ``"/var/git"

.. envvar:: hm_puppeteer__home_subdir

    Subdirectory of home directory of each user to which to link repositories
    he is allowed to access. Path must begin with ``/``.

    * *Datatype:* ``string``
    * *Default:* ``"/git"

.. envvar:: hm_puppeteer__repositories

    List of shared Git repositories.

    * *Datatype:* ``dictionary``
    * *Default:* ``{}``
    * *Example:*

    .. code-block:: yaml

        hm_puppeteer__repositories:
          reponame: name


Foreign variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

:envvar:`hm_accounts__users`

    User database will be used to fill in contact information for service administrators.


.. _section-role-puppeteer-files:

Managed files
--------------------------------------------------------------------------------

This role does not manage content of any files on target system.


.. _section-role-puppeteer-author:

Author and license
--------------------------------------------------------------------------------

| *Copyright:* (C) since 2019 Jan Mach <jan.mach@cesnet.cz>, CESNET, a.l.e.
| *Author:* Jan Mach <jan.mach@cesnet.cz>, CESNET, a.l.e.
| Use of this role is governed by the MIT license, see LICENSE file.
|
