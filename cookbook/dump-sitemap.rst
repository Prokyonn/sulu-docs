Improve Sitemap Speed
=====================

The Sulu sitemap is composed of small sections generated
by `SitemapProviders` (see :doc:`sitemap-provider`).
Each provider can return up to 50,000 links. Processing this many
links can take a significant amount of time, and the Google bot
will not wait long for a sitemap response.

To improve the speed of the sitemap page, Sulu provides a command to pre-generate
the page and cache it on the filesystem. This should be called via a cron job to keep the
pre-generated sitemap up to date.

.. note::

    This is a performance optimization for very large websites. In 99% of cases, this
    optimization is unnecessary, and the sitemap can be generated on the fly.

.. code-block:: bash

    php bin/websiteconsole sulu:website:dump-sitemap

If you use the ``{host}`` placeholder in your webspace URL
configuration, you must set the Symfony ``default_uri`` configuration option
to generate the URLs of your sitemap items via a command.
For more information, see the official Symfony Documentation on
`Generating URLs in Commands`_.

.. code-block:: yaml

    # config/packages/framework.yaml
    framework:
        router:
            default_uri: 'https://example.org'

.. tip::

    You can use ``%env(DEFAULT_URI)%`` to set this configuration
    via an environment variable.

If you are using a Symfony version before 5.1, you must configure the
`router context`_ parameters instead of the ``default_uri`` option:

.. code-block:: yaml

    # config/services.yaml
    parameters:
        router.request_context.scheme: 'https'
        router.request_context.host: 'example.org'

Switching back to on-the-fly generation
---------------------------------------

If you want to switch back to on-the-fly generation, you must
remove the existing pre-generated sitemaps from the ``var`` directory.

By default, pre-generated sitemaps are stored in the following directory in the
``prod`` environment:

.. code-block:: bash

    rm -rf var/cache/website/prod/sulu/sitemaps/

.. _router context: https://symfony.com/doc/4.4/routing.html#generating-urls-in-commands
.. _Generating URLS in Commands: https://symfony.com/doc/5.4/routing.html#generating-urls-in-commands
