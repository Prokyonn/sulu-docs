Adding a New Webspace
====================

To create a new webspace, create a new file within the
``config/webspaces`` directory. The content of the file should be
similar to the `website.xml`_ file in that folder.

.. note::

    The key of the webspace must be the same as the filename without the ``.xml``
    extension.

To activate the webspace within Sulu, clear the cache with the following commands:

.. code-block:: bash

    php bin/adminconsole cache:clear
    php bin/websiteconsole cache:clear

Afterward, initialize the new webspace by running the
following command:

.. code-block:: bash

    php bin/adminconsole sulu:document:initialize

.. note::

    To allow users to see the new webspace, you must also add the permissions for the
    webspace to the respective roles.

After these few steps, you will be able to administer and view your new webspace.

If you encounter any errors, you can use the following command to validate your webspace:

.. code-block:: bash

    php bin/adminconsole sulu:content:validate:webspaces

.. _website.xml: https://github.com/sulu/skeleton/blob/2.6/config/webspaces/website.xml
