Maintenance Mode
================

When you need to deploy a new version of your project to a production environment,
it is often necessary to disable your Sulu application and inform your users
about the downtime.

Sulu maintenance mode displays a simple holding page that can be easily customized.

Activating Maintenance Mode
---------------------------

Sulu is shipped with a simple maintenance page stored in the `public/maintenance.php`_
file, which can be customized to meet your needs.

To activate maintenance mode, set the environment variable ``SULU_MAINTENANCE`` to ``true``.
For example, in your ``.htaccess`` or vhost file for Apache:

.. code-block:: apache

    SetEnv SULU_MAINTENANCE true

For Nginx, you can configure maintenance mode in the PHP section of your vhost by adding:

.. code-block:: nginx

    fastcgi_param SULU_MAINTENANCE true;

Configuring Maintenance Mode
----------------------------

Allowed IP addresses
~~~~~~~~~~~~~~~~~~~~

You may want to access your application while maintenance mode is active. You can set the allowed IPs in ``public/maintenance.php``:

.. code-block:: php

    <?php
    $allowedIPs = ['127.0.0.1'];

Translations
~~~~~~~~~~~~

You can define translations for your template as follows:

.. code-block:: php

    <?php
    $translations = [
       'en' => [
          'title' => 'Maintenance',
          'heading' => 'The page is currently down for maintenance',
          'description' => 'Sorry for any inconvenience caused. Please try again shortly.',
       ],
    ];

Default locale
~~~~~~~~~~~~~~

By default, ``maintenance.php`` automatically detects your browser's language. If no translation exists for that language, the default locale is used. The default is English:

.. code-block:: php

    <?php
    define('DEFAULT_LOCALE', 'en');

.. _public/maintenance.php: https://github.com/sulu/skeleton/blob/2.x/public/maintenance.php
