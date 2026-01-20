Upgrading Sulu 2.x
==================

This upgrade guide describes how to upgrade a Sulu 2.x project to any newer version below 3.0. In the majority of cases,
these upgrades are straightforward because backwards compatibility is only broken when necessary to
fix a bug.

Independent of Sulu changes, upgrades sometimes require updating PHP, Symfony, or other dependencies first. The following table
shows which versions of Sulu are compatible with which versions of PHP and Symfony. It is
recommended to avoid large upgrades that tackle Sulu, PHP, and Symfony simultaneously. Instead, upgrade them and release 
the updates in small, separate steps. This approach helps to better identify
the cause of any problems.


.. list-table:: Sulu, Symfony, PHP Version support
   :header-rows: 1

   * - Sulu Version
     - supported PHP Versions
     - supported Symfony Versions
   * - 2.0
     - 7.2 - 7.4
     - 4.3 - 4.4
   * - 2.1
     - 7.2 - 8.0
     - 4.3 - 5.4
   * - 2.2
     - 7.2 - 8.1
     - 4.3 - 5.4
   * - 2.3
     - 7.2 - 8.1
     - 4.4 - 5.4
   * - 2.4
     - 7.2 - 8.3
     - 4.4 - 5.4
   * - 2.5
     - 8.0 - 8.4
     - 5.4 - 6.4
   * - 2.6
     - 8.2 - 8.5
     - 5.4 - 7.4
   * - 3.0
     - 8.2 - 8.5
     - 5.4 - 7.4

The upgrade process for Sulu consists of the following steps:

1. Update the sulu/sulu package
-------------------------------

The ``sulu/sulu`` package implements the functionality of the Sulu content management system. To update this package, you must update the version constraint in your project's ``composer.json``.

To do this, replace the ``~x.x.x`` with a version constraint such as ``~2.5.22`` and execute the following
command in the root folder of your project:

.. code-block:: bash

    composer require sulu/sulu:"~x.x.x" --no-update

Afterward, update all project dependencies by running:

.. code-block:: bash

    composer update

.. note::

    See the `Composer documentation`_ for more information about version constraints.

2. Check sulu/skeleton repository changes
-----------------------------------------

The `sulu/skeleton repository`_ contains the project template for Sulu. This template may be updated
between versions to include configuration for new features or to maintain alignment with `Symfony best practices`_.
It is advised to examine the changes in the ``sulu/skeleton`` repository between the versions you are upgrading and
apply those that are relevant to your project.

This step cannot be automated, as changes in the ``sulu/skeleton`` repository may include breaking changes or might
not be applicable to your specific project.

.. note::

    For a convenient view of all changes in the skeleton repository, open https://github.com/sulu/skeleton/compare/
    and select your current version as ``base`` and the target version as ``compare``.

3. Check the UPGRADE.md file for BC breaks
------------------------------------------

The `UPGRADE.md file`_ in the ``sulu/sulu`` repository contains all changes breaking backwards compatibility
between versions. These changes might affect your application if you utilize the modified parts of Sulu
in a specific way.

In the majority of cases, these changes will not affect your project. However, if something goes wrong, this file should contain an explanation of
what needs to be changed.

4. Update the Admin JavaScript build
------------------------------------

The Sulu administration interface requires a built version of its JavaScript code in the ``public/build/admin`` folder. The JavaScript code may be updated between versions to fix bugs or implement new features.
When upgrading, you must update the build to match the new Sulu version.
To simplify this step, Sulu provides a command to update the JavaScript build:

.. code-block:: bash

    $ bin/console sulu:admin:update-build

.. note::

    Refer to the :doc:`../cookbook/build-admin-frontend` documentation if you want to update the
    JavaScript build without using the ``sulu:admin:update-build`` command.

.. _Composer documentation: https://getcomposer.org/doc/articles/versions.md#writing-version-constraints
.. _sulu/skeleton repository: https://github.com/sulu/skeleton
.. _Symfony best practices: https://symfony.com/doc/current/best_practices.html
.. _UPGRADE.md file: https://github.com/sulu/sulu/blob/2.x/UPGRADE.md
