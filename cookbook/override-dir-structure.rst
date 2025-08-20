How to Override Sulu's default Directory Structure
==================================================

As Sulu is based on Symfony, you can read about overriding the default directory structure in the `Symfony documentation`_.
Keep in mind that the cache folder for Sulu needs to be different for the website and admin contexts (Kernel::getContext()).

Overriding the Admin JS/CSS Build Base Path
-------------------------------------------

If you want to override not only the `public-dir`_ but also the path where the JS/CSS for the admin is built,
you need to change the following in your Webpack configuration:

.. code-block:: js

    const webpackConfig = require('./vendor/sulu/sulu/webpack.config.js');

    module.exports = (env, argv) => {
        if (!env) {
            env = {};
        }

        env.base_path = 'your/new/path';

        return webpackConfig(env, argv);
    };

Also, you need to tell the framework bundle where it will find the new ``manifest.json`` after you
have generated it with ``npm install`` and ``npm run build`` in your new directory.

.. code-block:: yaml

    # config/packages/framework.yaml
    framework:
        assets:
            packages:
                sulu_admin:
                    json_manifest_path: "%kernel.project_dir%/public/your/new/path/manifest.json"

.. _Symfony documentation: https://symfony.com/doc/current/configuration/override_dir_structure.html
.. _public-dir: https://symfony.com/doc/current/configuration/override_dir_structure.html#override-the-public-directory


Overriding the Templates Configuration Files Path
-------------------------------------------------

To use a directory other than the default ``config/templates/pages``, you need to create a ``sulu_core.yaml`` file in ``config/packages`` and add the following parameters.
(Subdirectories are not included by design. This allows for the use of subdirectories for other purposes, such as with ``<xi:include .../>``. See :doc:`../book/templates`.)

.. code-block:: yaml

    # config/packages/sulu_core.yaml
    sulu_core:
        content:
            structure:
                paths:
                    page_projectA:
                        path: '%kernel.project_dir%/config/templates/pages/projectA'
                        type: page
                    page_projectB:
                        path: '%kernel.project_dir%/config/templates/pages/projectB'
                        type: page

Alternatively, use the ``SITE`` environment variable for the active webspace.

.. code-block:: yaml

    # config/packages/sulu_core.yaml
    sulu_core:
        content:
            structure:
                paths:
                    page_site:
                        path: '%kernel.project_dir%/config/sites/%env(SITE)%/templates'
                        type: page



Overriding the Webspace Config File Path
----------------------------------------

.. code-block:: yaml

    # config/packages/sulu_core.yaml
    sulu_core:
        webspace:
            config_dir: '%kernel.project_dir%/config/sites/%env(SITE)%'

